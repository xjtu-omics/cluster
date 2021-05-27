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
| 自定义 | *.*.*.* | 22 |

点击左上角的“用户身份验证”，依次填写管理员提供的用户名和密码，点击确定，连接成功。   
Xftp类同。
## 3. 集群概况
### 3.1 集群资源

| 属性 | Queue | nodes | memory | cpu | storage |
| ------ | ------ | ------ | ------| ------| ------ |
| 数量 | 2 | 15 | 11 | 640C | 476T |

queue目前有两个：batch和fat，fat的内存较大。   

### 3.2 集群拓扑结构
![Pandao editor.md](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/struct.png "Pandao editor.md")
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
pbs文件实例，cat MyPBS.pbs
```

#!/bin/bash

### Set job name，for example MyPBS  
#PBS -l walltime=9000:00:00
#PBS -V
#PBS -N MyPBS

### set output files
#PBS -o MyPBS.stdout       
#PBS -e MyPBS.stderr     

### set queue name  
#PBS -q batch

###set number of nodes,for example one node with 4 cpu.And you can specify the node: #PBS -l nodes=cu01:ppn=4 

#PBS -l nodes=1:ppn=4 
#the following is you own code
samtools index pb.map.bam
…..

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
## 6. 集群使用注意事项
★ 个人账号只限个人使用，严谨将账号和密码泄露给外人；   
★ 严谨使用集群进行项目无关的任何活动和行为；   
★ 人员发生调动时，请调动人员做好数据备份和移交，管理员及时清理；   
★ 私有数据和程序需存放在用户目录下，不能放在共有目录或他人目录下；   
★ 注意程序生成文件的路径，务必放在自己的目录下，如果不慎放在其他目录，及时删除；   
★ 资源按需申请，避免浪费；     
★ 不得随意创建定时任务；   
★ 一般使用队列batch，如需使用fat队列需提前告知相关老师；      
★ 如执行vnodes显示资源充足，但是提交作业后一直处于排队状态，联系管理员刷新资源管理器，释放资源；    
★ 如果作业运行时间较长，定时检查自己的作业是否有卡死或者极大消耗内存的问题，并及时删除有问题的作业；   
★ 批量提交作业时，先少量测试，没有问题再大量提交；   
★ 多用压缩文件或二进制文件（fq.gz,bam）；      
★ 多用管道少保留中间文件；      
★ 不用的数据及时删除；    
★ 如误删文件，及时联系管理员恢复；   
★ 如删除文件较大，可将需删除的文件mv到用户所在目录的WillDelete文件夹中，管理员负责定时清理；   
★ 删除文件数量较多时，建议分批删除，避免I/O错误，若删除文件较大时，及时通知管理员释放回收站；      
★ 由于资源紧张（兴庆），垃圾回收站只保留24小时内的文件，如有误删操作，及时联系管理员恢复。   
