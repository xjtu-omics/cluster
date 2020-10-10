# 如何使用集群ne 

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

点击左上角的“用户身份验证”，依次填写管理员提供的用户名和密码，点击确定，连接成功。
## 3. 集群概况
### 3.1 集群资源

| 属性 | Queue | nodes | memory | cpu | storage |
| ------ | ------ | ------ | ------| ------| ------ |
| 数量 | 2 | 15 | 11 | 640C | 676T |
### 3.2 集群拓扑结构
![Pandao editor.md](https://raw.githubusercontent.com/zhaohh52/cluster/main/pictures/struct.png "Pandao editor.md")
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
★ 一般使用队列batch，如需使用fat队列需提前告知有关老师；   
★ 删除文件数量较多时，建议分批删除，避免I/O错误，若删除文件较大时，及时通知管理员释放回收站；   
★ 资源按需申请，避免浪费；   
★ 多用压缩文件或二进制文件（fq.gz,bam）；   
★ 多用管道少保留中间文件；    
★ 不用的数据及时删除。 

