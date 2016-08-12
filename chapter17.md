#ubuntu下使用rsync实现文件同步

实验环境：ubuntu   
主服务器：192.168.1.103  
从服务器：192.168.1.105 (做备份用)  
实验用户：root   

### 1  rsync的安装
更新   

    apt-get update
安装依赖工具包

    apt-get install build-essential
安装

    apt-get  install  rsync
rsync 服务器架设比较简单，可能我们安装好rsync后，并没有发现配置文件，以及rsync服务器启动程序，因为每个管理员可能对rsync 用途不一样，所以一般的发行版只是安装好软件就完事了，让管理员来根据自己的用途和方向来自己架设rsync服务器；
 
###2  rsync服务器的配置文件rsyncd.conf
    [root@linuxsir:~]#mkdir /etc/rsyncd  
注：在/etc目录下创建一个rsyncd的目录，我们用来存放rsyncd.conf 和rsyncd.secrets文件；

    [root@linuxsir:~]#touch /etc/rsyncd/rsyncd.conf  
注：创建rsyncd.conf ，这是rsync服务器的配置文件；

    [root@linuxsir:~]#touch /etc/rsyncd/rsyncd.secrets  
注：创建rsyncd.secrets ，这是用户密码文件；

    [root@linuxsir:~]#chmod 600 /etc/rsyncd/rsyncd.secrets  
注：为了密码的安全性，我们把权限设为600；
 
    [root@linuxsir:~]#ll /etc/rsyncd/rsyncd.secrets 
    -rw------- 1 root root 14 2007-07-15 10:21 /etc/rsyncd/rsyncd.secrets
    [root@linuxsir:~]#touch /etc/rsyncd/rsyncd.motd
下面我们修改 rsyncd.conf 和rsyncd.secrets 和rsyncd.motd 文件；
rsyncd.conf 是rsync服务器主要配置文件，我们来个简单的示例；比如我们要备份服务上
的 /usr/limu/  ，在/usr/limu/ 中，我想把beinan 和 samba 目录排除在外；

    #vim rsyncd.conf：

    pid file = /var/run/rsyncd.pid   
    port = 873
    address = 192.168.1.103
    #uid = nobody 
    #gid = nobody    
    uid = root   
    gid = root  
    use chroot = yes  
    read only = yes 

    #limit access to private LANs
    hosts allow=192.168.1.105/255.255.255.0 10.0.1.0/255.255.255.0  
    hosts deny=*
    max connections = 5 
    motd file = /etc/rsyncd/rsyncd.motd
    #This will give you a separate log file
    log file = /var/log/rsync.log
    #This will log every file transferred - up to 85,000+ per user, per sync
    transfer logging = yes
    log format = %t %a %m %f %b
    syslog facility = local3
    timeout = 300
    [datatime]   //模块名,自己定义，注意后面执行命令时要一致  
    path =/usr/limu  
    list=yes 
    ignore errors 
    auth users = ubuntu    #auth users是必须在服务器上存在的真实的系统用户,
    #如果你想用多个用户，那就以,号隔开；比如 auth users = beinan , linuxsir 
    secrets file = /etc/rsyncd/rsyncd.secrets  
    comment = linuxsir tmp  
    exclude =   beinan/  samba/     
 
修改rsyncd.secrets  密码文件
 
    #vim rsyncd.secrets  密码文件

    ubuntu:123456

rsyncd.motd是定义rysnc 服务器信息的，也就是用户登录信息。比如让用户知道这个服务器是谁提供的等；类似ftp服务器登录时，我们所看到的 linuxsir.org ftp ……。 当然这在
全局定义变量时，并不是必须的，你可以用#号注掉，或删除；我在这里写了一个rsyncd.motd的内容为：

    #vim rsyncd.motd
    +++++++++++++++++++++++++++++++++++++++++++++++++++++
             Welcome to DateTime rsync services  
                 2015--2016 create by limu    
    ++++++++++++++++++++++++++++++++++++++++++++++++++++ 

###3  启动rsync服务器

    #/usr/bin/rsync --daemon  --config=/etc/rsyncd/rsyncd.conf

如果遇到报错为 Failed to Create pid file /var/run/rsyncd.pid:file exists 
解决方法：

    #ps ax|grep rsync //查看进程id
    3033    ?        S      0:00 /usr/bin/rsync --no-detach --daemon --config
    /etc/rsyncd.conf
    4360 pts/1    S+     0:00 tail -f rsync.log
    4786 pts/0    D+     0:00 grep rsync
    #kill 3033
