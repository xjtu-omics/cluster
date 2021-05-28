★ 浪潮马甜工程师联系方式，电话：16109530270，微信：familytomorrow   

## 日期：8月12日    
日志：cu01节点挂在失败   
具体描述：cu01节点的共享目录/opt /home挂载失败，原因是cu01节点的IB卡没插好，重新插好后重新mount/opt和/home目录，具体路径详见/etc/fstab   
                执行：mount -t nfs -o vers=3 ibmu01:/opt /opt   
	              mount -t nfs 10.10.10.234:/. /home -o vers=3   
	              df -Th 查看挂载是否成功

## 日期：8月18日 
日志：个人用户删除文件失败，该问题从3月份开始出现   
具体描述：用户在当前路径下创建的文件删除操作不允许，提示“rm: cannot remove ‘f2’: Operation not permitted”，临时方法是将需要删除的文件move到垃圾回收站，管理员在存储节点删除。   
         	联系浪潮的工作人员，定位到问题是存储版本问题，需要升级或者安装补丁。   
		目前问题还没有解决，浪潮正在评估方案。   

## 日期：8月20日 
日志：与浪潮沟通回收站升级   
具体描述：解决8.18号描述的问题   
解决办法： 升级回收站，定于8.21号上午10点现场升级。   

## 日期：8月21日 
日志：回收站现场升级   
具体描述：浪潮工作人员升级icfs-3.7.16.18，升级成功后重启五台存储节点，重启ganesha服务（systemctl restart ganesha）   
	进入inspur01节点：cd /root/icfs-3.7.16.18     
	执行: sh icfs_update.sh  

## 日期：8月27日 
日志：cu02节点down  
具体描述：集群显示cu02节点down，ssh连接失败  
	机房开机解决，状态变为free。  
## 日期：8月27日    
日志：xixizhao job提交后一直显示Q状态     
具体描述：集群中的节点利用率目前较高，没有节点有20核的cpu，因此无法调度。      
解决办法：调整申请资源，job状态变为R。         
 
## 日期：8月28日 
日志：集群中的节点时间不同步  
具体描述：没有实时发起时间同步事件，导致重启或者长时间后时间不一致。     
解决办法：创建新的分组和定时任务，实时与mu01节点同步。/root/mylib/ntpdate.sh     
补充：    crontab -e 编辑定时任务 crontab -l 查看定时任务 查看定时任务日志：路径/var/log/cron（例如：cat /var/log/cron | grep /root/mylib/clear-cache.sh）     

## 日期：8月31日 
日志：定期清理缓存    
具体描述：写定时程序，每天1点清理缓存。/root/mylib/clear-cache.sh  
修改： 10.12日修改，每10分钟清理一次缓存。   

## 日期：9月3日
日志：在资源充足的情况下，只要前面作业在排队，后边的作业就处于排队状态无法执行，showq有反应。       
具体描述：xixizhao再batch队列上提交作业处于Q状态，pengjia在fat上提交作业（资源充足）处于排队状态，无法执行。maui是任务调度的服务，重启刷新一下就好了，可能之前有任务运行完，这个服务记录的信息没有更新。        
解决办法: 在执行mu01上执行：service maui restart       

## 日期：9月7日   
日志：too many open files   
具体描述：与内核参数有关。   
解决办法：修改etc/security/limits.conf和/etc/security/limits.d/目录下文件（vim /etc/security/limits.d/def.conf）的参数，，* soft nofile 655350 及* hard nofile 655350，重新连接用户即可。ulimit -n查看当前open files数量。  

## 日期：9月8日    
日志：作业提交后一直处于排队状态，资源充足，showq无反应。    
具体描述：maui服务虽然处于active状态，但实际已经不工作，状态active(exited),并提示某个文件路径找不到。     
解决办法：修改/etc/profile 文件并生效成功。另外联系浪潮的工程师，其添加了/etc/profile 中有关TORQUE_HOME的环境变量。     

