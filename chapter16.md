#redmine迁移：

###准备：
首先将volume中的数据迁移到对应相同的目录里，将redmine中以下目录中的所有数据拷贝到新的机器中对应的目录中
	/srv/docker/redmine/postgresql
	/srv/docker/redmine/redmine

举例来说：

	scp -r /srv/docker/redmine/redmine chu888chu888@192.168.1.2:/srv/docker/redmine

###实施：
redmine有官方和非官方的镜像，由于便于安装，本例使用的是dockerhub中使用最多的非官方镜像——sammersbn/redmine，详情请参考[sameersbn/redmine](https://hub.docker.com/r/sameersbn/redmine/ "sameersbn/redmine")


1、确保Linux机器中已经安装过docker，若没有安装，则执行命令

	wget -qO- https://get.docker.com/ | sh

具体docker的安装方法请参考官方文档[docker install](https://docs.docker.com/engine/installation/ "docker install")，本例不作详细说明

2、pull镜像，可以在镜像名后加上所需版本号

	docker pull sameersbn/redmine:3.1.4-6

3、生成容器

方法一：使用docker-compose命令快速安装（推荐使用）

	wget https://raw.githubusercontent.com/sameersbn/docker-redmine/3.1-stable/docker-compose.yml
	
	docker-compose up


方法二：分别手动运行容器
	

(1)运行postgresql容器

    docker run --name=postgresql-redmine -d \
    --env='DB_NAME=redmine_production' \
    --env='DB_USER=redmine' --env='DB_PASS=password' \
    --volume=/srv/docker/redmine/postgresql:/var/lib/postgresql \
    sameersbn/postgresql:9.4-21

(2)运行redmine容器

    docker run --name=redmine -d \
    --link=postgresql-redmine:postgresql --publish=10083:80 \
    --env='REDMINE_PORT=10083' \
    --volume=/srv/docker/redmine/redmine:/home/redmine/data \
    sameersbn/redmine:3.1.4-6

4、输入localhost:10083测试是否连接成功,账号密码为admin/admin

5、下载scrum插件、redcase插件，并将两个插件解压后的文件夹拷贝到`/srv/docker/redmine/redmine/plugins`目录中（关于redmine中所涉及到plugins的使用请参考[http://www.redmine.org/projects/redmine/wiki/Plugins](http://www.redmine.org/projects/redmine/wiki/Plugins)

6、进入所安装好的redmine容器中，进行插件的安装

    docker exec -it [redmine Container ID] /bin/bash
    cd /srv/docker/redmine/redmine
    apt-get update
    bundle install

7、安装scrum

    bundle exec rake redmine:plugins:migrate RAILS_ENV='production'

8、安装redcase

For Redmine 1.x:

    bundle exec rake db:migrate_plugins RAILS_ENV=production

For Redmine 2.x:

    bundle exec rake redmine:plugins:migrate RAILS_ENV=production

9、安装主题：

这里安装的主题是gitmike,安装过程详见[github](https://github.com/makotokw/redmine-theme-gitmike)

执行安装会报错，提示需要安装ruby和rubygems包
![](http://i.imgur.com/rFyJVCW.png)

安装ruby的两个依赖包

	apt-get install ruby
	apt-get install rubygems

再执行主题的安装即可

	gem install compass
	npm install -g gulp
	npm install
	gulp debug




#gitlab迁移

###准备：
gitlab迁移涉及到很多代码和账户的数据，所以一定要在没有对gitlab修改的条件下进行！

1、首先将volume中的数据迁移到对应相同的目录里，将gitlab中以下目录中的所有数据拷贝到新的机器中对应的目录中：

    /srv/docker/gitlab/postgresql
    /srv/docker/gitlab/redis
    /srv/docker/gitlab/gitlab

举例来说：

    scp -r /srv/docker/gitlab/postgresql chu888chu888@192.168.1.2:/srv/docker/gitlab

###实施：

1、使用docker-compose方式安装（推荐使用）
	
	wget https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml
	docker-compose up



2、手动方式安装

（1）安装Postgresql容器

    docker run --name gitlab-postgresql -d \
    --env 'DB_NAME=gitlabhq_production' \
    --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
    --env 'DB_EXTENSION=pg_trgm' \
    --volume /srv/docker/gitlab/postgresql:/var/lib/postgresql \
    sameersbn/postgresql:9.4-21

（2）安装redis容器

	docker run --name gitlab-redis -d \
    --volume /srv/docker/gitlab/redis:/var/lib/redis \
    sameersbn/redis:latest

（3）安装gitlab容器

	docker run --name gitlab -d \
    --link gitlab-postgresql:postgresql --link gitlab-redis:redisio \
    --publish 10022:22 --publish 10080:80 \
    --env 'GITLAB_PORT=10080' --env 'GITLAB_SSH_PORT=10022' \
    --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' \
    --volume /srv/docker/gitlab/gitlab:/home/git/data \
    sameersbn/gitlab:8.7.6

4、本例中修改`/srv/docker/gitlab中下载的docker-compose.yml`中参数，**TZ** 改为北京时区
**GITLAB_HOST** 改为本机gitlab的地址，这个值会被Gitlab用来生成repo的链接，所以**必须**要设置。否则，在创建的repo中，会发现所有的repo链接都是以localhost为hostname

![](http://i.imgur.com/p2XELeP.png)


5、在浏览器中输入本机的gitlab地址，端口为10080即可
例如：192.168.1.91:10080

附：
GitLab的一系列配置信息（如：GitLab_HOST、Mail、LDAP等）目前还无法从web界面进行配置。 
而docker-gitlab为这提供了以环境变量的形式提供了一系列可配置的参数，这些环境变量需要在GitLab镜像启动的时候指定。 如果当前GitLab容器已启动，可以停止、删除容器，然后基于GitLab镜像指定环境变量再创建、启动新容器即可。

#jenkins迁移

###准备：
首先将volume中的数据迁移到对应相同的目录里，将jenkins中以下目录中的所有数据拷贝到新的机器中对应的目录中：
	/srv/docker/jenkins/jenkins
	/srv/docker/jenkins/gradle

举例来说：

    scp -r /srv/docker/jenkins/jenkins chu888chu888@192.168.1.2:/srv/docker/jenkins
	
###实施：

1、pull一个最新的jenkins的镜像

    docker pull jenkins

2、运行生成容器（本例中需要在jenkins容器中实现docker命令，以及在docker中集成了docker-compose和gradle自动化构建工具）

    docker run -u root -p 8080:8080 --name jenkins \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v $(which docker):/bin/docker \
    -v /usr/local/bin/docker-compose:/usr/local/bin/docker-compose \
    -v /srv/docker/jenkins/jenkins:/var/jenkins_home \
    -v /srv/docker/jenkins/gradle:/root/.gradle \
    --privileged -d jenkins

3、进入jenkins容器的命令行界面

    docker exec -it jenkins /bin/bash

4、在容器中安装docker（和宿主机上安装过程一样）

(1) apt-get update

(2) 再执行apt-get install vim

这样就可以使用vim命令来编辑docker源了（吐槽一下：官方的源实在是太慢太慢了...会经常失败，多试几次即可）

(3) cd /etc/apt/sources.list.d

修改里面的**.list**文件，如果没有可以新建一个***.list 文件（例如aliyun.list），这里添加阿里源，将下面的源地址添加进文件后保存
    
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


(4) apt-get install wget

(5) apt-get install apt-utils

(6) 安装docker ，执行命令 wget -qO- https://get.docker.com/ | sh

(7) 输入docker ps测试下是否可用

(8) apt-get install docker-compose

(9) 输入docker-compose测试是否可用

(10) exit退出

5、在浏览器中输入本机jenkins地址，端口为8080，点击新建，如下图

![](http://i.imgur.com/fflTtCt.png)

输入Item名称，单击“构建一个自由风格的软件项目”

![](http://i.imgur.com/NvYOO13.png)

在构建中选择“Excute shell”，在输入框中输入docker ps，点击保存

![](http://i.imgur.com/UPqiQ7J.png)

单击立即构建，就会发现已经构建成功（这里是手动构建，可以配置自动构建，此处不深入探讨）

![](http://i.imgur.com/VXbR1rM.png)

单击进入Console Output，可以查看刚刚在shell框中输入的docker命令

![](http://i.imgur.com/IjPI2Zc.png)


6、最后commit成新的镜像

	docker commit [Container ID] [images:version]
 