重启服务 

    #/usr/bin/rsync --daemon  --config=/etc/rsyncd/rsyncd.conf
 
 

###4 rsync 客户端同步数据客户端只需要安装rsync即可，不需要特别配置
客户端执行：

    rsync -vzrtopg --delete --progress ubuntu@192.168.1.103::datatime /usr/limu
输入密码：123456   
![](./截图/1.png)
ubuntu是指定密码文件中的用户名   
::linuxsirhome 表示在rsyncd.conf中设置的模块名  
/usr/limu是备份到本地的目录名

###5 例行脚本

    vim rsync.sh
    
    #!/bin/bash  
    #每秒执行同步备份文件          
    while true         
    do
    DATE=$(date +%d-%X)
    time=${DATE%' '*}
    rsync -vzrtopg --delete --progress --password-file=/usr/rsyncd/rsyncd.secrets 
    ubuntu@192.168.1.103::datatime /usr/limu >/var/log/rsync.$time 
    sleep 1
    done
给脚本执行权限：

    chmod +x /usr/rsyncd/rsync.sh
创建执行脚本需要的密码文件，在/usr/rsyncd/目录下创建rsyncd.secrets ，里面只有服务端ubuntu用户的密码:
    
    root@ubuntu3:/usr/rsyncd# cat rsyncd.secrets 
    123456
修改密码文件权限，不然会报如下错：
![](./截图/2.png) 

解决方法：

    chmod 600 /usr/rsyncd/rsyncd.secrets
该脚本需要跑在后台而且不要有输出信息,命令如下：

    ./rsync.sh >/dev/null & 
执行后会只返回进程号：
![](./截图/3.png)
 
常见问题与解决办法：
问题一：

    @ERROR: chroot failed
    rsync error: error starting client-server protocol (code 5) at main.c(1522) [receiver=3.0.3]

原因：
服务器端的目录不存在或无权限，创建目录并修正权限可解决问题。

问题二：

    @ERROR: auth failed on module tee
    rsync error: error starting client-server protocol (code 5) at main.c(1522) [receiver=3.0.3]

原因：
服务器端该模块（tee）需要验证用户名密码，但客户端没有提供正确的用户名密码，认证失败。
提供正确的用户名密码解决此问题。

问题三：
 
    @ERROR: Unknown module ‘tee_nonexists’
    rsync error: error starting client-server protocol (code 5) at main.c(1522) [receiver=3.0.3]

原因：
服务器不存在指定模块。提供正确的模块名或在服务器端修改成你要的模块以解决问题。
问题1：
在client上遇到问题：

    rsync -auzv –progress –password-file=/etc/rsync.pas root@192.168.1.128::backup /home/
    rsync: could not open password file “/etc/rsync.pas”: No such file or directory (2)
    Password:
    @ERROR: auth failed on module backup
    rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
遇到这个问题：client端没有设置/etc/rsync.pas这个文件，而在使用rsync命令的时候，加了这个参数–
password-file=/etc/rsync.pas

问题2：

    rsync -auzv –progress –password-file=/etc/rsync.pas root@192.168.1.128::backup /home/
    @ERROR: auth failed on module backup
    rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
遇到这个问题：client端已经设置/etc/rsync.pas这个文件，里面也设置了密码111111，和服务器一致，但是
服务器段设置有错误，服务器端应该设置/etc/rsync.pas ，里面内容root:111111 ,这里登陆名不可缺少

问题3：

    rsync -auzv –progress –password-file=/etc/rsync.pas root@192.168.1.128::backup /home/
    @ERROR: chdir failed
    rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
遇到这个问题：是因为服务器端的/home/backup 其中backup这个目录并没有设置，所以提示：chdir failed

问题4：

    rsync: write failed on “/home/backup2010/wensong”: No space left on device (28)
    rsync error: error in file IO (code 11) at receiver.c(302) [receiver=3.0.7]
    rsync: connection unexpectedly closed (2721 bytes received so far) [generator]
    rsync error: error in rsync protocol data stream (code 12) at io.c(601) [generator=3.0.7]
遇到这个问题：磁盘空间不够，所以无法操作。可以通过df /home/backup2010 来查看可用空间和已用空间

