# 服务器端设置

在这一章中我们将介绍各种Ubuntu server中的常见的服务器设置工作



##二 添加Python源

Ubuntu下安装pip的方法
 
安装pip的方法：

Install pip and virtualenv for Ubuntu 10.10 Maverick and newer
```
$ sudo apt-get install python-pip python-dev build-essential 
$ sudo pip install --upgrade pip 
$ sudo pip install --upgrade virtualenv 
```
For older versions of Ubuntu
```
Install Easy Install
$ sudo apt-get install python-setuptools python-dev build-essential 
Install pip
$ sudo easy_install pip 
Install virtualenv
$ sudo pip install --upgrade virtualenv 
``` 

```
安装mysql拓展包可以：sudo pip install mysql-python
安装其他的包只要pip + 包的名字 就行了 很方便。而且安装完的mysqlib用起来没一点问题,
在windows下还要修改下源文件。写程序还是在linux下好啊。
安装mysql 
apt-get install mysql-server
```


##三 安装Python MySQL

```
Ubuntu下安装pip的方法
安装pip的方法：
Install pip and virtualenv for Ubuntu 10.10 Maverick and newer
$ sudo apt-get install python-pip python-dev build-essential
$ sudo pip install –upgrade pip
$ sudo pip install –upgrade virtualenv
For older versions of Ubuntu
Install Easy Install
$ sudo apt-get install python-setuptools python-dev build-essential
Install pip
$ sudo easy_install pip
Install virtualenv
$ sudo pip install –upgrade virtualenv


安装mysql拓展包可以：

sudo pip install mysql-python
sudo apt-get install python-dev
#apt-get install python-devel 
apt-get install mysql-server
apt-get install libmysqld-dev libmysqlclient-dev 
```

##四 JDK配置
### Step1. 下载
```
上 Oracle 下载 Linux 下的 JDK 压缩包，我下载的是 jdk-7u25-linux-x64.tar.gz
```
### Step2. 解压
```
解压，并将解压后文件复制到 /usr/lib/jvm 中
tar -zxvf jdk-7u25-linux-x64.tar.gz
sudo cp -r ~/Downloads/jdk1.7.0_25/ /usr/lib/jvm/
```
### Step3. 配置环境变量
```
sudo gedit ~/.profile
文件的最后一行末尾添加：
export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_25
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
保存并关闭，在 terminal 中输入 source ~/.profile 更新，使用 env 命令查看 JAVA_HOME 的值是否为：“/usr/lib/jvm/jdk1.7.0_25” 如果是的话，说明配置成功
```

### Step4. 修改系统默认的 jdk
```
系统默认的 jdk 是 open jdk，这里我们把它改成 sun 公司的 jdk

sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java/bin/java 300  
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/java/bin/javac 300  
sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/java/bin/jar 300   
sudo update-alternatives --install /usr/bin/javah javah /usr/lib/jvm/java/bin/javah 300   
sudo update-alternatives --install /usr/bin/javap javap /usr/lib/jvm/java/bin/javap 300
sudo update-alternatives --config java
sudo update-alternatives --config javac
在上面的步骤中，如果遇见系统要求你选择，选择 jdk1.7.0_25 前对应的选项即可
最后查看以下 java 的信息：java -version，我的如下：

java version "1.7.0_25"
Java(TM) SE Runtime Environment (build 1.7.0_25-b15)
Java HotSpot(TM) 64-Bit Server VM (build 23.25-b01, mixed mode)
出现上面的信息，表明已经安装成功了
```




