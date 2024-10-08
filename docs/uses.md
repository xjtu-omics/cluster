# 如何使用集群

## 1. 账号申请 
联系管理员创建账号密码（如zhangsan/123456）
## 2. 远程客户端及传输软件安装
### 2.1 下载及安装
推荐Xshell和Xftp  
下载地址：https://www.netsarang.com/zh/free-for-home-school/
### 2.2 连接
以Xshell为例，打开软件,点击左上角 ‘新建’，依次填写名称、主机及端口号

| 名称 | 主机 | 端口号 |
| ------ | ------ | ------ |
| 自定义 | 192.168.20.4 | 22 |
| 自定义 | 10.181.7.169 | 65533 |

点击左上角的“用户身份验证”，依次填写管理员提供的用户名和密码，点击确定，连接成功。   
Xftp类同。
## 3. 集群概况
### 3.1 集群资源

兴庆集群

| 属性 | Queue | nodes | cpu | storage |
| ------ | ------ | ------ | ------| ------ |
| 数量 | 2 | 15 | 640C | 476T |   

创新港集群
| 属性 | Queue | nodes | cpu | storage |
| ------ | ------ |------| ------| ------ |
| 数量 | 2 | 16 | 1000C | 1300T | 

queue目前有两个：batch和fat，fat的内存较大、CPU核数较多。   

### 3.2 集群拓扑结构

兴庆集群
 
![Image](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/struct.png)   
创新港集群

![Image](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/cxg_cluster.png)   
## 4. 集群操作
### 4.1 集群常用命令
```

  vnodes  # 查看集群各节点资源   
  qnodes  # 查看节点上的队列信息    
  pestat  # 查看各节点当前状态   
  showq   # 查看队列   
  
```
### 4.2 linux常用命令
```

  df -Th     # 查看磁盘大小   
  ls -a      # 显示当前文件夹下所有文件，包含.files    
  less file  # 浏览文件file  
  free -g    # 查看机器内存情况
  top        # 显示系统中各个进程的资源占用状况  
  
```
## 5. 任务操作
### 5.1 新建任务
pbs脚本示例说明   
```

#!/bin/bash

#PBS -N <jobname>
#PBS -q <queuename>
#PBS -d <workdir>
#PBS -o <path_outfile>
#PBS -e <path_errfile>
#PBS -l nodes=<num_n>:ppn=<num_p>,walltime=<hh:mm:ss>

<the content>

For example:
#!/bin/bash

#PBS -N test_1
#PBS -q batch
#PBS -l nodes=1:ppn=24,walltime=24:00:00

<the content>

```   
说明：
``` 
<jobname>		表示作业名称，必填  
<queuename>		表示 队列名称（当前集群已设置的队列：batch），必填    
<workdir>		表示 工作路径     
<hh:mm:ss>		表示 标准输出文件路径及名称    
<path_errfile>		表示 错误输出文件路径及名称   
<num_n>			表示 节点数量，必填    
<num_p>			表示 核心数量，必填      
<hh:mm:ss>		表示 作业运行时间   
<the content>		表示 需要执行的命令、程序，必填   
``` 
### 5.2 提交任务
```
qsub MyPBS.pbs
```

### 5.3 查看任务
```
qstat -an
```
### 5.4 删除任务
```
qdel YourJobID
```

## 6. 误删恢复   
★ 兴庆集群回收站保存时间为24小时，创新港集群保存时间为7*24小时。  
创新港集群恢复操作   
![Image](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/recycle.png)     
兴庆集群联系管理员   
## 7. 集群使用注意事项
★ 个人账号只限个人使用，严谨将账号和密码泄露给外人；   
★ 禁止在登陆节点（创新港manager、兴庆mu01）上直接运行程序，如造成节点故障首次惩罚一周禁用集群，再次一个月禁用集群；   
★ 禁止直接进入计算节点执行任务，会影响作业调度，如有发现，管理员立即终止任务，违规者首次惩罚一周禁用集群，再次一个月禁用集群；    
★ 严谨使用集群进行项目无关的任何活动和行为；  
★ 使用snakemake提交作业时，禁止调度所有节点，必须空出至少1/2的计算节点资源；   
★ 人员发生调动时，请调动人员做好数据备份和移交，管理员及时清理；   
★ 私有数据和程序需存放在用户目录下，不能放在共有目录或他人目录下；   
★ 注意程序生成文件的路径，务必放在自己的目录下，如果不慎放在其他目录，及时删除；   
★ 资源按需申请，避免浪费，熟悉程序参数（Thread）；     
★ 不得随意创建定时任务；         
★ 如执行vnodes显示资源充足，但是提交作业后一直处于排队状态，联系管理员刷新资源管理器，释放资源；    
★ 如果作业运行时间较长，定时检查自己的作业是否有卡死或者极大消耗内存的问题，并及时删除有问题的作业；   
★ 批量提交作业时，先少量测试，没有问题再大量提交；   
★ 多用压缩文件或二进制文件（fq.gz,bam）；      
★ 多用管道少保留中间文件；      
★ 不用的数据及时删除；    
★ 如误删文件，及时联系管理员恢复；   
★ 如删除文件较大，可将需删除的文件mv到用户所在目录的WillDelete文件夹中，管理员负责定时清理；   
★ 删除文件数量较多时，建议分批删除，避免I/O错误，若删除文件较大时，及时通知管理员释放回收站；      
★ 由于资源紧张（兴庆），垃圾回收站只保留24小时内的文件，如有误删操作，及时联系管理员恢复；    
★ manager上安装软件显示rm问题造成安装失败问题，建议进入login节点进行安装（回收站程序问题）。
