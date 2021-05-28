★ 刘工程师，电话： ，微信：lq512524919   

## 日期：5月28日    
日志：三个队列batch、fat、gpu 提交作业均显示排队状态   
具体描述：提交作业后作业不执行，一直处于Q状态
解决办法：作业提交前管理员重新做了资源分配，即三个队列中节点的分配，做完后没有刷新pbs服务；
         在manager节点执行clush -b -w node[1-14],gpu1,fat1 'df -h |grep data' 查看挂载是否正常，显示正常   
         执行：clush -b -w node[1-14],gpu1,fat1 systemctl restart pbs_mom重启pbs_mom服务
         执行systemctl restart pbs_server，systemctl restart maui，systemctl restart trqauthd.service重启manager上的服务
         提交任务正常。



