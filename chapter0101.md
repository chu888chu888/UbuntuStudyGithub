# 1.更新源

##一 更新源

ubuntu server 官方的源代码更新比较慢，所以更替成国内的源。操作如下：

```
piaoyun@Ubuntu:~$ sudo cp /etc/apt/sources.list /etc/apt/sources.list.apt
piaoyun@Ubuntu:~$ sudo vim /etc/apt/sources.list
或者使用nano编辑：
piaoyun@Ubuntu:~$ sudo nano /etc/apt/sources.list
ctrl+o   #保存配置
ctrl+x   #退出编辑
```

常用的国内源

```
阿里云源：
Trusty(14.04)
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse 

Quantal(12.10)
deb http://mirrors.aliyun.com/ubuntu/ quantal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ quantal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ quantal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ quantal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ quantal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ quantal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ quantal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ quantal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ quantal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ quantal-backports main restricted universe multiverse

Precise(12.04)
deb http://mirrors.aliyun.com/ubuntu/ precise main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ precise-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ precise-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ precise-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ precise-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ precise main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ precise-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ precise-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ precise-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ precise-backports main restricted universe multiverse

Oneiric(11.10)
deb http://mirrors.aliyun.com/ubuntu/ oneiric main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ oneiric-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ oneiric-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ oneiric-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ oneiric-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ oneiric main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ oneiric-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ oneiric-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ oneiric-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ oneiric-backports main restricted universe multiverse

Natty(11.04)
deb http://mirrors.aliyun.com/ubuntu/ natty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ natty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ natty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ natty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ natty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ natty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ natty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ natty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ natty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ natty-backports main restricted universe multiverse

163源：
Trusty(14.04)
deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse 

Quantal(12.10)
deb http://mirrors.163.com/ubuntu/ quantal main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ quantal-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ quantal-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ quantal-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ quantal-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ quantal main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ quantal-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ quantal-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ quantal-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ quantal-backports main restricted universe multiverse

Precise(12.04) 
deb http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ precise-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ precise-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse 

Oneiric(11.10) 
deb http://mirrors.163.com/ubuntu/ oneiric main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ oneiric-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ oneiric-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ oneiric-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ oneiric-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ oneiric main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ oneiric-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ oneiric-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ oneiric-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ oneiric-backports main restricted universe multiverse

Natty(11.04)
deb http://mirrors.163.com/ubuntu/ natty main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ natty-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ natty-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ natty-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ natty-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ natty main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ natty-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ natty-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ natty-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ natty-backports main restricted universe multiverse

sohu源：
Trusty(14.04)
deb http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse

Quantal(12.10) 
deb http://mirrors.sohu.com/ubuntu/ quantal main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ quantal-security main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ quantal-updates main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ quantal-proposed main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ quantal-backports main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal-security main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal-updates main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal-proposed main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ quantal-backports main restricted universe multiverse

Precise(12.04)
deb http://mirrors.sohu.com/ubuntu/ precise main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ precise-security main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ precise-updates main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ precise-proposed main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ precise-backports main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ precise main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ precise-security main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ precise-updates main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ precise-proposed main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ precise-backports main restricted universe multiverse

Oneiric(11.10)
deb http://mirrors.sohu.com/ubuntu/ oneiric main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ oneiric-security main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ oneiric-updates main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ oneiric-proposed main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ oneiric-backports main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ oneiric main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ oneiric-security main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ oneiric-updates main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ oneiric-proposed main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ oneiric-backports main restricted universe multiverse

Natty(11.04) 
deb http://mirrors.sohu.com/ubuntu/ natty main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ natty-security main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ natty-updates main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ natty-proposed main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ natty-backports main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ natty main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ natty-security main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ natty-updates main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ natty-proposed main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ natty-backports main restricted universe multiverse
```