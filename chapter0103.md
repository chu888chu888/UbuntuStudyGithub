##ubuntu 忘记用户密码，忘记root密码的解决办法
ubuntu的root默认是禁止使用的，在安装的时候也没有要求设置root的密码。要使用，给root设置密码就行了，sudo passwd root 。如果只是普通用户密码忘了，用root就可以修改。如果root密码忘了就进入单用户模式。  
进入单用户模式：

　　1，在开机引导到GRUB等待界面（GRUB loading, please wait…）这里的时候按下ESC键来进入启动菜单，选择相应内核版本的Recovery Mode，按e键进入编辑状态。

　　2，移动光标，将”ro recovery nomodeset“修改为"rw single init=/bin/bash"以后 按F10就可以即进入单用户模式，

　　3，然后我们就可以用命令 passwd 用户名来修改密码了。

　　见到：root@(none):/#  马上输入”passwd“ 回车！

　　见到：Enter new UNIX password:  马上输入”123“ 回车！

　　见到：retype new UNIX password:马上输入”123“ 回车！

　　新密码变成”123“了。

　　4. 输入”reboot“ 回车！也可以Ctr Alt Del. 总之重启动就可以了！  
当在ubuntu执行sudo命令的时候出现：  

	sudo: /etc/sudoers is world writable  
	sudo: no valid sudoers sources found, quitting  
	sudo: unable to initialize policy plugin  
出错原因：在/etc/目录下的sudoers的权限具有写权限  
解决方法：`pkexec chmod   0440 /etc/sudoers`


　　