## 日期：9月9日
日志：作业提交后一直处于排队状态，资源充足，showq有反应。   
具体描述：可能处于用户占用的资源，没彻底释放完的原因，定期执行 /opt/job/jobcheck.sh任务来释放资源。   

## 日期：9月14日
日志：误删文件找回。    
具体描述：用户删除文件后，文件将存放在回收站24小时，管理员可手动恢复。    
解决办法：在inspur01节点上进入/run/icfs_client_backend/system/.recycle/目录，找到文件，mv至/run/icfs_client_backend/share/指定目录下。    

## 日期：9月18日  
日志：浪潮工程师例行磁盘检查   
具体描述：远程，主要查看了存储节点icfs版本和磁盘空间。   

## 日期：9月21日
日志：cu01和cu06 down  
具体描述：节点ssh进入之后，卡顿，发现jdlin的进程都dead了   
解决办法：kill jdlin的任务，重启ganesha服务   

## 日期：9月22日到9月24日   
日志：mu01节点卡顿，所有无法使用，存储节点inspur05节点ganesha服务异常。   
具体描述：mu01节点df -Th查看存储使用率以达到86%，可能是导致管理节点mu01卡顿的原因，其次，mu01上的内存使用紧张，也是mu01卡顿的原因，jdlin并发删除操作比较大，对mu01和存储节点的i/o有很大影响。
解决办法：清理mu01节点缓存，清理存储节点垃圾回收站。   

## 日期：10月13日
日志：计算节点/opt目录磁盘占用近100%。    
具体描述：集群卡顿，发现计算节点/opt目录磁盘占用近100%。   
解决办法：mu01节点目录 /opt/tsce4/torque6/share/下执行du -d 1 -h，作业缓存占用较大的文件，沟通提交作业者，删除。    

## 日期：10月19日 
日志：向fat01提交自作业失败   
具体描述：pestat显示fat01 down,ssh fat01 显示ssh: connect to host fat01 port 22: No route to host。         
解决办法：去机房手动重启fat01节点，重启之后挂载/home目录，（/etc/rc.local 文件里有记录:mount -t nfs -o vers=3 10.10.10.235:/. /home）
根本问题：为什么节点会无故down？   
   
## 日期：11月3日 
日志：fat01down  
具体描述：pestat显示fat01 down,ssh fat01 显示ssh: kernel:BUG: soft lockup - CPU#50 stuck for 22s! [migration/67:539]         
解决办法：reboot重启之后挂载/home目录，（/etc/rc.local 文件里有记录:mount -t nfs -o vers=3 10.10.10.235:/. /home）
根本问题：为什么节点会无故down？ 提交作业出现软锁。
      
## 日期：11月10日 
日志：cu11 down  
具体描述：mu01 可以ping通，但是ssh慢       
解决办法：预计是内存满，卡顿，需要释放，ssh cu11 echo 3 > /proc/sys/vm/drop_caches解决   
   
## 日期：11月16日 
日志：jdlin 的一个任务qdel删除不了，前提是已经运行很长时间。  
具体描述：任务在cu04上，进入节点后，kill相应进程杀不掉，查看具体进程发现是僵尸进程     
解决办法：在mu01上，执行qdel -p 强制删除，资源释放。

## 日期：5月15日 
日志：mu01节点根目录占用100%,作业无法提交
解决办法：日志文件较大，占用了内存，使用du -h -d 1 追踪到目录/opt/tsce4/torque6/share/下，删除较大日志即可.   

## 日期：5月26日 
日志：fat01节点ssh不进去，可以ping通，pestat结果显示down   
解决办法：cat   /etc/hosts  |grep  fat01；  ipmitool    -I   lanplus    -H    11.11.12.102   -U   admin   -P   admin    power  off   ；ipmitool    -I   lanplus    -H    11.11.12.102   -U   admin   -P   admin    power  on。   


