★ 售后：刘工程师  电话：--  微信：lq512524919   

## 日期：5月28日    
日志：三个队列batch、fat、gpu 提交作业均显示排队状态   
具体描述：提交作业后作业不执行，一直处于Q状态
解决办法：作业提交前管理员重新做了资源分配，即三个队列中节点的分配，做完后没有刷新pbs服务；
         在manager节点执行clush -b -w node[1-14],gpu1,fat1 'df -h |grep data' 查看挂载是否正常，显示正常   
         执行：clush -b -w node[1-14],gpu1,fat1 systemctl restart pbs_mom重启pbs_mom服务
         执行systemctl restart pbs_server，systemctl restart maui，systemctl restart trqauthd.service重启manager上的服务
         提交任务正常。

  ## 日期：5月31日    
日志：集群失联   
具体描述：ssh manager节点无反应，可以ping通,sbwang用户的进程占了大量的cpu和内存导致manager节点死机，重启后发现进程仍然存在，不久后manager节点又死机，其他计算节点也有相同问题，所有节点上sbwang有定时任务。   
解决办法：重启manager和login节点后注意重启服务和重新挂载，删除每节点上sbwang的定时任务（需su到sbwang用户），删除sbwang用户的所有进程（killall -u sbwang），删除sbwang用户目录下的所有可疑文。                 截图如下：   
         sbwang有很多accepting connections进程消耗了很多CPU和内存
![Pandao editor.md](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/PID.png "Pandao editor.md")   
         sbwang用户下的定时任务（每个节点上都有）   
![Pandao editor.md](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/crontab.png "Pandao editor.md")   
           
![Pandao editor.md](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/crontab1.png "Pandao editor.md")    
         进程删除后立马重现   
![Pandao editor.md](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/nodes.png "Pandao editor.md")    


## 日期：7月1日    
日志：pestat 显示fat1 dowm   
具体描述：pestat 显示fat1 dowm,ssh无反应      
解决办法：在manager节点上重启pbs_server服务。   

## 日期：7月2日    
日志：众多计算节点根目录使用率超过85%    
具体描述：由于有用户操作docker，docker的overlay2目录在计算节点的根目录挂载，目录大小50G，容器启动后很容易沾满。     
解决办法：执行：docker system df 查看docker磁盘使用情况，执行docker system prune -a删除镜像及日志。   
![Pandao editor.md](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/storage.png "Pandao editor.md")    

## 日期：7月7日    
日志：node1和node2节点手动释放内存失败（echo 3 > /proc/sys/vm/drop_caches）   
具体描述：node1和node2在无作业、无进程运行情况下，内存占用如下图所示。   
![Pandao editor.md](https://raw.githubusercontent.com/xjtu-omics/cluster/main/pictures/node1.png "Pandao editor.md")    
解决办法：1）应该是之前有多个进程使用了共享内存，这些进程停止或者异常停止后，内存未能正常释放，导致这些共享内存一直处于被占用状态，无法正常使用；    
         2）被共享的内存无法被drop_caches处理，所以没法手动释放。
         最终reboot解决，切记reboot之后重新挂载/data目录。    
        
## 日期：7月12-7月13日    
日志：ssh oss1 无反应   
具体描述：多次开关机后,ping不通oss1.   
解决办法：进机房收集硬件日志：将笔记本连接到交换机，配置一个192.168.100字段的IP，火狐浏览器进入地址：https://192.168.100.21 ,账号密码：admin/Password@_ 进入之后点击“一键收集”--》“下载全部日志”，交给兴华三硬件处理。最终问题是阵列卡有问题，换取硬件。    
