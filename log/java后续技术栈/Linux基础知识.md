简介：
此文章主要是对Linux系统的基础学习，一些比较基础的命令。
<!--more-->

##  一、前期准备

### 1、初步了解Linux

 注意：Linux内核版本都是一样的，自己也可以使用官方的内核，来开发一个自己的Linux系统。

####   为什么要用centOS呢？

因为redhat收费，centOS为社区开发版本，其中命令什么的都和redhat一致 ，所以选择centOS作为学习版本。

开源软件：足够多的眼睛，就会让问题浮现，开源不等同于免费。 

#### 最流行的网站平台搭建环境：

![image-20211112162447394](Linux基础知识/image-20211112162447394.png)

#### 如何查询服务器使用的是什么系统？

www.netcraft.com



### 2、系统分区

步骤：

![image-20211112183535625](Linux基础知识/image-20211112183535625.png)

1. 写入文件系统就是写入内存
2. 挂载点必须是目录，而且必须是空目录。

###  3、系统安装

图形化————》字符界面 转换   c+a+f2(f1)

### 4、网络连接

桥接模式：使用真实的网卡

net/only:使用虚拟的  同网段的ip，没网也可以进行通信



ifconfig:查看网络   与ipconfig类似。**ifconfig  网卡名称   ip地址**     修改IP地址 

### 5、远程工具

![image-20211113101905593](Linux基础知识/image-20211113101905593.png)

破解教程

![image-20211113101927774](Linux基础知识/image-20211113101927774.png)

注意是否可以ping通ip地址。

**centOS7  root用户不可以远程   好像要开启ssh   centOS7  就使用xshell吧**

#### CentOS 7.7 远程SSH连接不上问题解决

1.检查主机与目标服务器是否能相互ping通

在主机上 cmd --> telnet 192.168.199.227 22  -- 目标机器的ip、端口

如果主机无法使用telnet命令,则需要打开Telnet客户端,控制面板 --> 程序 --> 卸载或更改程序 --> 打开或关闭Windows功能 --> 选中 Telnet客户端

2.查看linux是否安装ssh服务

登录 root 用户下

命令：ssh localhost 

如果提示 "ssh:connect to host localhost port 22:connection refused",就说明没有打开ssh服务或者未安装ssh服务.

(1)如果linux系统是 ubuntu linux 版本,执行命令：sudo apt-get install openssh-server 安装ssh服务，在提示时都选择yes,然后会自动安装ssh服务.

(2)如果linux系统是 centos linux 版本,查看ssh是否安装,执行命令：rpm -qa | grep ssh 

　如果没有安装过ssh服务，则执行命令：yum install openssh-server 安装

3.安装完成后输入命令(root用户下)

　启动ssh服务命令:service sshd start 

　重启SSH服务:service sshd restart

　停止ssh服务命令:service sshd stop 

　查看ssh服务22端口是否启动命令：netstat -antp | grep sshd

　查看ssh服务进程命令：ps -ef|grep ssh

    设置ssh服务为开机启动命令：chkconfig sshd on 

　设置ssh服务禁止开机启动命令：chkconfig sshd off 

 

具体操作：

```shell
[root@centos01 ~]# ssh localhost
The authenticity of host 'localhost (::1)' can't be established.
ECDSA key fingerprint is SHA256:2LmA8OrgIY4wtuI+92d6t0xk6zleY3obJmkyh4E2tuA.
ECDSA key fingerprint is MD5:ed:ed:80:68:1f:47:a6:bc:73:b3:bc:9d:63:6b:c1:5c.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
root@localhost's password: 
Last login: Sun Apr 26 17:51:19 2020
[root@centos01 ~]# rpm -qa | grep ssh
openssh-clients-7.4p1-21.el7.x86_64
libssh2-1.8.0-3.el7.x86_64
openssh-7.4p1-21.el7.x86_64
openssh-server-7.4p1-21.el7.x86_64   -- 说明已经安装了ssh
[root@centos01 ~]# ps -ef|grep ssh
root       1301      1  0 16:15 ?        00:00:00 /usr/sbin/sshd -D
root      54071  53935  0 17:51 ?        00:00:00 /usr/bin/ssh-agent /bin/sh -c exec -l /bin/bash -c "env GNOME_SHELL_SESSION_MODE=classic gnome-session --session gnome-classic"
root      55734  54650  0 18:40 pts/0    00:00:00 ssh localhost
root      55735   1301  0 18:40 ?        00:00:00 sshd: root@pts/1
root      55742  53930  0 18:41 ?        00:00:00 /usr/bin/ssh-agent -D -a /run/user/0/keyring/.ssh
root      55815  55750  0 18:43 pts/1    00:00:00 grep --color=auto ssh
[root@centos01 ~]# ps -ef | grep ssh | grep -v grep
root       1301      1  0 16:15 ?        00:00:00 /usr/sbin/sshd -D
root      54071  53935  0 17:51 ?        00:00:00 /usr/bin/ssh-agent /bin/sh -c exec -l /bin/bash -c "env GNOME_SHELL_SESSION_MODE=classic gnome-session --session gnome-classic"
root      55734  54650  0 18:40 pts/0    00:00:00 ssh localhost
root      55735   1301  0 18:40 ?        00:00:00 sshd: root@pts/1
root      55742  53930  0 18:41 ?        00:00:00 /usr/bin/ssh-agent -D -a /run/user/0/keyring/.ssh
[root@centos01 ~]# service sshd start
Redirecting to /bin/systemctl start sshd.service
[root@centos01 ~]# ps -ef|grep ssh|grep -v grep
root       1301      1  0 16:15 ?        00:00:00 /usr/sbin/sshd -D
root      54071  53935  0 17:51 ?        00:00:00 /usr/bin/ssh-agent /bin/sh -c exec -l /bin/bash -c "env GNOME_SHELL_SESSION_MODE=classic gnome-session --session gnome-classic"
root      55734  54650  0 18:40 pts/0    00:00:00 ssh localhost
root      55735   1301  0 18:40 ?        00:00:00 sshd: root@pts/1
root      55742  53930  0 18:41 ?        00:00:00 /usr/bin/ssh-agent -D -a /run/user/0/keyring/.ssh
[root@centos01 ~]# netstat -antp | grep sshd
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1301/sshd           
tcp6       0      0 :::22                   :::*                    LISTEN      1301/sshd           
tcp6       0      0 ::1:22                  ::1:41754               ESTABLISHED 55735/sshd: root@pt 
[root@centos01 ~]# ps -ef | grep ssh | grep -v grep
root       1301      1  0 16:15 ?        00:00:00 /usr/sbin/sshd -D
root      54071  53935  0 17:51 ?        00:00:00 /usr/bin/ssh-agent /bin/sh -c exec -l /bin/bash -c "env GNOME_SHELL_SESSION_MODE=classic gnome-session --session gnome-classic"
root      55734  54650  0 18:40 pts/0    00:00:00 ssh localhost
root      55735   1301  0 18:40 ?        00:00:00 sshd: root@pts/1
root      55742  53930  0 18:41 ?        00:00:00 /usr/bin/ssh-agent -D -a /run/user/0/keyring/.ssh
[root@centos01 ~]# chkconfig sshd on     -- 设置开机启动ssh服务
注意：正在将请求转发到“systemctl enable sshd.service”。
[root@centos01 ~]# 
```

xshell

xftp

### 6、注意点

![image-20211113103218757](Linux基础知识/image-20211113103218757.png)

![image-20211113103231937](Linux基础知识/image-20211113103231937.png)

![image-20211113103832335](Linux基础知识/image-20211113103832335.png)

### 7、Linux个目录的作用

![image-20211113164216250](Linux基础知识/image-20211113164216250.png)

![ ](Linux基础知识/image-20211113164833061.png)

![image-20211113164844417](Linux基础知识/image-20211113164844417.png)

### 8、服务器注意事项

- 远程服务器不允许关机，只能重启
- 重启时应该关闭服务
- 不要在服务器访问高峰运行高负载命令
- 远程配置防火墙时不要把自己踢出服务器

## 二、linux常用命令

### 1、目录与文件处理命令

#### 命令格式：![image-20211114085736246](Linux基础知识/image-20211114085736246.png)

#### 目录处理命令：ls

![image-20211114090330809](Linux基础知识/image-20211114090330809.png)

文件的三个属性：所有者（只能有一个，可以变更）、所属组（只能有一个）、其他人   u  g   o

补充：

-h   人性化显示，可以自己计算文件大小。

-d  显示目录信息 ，一般与l一块使用

-i    i节点，相当于身份证号



#### 权限：

![image-20211114091519790](Linux基础知识/image-20211114091519790.png)

#### 目录处理命令：mkdir

![image-20211114092721111](Linux基础知识/image-20211114092721111.png)

-p      可以递归创建

#### 目录处理命令：cd

cd  路径   切换到指定路径

cd ..   返回上级目录

#### 目录处理命令：pwd

显示当前目录路径

#### 文件处理命令：rmdir

![image-20211114093102375](Linux基础知识/image-20211114093102375.png)

#### 目录处理命令：cp

![image-20211114093250516](Linux基础知识/image-20211114093250516.png)

补充：可以在复制的时候进行改名

#### 目录处理命令：mv

![image-20211114093839405](Linux基础知识/image-20211114093839405.png)

#### 快捷键：清空屏幕   c+l

#### 目录处理命令：rm

![image-20211114094308739](Linux基础知识/image-20211114094308739.png)

#### 文件处理命令：touch  

![image-20211114095608876](Linux基础知识/image-20211114095608876.png)

可以使用双引号创建带空格的文件名，但是不推荐使用。

#### 文件处理命令：cat

功能：显示文件内容（不太适合显示长文件）

![image-20211114095654646](Linux基础知识/image-20211114095654646.png)

#### 文件处理命令：tac

反向显示文件内容

#### 文件处理命令：more

功能：分页显示文件内容

![image-20211114100057951](Linux基础知识/image-20211114100057951.png)

缺点：不可以向上翻页

#### 文件处理命令：less

功能：与more功能类似，但是支持向上翻页

最主要的是  它可以进行搜索   /关键词  可以按n  下一个



#### 文件处理命令：head

功能：查看前多少行（默认查看十行）

![image-20211114101400895](Linux基础知识/image-20211114101400895.png)

#### 文件处理命令：tail

功能：查看文件内容的末尾多少行，默认十行

![image-20211114101756744](Linux基础知识/image-20211114101756744.png)

#### 文件处理命令：链接命令 ln

![](Linux基础知识/image-20211114103354644.png)

 软连接特点：和快捷方式差不多    

- 权限很高：l777
- 以l开头
- 文件大小很小，只是符号链接
- 箭头指向源文件

硬链接：几乎等于拷贝，但区别是 可以同步更新

- 不能跨分区
- 不能针对目录使用

如果删除了源文件：

- 软连接：也跟着删除
- 硬链接：不跟着删除

如何**判断哪个是硬链接**？

**使用i节点来判断**，硬链接的i节点与源文件相同。所以才可以同步更新。

### 2、权限管理命令

#### 改变文件或者目录权限命令：chmod

![image-20211118195143847](Linux基础知识/image-20211118195143847.png)

![image-20211118201651655](Linux基础知识/image-20211118201651655.png)

**需要注意的是：删除一个文件不是对一个文件有写权限。而是对目录有写权限，因为文件的写权限只能对内容进行操作，而目录的写权限是对文件有创建和删除命令的。**

简单来说就是：打狗看主人

#### 改变所有者命令：chown

![image-20211118202356799](Linux基础知识/image-20211118202356799.png)

在linux里面，改变文件的所有者，只有管理员root可以执行

#### 改变所属组命令：chgrp

![image-20211118203044099](Linux基础知识/image-20211118203044099.png)

#### 显示、设置文件的默认权限命令：umask

![image-20211118203610280](Linux基础知识/image-20211118203610280.png)

**注意：**在linux系统下，默认新建的**文件**是不具有可执行权限的（这样就可以阻止木马什么的病毒）

**0022**意思是：![image-20211120082649370](Linux基础知识/image-20211120082649370.png)

### 3、文件搜索命令

#### 文件搜索命令:find

![image-20211120083322675](Linux基础知识/image-20211120083322675.png)

选项（有很多，只掌握最常用的就可以了）

![image-20211120083615194](Linux基础知识/image-20211120083615194.png)

**注意**搜索文件时，linux是严格区分大小写的，但是如果使用**-iname**选项的话，就不区分大小写

- 数据块是linux下存储文件的最小单位

  ![image-20211120084708069](Linux基础知识/image-20211120084708069.png)

- 需要使用数据块来查找

![image-20211120085011360](Linux基础知识/image-20211120085011360.png)

![image-20211120085232843](Linux基础知识/image-20211120085232843.png)

![image-20211120085415992](Linux基础知识/image-20211120085415992.png)

#### 其他文件搜索命令：locate

![image-20211120091202581](Linux基础知识/image-20211120091202581.png)

需要注意的是：这个资料库不是实时更新，而是隔一段时间更新。可以手动使用updatedb命令来更新。

如果文件存放在/tmp下面，使用locate也可能找不到。

#### 文件搜索命令：which

![image-20211120092014779](Linux基础知识/image-20211120092014779.png)

例如：

![image-20211120092228344](Linux基础知识/image-20211120092228344.png)

bin就是所有用户都能用，sbin的意思就是只有root用户可以使用。

#### 命令搜索命令：whereis

![image-20211120093326756](Linux基础知识/image-20211120093326756.png)

搜索出现命令所在的绝对路径以及帮助文档所在的绝对路径



#### 文件内容搜索命令：grep（常用）

![image-20211120095247252](Linux基础知识/image-20211120095247252.png)

![image-20211120095042543](Linux基础知识/image-20211120095042543.png)

意思是，把文件中带#的行都去掉，只剩下不带#的行

### 4、帮助命令

#### 查看命令详细信息的命令：man（非常重要）

![image-20211120103248329](Linux基础知识/image-20211120103248329.png)



需要了解的是在配置文档中，1   一般是命令的帮助      5 	配置文件的帮助

![image-20211120102438417](Linux基础知识/image-20211120102438417.png)

![image-20211120102610155](Linux基础知识/image-20211120102610155.png)

附加：whatis +命令名称=简短的命令信息

apropos +配置文件名称=简短的配置文件信息

#### 显示选项信息命令：--help

例如： ls --help

![image-20211120103632088](Linux基础知识/image-20211120103632088.png)

#### 帮助命令：help

![image-20211120103706639](Linux基础知识/image-20211120103706639.png)

### 5、用户管理命令

#### 用户管理命令：useradd（添加用户）

![image-20211120105505025](Linux基础知识/image-20211120105505025.png)

#### 用户管理命令：passwd（为用户添加密码）

![image-20211120110527018](Linux基础知识/image-20211120110527018.png)

#### 用户管理命令：who（查看登录用户信息）

![image-20211120110559327](Linux基础知识/image-20211120110559327.png)

![image-20211120110826249](Linux基础知识/image-20211120110826249.png)

![image-20211120110842289](Linux基础知识/image-20211120110842289.png)

#### 用户登录命令：w（用户详细登录信息）

![image-20211120111347406](Linux基础知识/image-20211120111347406.png)

### 6、压缩解压命令

#### 压缩命令：gzip

![image-20211120111951858](Linux基础知识/image-20211120111951858.png)

需要注意的是：gzip只能压缩文件，不能压缩文件夹，压缩文件夹可以使用tar打包后压缩。而且不保存源文件

#### 解压缩命令：gunzip

![image-20211120112045593](Linux基础知识/image-20211120112045593.png)

或者gzip -d也可以解压缩

#### 压缩打包命令：tar（打包）

**![image-20211120113106747](Linux基础知识/image-20211120113106747.png)**

#### 解压命令：tar

![image-20211120113150171](Linux基础知识/image-20211120113150171.png)

需要注意的是：-z选项需要放到最前面

#### 压缩命令：zip

![image-20211120143534014](Linux基础知识/image-20211120143534014.png)

#### zip解压缩

![image-20211120143801916](Linux基础知识/image-20211120143801916.png)

#### 压缩命令：bzip2

**![image-20211120143921205](Linux基础知识/image-20211120143921205.png)**

与zip的区别：

- 可以保留源文件
- 压缩比 比较惊人

#### 解压缩命令：bunzip2

![image-20211120150928549](Linux基础知识/image-20211120150928549.png)

###  7、网络命令

#### 网络命令：write

![image-20211120155518803](Linux基础知识/image-20211120155518803.png)

一定要注意，是给在线用户发

#### 网络命令：wall

![image-20211122191809594](Linux基础知识/image-20211122191809594.png)

给所有在线的用户发送广播

#### 网络命令：ping

![image-20211122192354097](Linux基础知识/image-20211122192354097.png)

功能：看网络是否联通

#### 网络命令：ifconfig

![image-20211122192446217](Linux基础知识/image-20211122192446217.png)

#### 网络命令：mail

![image-20211122193006063](Linux基础知识/image-20211122193006063.png)

发：mail   用户名

收：mail

#### 网络命令：last

![image-20211122193447703](Linux基础知识/image-20211122193447703.png)

查看所有已经登录过的用户信息。

#### 网络命令：lastlog

![image-20211122193702715](Linux基础知识/image-20211122193702715.png)

查看用户最后一次登录的时间。

网络命令：traceroute（跟踪路由）

![image-20211122193902674](Linux基础知识/image-20211122193902674.png)

用来查看经过的路由节点。

网络命令：netstat（网络状态）（非常重要）

![image-20211122194330563](Linux基础知识/image-20211122194330563.png)

![image-20211122194557537](Linux基础知识/image-20211122194557537.png)

tcp：相当于手机，更安全

udp：相当于发短信，更快，但不安全

####  网络命令：setup

![image-20211122195616672](Linux基础知识/image-20211122195616672.png)

注意：这是redhat系列专有的命令。

#### 挂载命令：mount

![image-20211122200231067](Linux基础知识/image-20211122200231067.png)

### 8、关机重启命令

#### 关机重启命令：shutdown

![image-20211122203054861](Linux基础知识/image-20211122203054861.png)

![image-20211122203853326](Linux基础知识/image-20211122203853326.png)

![image-20211122204002436](Linux基础知识/image-20211122204002436.png)

![image-20211122204108085](Linux基础知识/image-20211122204108085.png)

![image-20211122205057722](Linux基础知识/image-20211122205057722.png)

## 三、文本编辑Vim

![image-20211122210016589](Linux基础知识/image-20211122210016589.png)

![image-20211122210530890](Linux基础知识/image-20211122210530890.png)

### 插入命令

![image-20211122210609123](Linux基础知识/image-20211122210609123.png)

### 定位命令

![image-20211122211322822](Linux基础知识/image-20211122211322822.png)

### 删除命令

![image-20211122211350885](Linux基础知识/image-20211122211350885.png)

### 复制命令

![image-20211122211718925](Linux基础知识/image-20211122211718925.png)

### 替换与取消命令

![image-20211122212103650](Linux基础知识/image-20211122212103650.png)

### 搜索和搜索替换命令

![image-20211122212140867](Linux基础知识/image-20211122212140867.png)

### 保存和退出命令

![image-20211122212519228](Linux基础知识/image-20211122212519228.png)

### Vim使用技巧

#### 1、导入文件内容

> ：r 文件名    #导入文件内容到光标所在位置

![image-20211201135524461](Linux基础知识/image-20211201135524461.png)

#### 2、在不退出Vim的情况下执行命令

> ：！命令      

可以与：r一起使用：

例如：

> :r !data   #将时间插入光标所在行

![image-20211201140304135](Linux基础知识/image-20211201140304135.png)

#### 3、定义快捷键

![image-20211201202502477](Linux基础知识/image-20211201202502477.png)

> :map  (ctrl+v+p)==^p   I#<ESC>   (意思是将ctrl+p定义成跳到行首插入一个#在进入插入模式      就是把这一行注释)

![image-20211201204756980](Linux基础知识/image-20211201204756980.png)

想要永久使用快捷键的话：需要去家目录下：例如

> /root/.vimrc

去定义快捷键，永久有效

## 四、软件包管理

### 1、软件包分类

- 源码包
  - 脚本安装包
  - ![image-20211201211014162](Linux基础知识/image-20211201211014162.png)
  - ![image-20211207165339870](Linux基础知识/image-20211207165339870.png)
- 二进制包（RPM包、系统默认包） 
  - ![image-20211207170213748](Linux基础知识/image-20211207170213748.png)
  - ![image-20211207170313204](Linux基础知识/image-20211207170313204.png)

### 2、rpm命令管理-包名名与依赖性

1. rpm包的命名规则

   ![image-20211207171320812](Linux基础知识/image-20211207171320812.png)

2. rpm包依赖性

   ![image-20211207172403022](Linux基础知识/image-20211207172403022.png)

### 3、rpm命令管理-安装升级与卸载

1. 包全名与包名

   ![image-20211207173056632](Linux基础知识/image-20211207173056632.png)

2. RPM安装

   ![image-20211207173357736](Linux基础知识/image-20211207173357736.png)

3. RPM升级

   ![image-20211208144105109](Linux基础知识/image-20211208144105109.png)

4. RPM卸载

   ![image-20211208144410387](Linux基础知识/image-20211208144410387.png)

### 3、rpm命令管理-查询

1. 查询是否安装

   ![image-20211208144832397](Linux基础知识/image-20211208144832397.png)

2. 查询软件包详细信息

   ![image-20211208145109892](Linux基础知识/image-20211208145109892.png)

3. 查询软件包中文件安装位置

   ![image-20211208145505155](Linux基础知识/image-20211208145505155.png)

4. 查询系统文件属于哪个包

   ![image-20211208145659618](Linux基础知识/image-20211208145659618.png)

5. 查询软件包的依赖性

   ![image-20211208145945559](Linux基础知识/image-20211208145945559.png)

### 4、rpm命令管理-校验和文件提取

1. RPM包校验 

   ![image-20211208150549001](Linux基础知识/image-20211208150549001.png)

   意思就是，拿初始安装的与修改后的进行对比，看是否有问题

   ![image-20211208150906599](Linux基础知识/image-20211208150906599.png)

   ![image-20211208151248541](Linux基础知识/image-20211208151248541.png)

2. RPM包中文件提取

   ![image-20211208151550508](Linux基础知识/image-20211208151550508.png)

   有什么用呢？比如说，我在操作系统的时候，误删除了一个配置文件，现在不需要重新安装一个系统，只要知道这个配置文件属于哪一个包，提取出来，覆盖即可。

   操作示例：

   ![image-20211208152057225](Linux基础知识/image-20211208152057225.png)

   这个文件提前的操作，最主要的功能就是用来修补系统的错误。

### 5、yum在线管理-ip地址配置与网络yum源

如果访问内网，只需要ip与子网掩码即可。

如果要访问公网，还需要配置网关和DNS。

使用最简单的配置方法：setup

1. IP地址配置

   ![image-20211208163937314](Linux基础知识/image-20211208163937314.png)

2. 网络YUM源

   ![image-20211208164332265](Linux基础知识/image-20211208164332265.png)

### 6、yum命令

1. 常用yum命令

   ![image-20211208165528753](Linux基础知识/image-20211208165528753.png)

2. yum软件组管理管理命令

   ![image-20211208200455832](Linux基础知识/image-20211208200455832.png)

### 7、光盘yum源搭建 

![image-20211208201709323](Linux基础知识/image-20211208201709323.png)

### 8、源码包管理

#### 1、源码包和RPM包的区别

- 区别

  ![image-20211208202215252](Linux基础知识/image-20211208202215252.png)

- RPM包安装位置

  ![image-20211208202327926](Linux基础知识/image-20211208202327926.png)

- 源码包安装位置

  ![image-20211208203954435](Linux基础知识/image-20211208203954435.png)

- 安装位置不同带来的影响（源码包就不能使用service启动）

  ![image-20211208203706947](Linux基础知识/image-20211208203706947.png)

  ![image-20211208203821690](Linux基础知识/image-20211208203821690.png)

#### 2、源码包安装过程

1. 安装准备

   ![image-20211208205453930](Linux基础知识/image-20211208205453930.png)

2. 开始

   ![image-20211209093940563](Linux基础知识/image-20211209093940563.png)

   一般的源码包中，都会有INSTALL(安装说明)和README（使用说明）

3. 软件配置与检查

   ![image-20211209094147185](Linux基础知识/image-20211209094147185.png)

   ![image-20211209094307271](Linux基础知识/image-20211209094307271.png)

   安装路径一定要写！！！

   > ./configure --prefiax=/usr/local/自定义名称

4. make编译

   作用：调用gcc   把源码包翻译成为机器语言。

    到现在为止，还没有写入任何文件，只是产生了临时文件。

   如果在这两步报错，不需要执行任何操作，

   使用make clean来清除编译产生的临时文件。

5. make install进行安装即可

6. 卸载源码包

   直接使用

   > rm -rf 删除安装目录即可

#### 3、脚本安装包

![image-20211209095656928](Linux基础知识/image-20211209095656928.png)

1. Webmin的作用

   ![image-20211209095903769](Linux基础知识/image-20211209095903769.png)

2. Webmin的安装

   ![image-20211209100028195](Linux基础知识/image-20211209100028195.png)

3. 进入压缩目录之后，执行setup.sh



## 五、用户和用户组管理

### 5.1、用户配置文件

#### 5.1.1、用户信息文件/etc/passwd

1. 用户管理简介

   ![image-20211209101346477](Linux基础知识/image-20211209101346477.png)

2. /etc/passwd

   ![image-20211209102035508](Linux基础知识/image-20211209102035508.png)

   ![image-20211209103943333](Linux基础知识/image-20211209103943333.png)

3. 初始组与附加组

   ![image-20211209104058178](Linux基础知识/image-20211209104058178.png)

4. shell是什么？

   ![image-20211209104842056](Linux基础知识/image-20211209104842056.png)

####  5.1.2、影子文件/etc/shadow

![image-20211209105602928](Linux基础知识/image-20211209105602928.png)

![image-20211209110541259](Linux基础知识/image-20211209110541259.png)

![image-20211209110458014](Linux基础知识/image-20211209110458014.png)

####  5.1.3、组信息文件/etc/group和组密码文件/etc/gshadow 

1. 组信息文件/etc/group

   ![image-20211209111155442](Linux基础知识/image-20211209111155442.png)


### 5.2、用户管理相关文件

#### 5.21、用户的家目录

![image-20211210165047521](Linux基础知识/image-20211210165047521.png)

#### 5.22、用户的邮箱

![image-20211210170045880](Linux基础知识/image-20211210170045880.png)

#### 5.33、用户模板目录

![image-20211210170307549](Linux基础知识/image-20211210170307549.png)

模板是什么意思？

例如，创建一个用户，他可能会自动在家目录创建一些文件，模板，就是来控制来创建哪些文件的。

### 5.3、用户管理命令

#### 5.3.1、useradd：添加用户

注意：如果添加一个用户不设置密码，是不完整的。是不能登录的。

![image-20211213140948603](Linux基础知识/image-20211213140948603.png)

添加完成用户以后，就可以在这几个文件中查询到sc用户的信息了。

**useradd用户命令格式：**

![image-20211213141340150](Linux基础知识/image-20211213141340150.png)

用户默认值文件：

![image-20211213142122030](Linux基础知识/image-20211213142122030.png)

另外一个用户配置文件：

![image-20211213142356275](Linux基础知识/image-20211213142356275.png)

#### 5.3.2、passwd：添加用户

![image-20211213145156264](Linux基础知识/image-20211213145156264.png)

超级用户可以改变任意用户的密码，普通用户只能修改自己的密码，即：passwd 

**查看密码状态：**

![image-20211213145726966](Linux基础知识/image-20211213145726966.png)

**锁定用户和解锁用户：**

![image-20211213145810051](Linux基础知识/image-20211213145810051.png)



使用字符串作为用户的密码：

![image-20211215095638177](Linux基础知识/image-20211215095638177.png)



#### 5.3.3、修改用户信息usermod

![image-20211215095910507](Linux基础知识/image-20211215095910507.png)

![image-20211215095959093](Linux基础知识/image-20211215095959093.png)

#### 5.3.4、修改用户密码状态chage

![image-20211215100453015](Linux基础知识/image-20211215100453015.png)

#### 5.3.5、删除用户userdel与切换用户su

删除用户选项

![image-20211215104907693](Linux基础知识/image-20211215104907693.png)

手动删除用户

![image-20211215105019144](Linux基础知识/image-20211215105019144.png)

查看用户id

![image-20211215110340022](Linux基础知识/image-20211215110340022.png)

 切换用户身份su

![image-20211215110433354](Linux基础知识/image-20211215110433354.png)

### 5.4、用户组管理命令

#### 5.4.1、添加用户组groupadd

![image-20211215111506971](Linux基础知识/image-20211215111506971.png)

#### 5.4.2、修改组groupmod

![image-20211215111649902](Linux基础知识/image-20211215111649902.png)

#### 5.4.3、删除组groupdel

![image-20211215112044235](Linux基础知识/image-20211215112044235.png)

如果用户组中有用户的话，可以删除吗？

如果组中有初始用户，那么这个组不能删，因为删了之后，初始组没地方放。

如果这个组里面没有初始用户，而是附加用户，那么可以删除。

#### 5.4.4、把用户添加入组或者从组中删除

![image-20211215112719518](Linux基础知识/image-20211215112719518.png)

## 六、权限管理

### 6.1、ACL权限

#### 6.1.1、ACL权限介绍

![image-20211215113303614](Linux基础知识/image-20211215113303614.png)

与windows类似，所有者、所属组、其他人。已经不够用了，所以单独给需要的用户添加权限。

#### 6.1.2、查看分区ACL权限是否开启

![image-20211215201753712](Linux基础知识/image-20211215201753712.png)

ACL权限是和分区相关的。默认分区都是开启ACL权限的。与ACL相关的是**挂载默认选项**。

#### 6.1.3、临时开启分区ACL权限

![image-20211215202237947](Linux基础知识/image-20211215202237947.png)

#### 6.1.4、永久开启分区ACL权限

![image-20211215202624333](Linux基础知识/image-20211215202624333.png)

#### 6.2、查看与设定ACL权限

#### 6.2.1、查看ACL命令getfacl

![image-20211215202911933](Linux基础知识/image-20211215202911933.png)

#### 6.2.2、设定ACL权限的命令setfacl

![image-20211215203009126](Linux基础知识/image-20211215203009126.png)

#### 6.2.3、给用户设定ACL2权限

![image-20211215203705568](Linux基础知识/image-20211215203705568.png)

#### 6.3、最大有限权限与删除ACL权限

#### 6.3.1、最大有效权限mask

![image-20211215204324901](Linux基础知识/image-20211215204324901.png)

![image-20211215204338076](Linux基础知识/image-20211215204338076.png)

![image-20211215204529035](Linux基础知识/image-20211215204529035.png)

#### 6.3.2、删除ACL权限

![image-20211215210838293](Linux基础知识/image-20211215210838293.png)

![image-20211215210900594](Linux基础知识/image-20211215210900594.png)

#### 6.4、默认ACL权限和递归ACL权限

#### 6.4.1、递归ACL权限

![image-20211216103303618](Linux基础知识/image-20211216103303618.png)

#### 6.4.2、默认ACL权限

主要用来控制新创建或者新加入的文件的权限。

![image-20211216103348082](Linux基础知识/image-20211216103348082.png)

### 6.2、文件特殊权限

#### 6.2.1、SetUID的功能

![image-20211216105213546](Linux基础知识/image-20211216105213546.png)

设定SsetUID的方法

![image-20211216110415461](Linux基础知识/image-20211216110415461.png)

取消SetUID方法

![image-20211216111206347](Linux基础知识/image-20211216111206347.png)

危险的SetUID

![image-20211216111244196](Linux基础知识/image-20211216111244196.png)

总结：普通用户在执行拥有SUID的程序的时候，会暂时变身成为超级用户。

#### 6.2.2、SetGID的功能

1、SetGID针对文件的作用

![image-20211216111955028](Linux基础知识/image-20211216111955028.png)

举例：

![image-20211216112847374](Linux基础知识/image-20211216112847374.png)

2、SetGID针对目录的作用

![image-20211216112041459](Linux基础知识/image-20211216112041459.png)







意思就是赋予目录S权限之后，在目录中创建文件，文件的所属组会与父目录的s属组相同。

3、取消SetGID

![image-20211216113433566](Linux基础知识/image-20211216113433566.png)

#### 6.2.3、Sticky BIT

1. SBIT粘着位作用

   ![image-20211216153448745](Linux基础知识/image-20211216153448745.png)

2. 设置粘着位

   ![image-20211216154231011](Linux基础知识/image-20211216154231011.png)

3. 通俗来说，就是赋予粘着位之后，普通用户只能删除自己创建的文件，而不能删除其他用户创建的文件。

### 6.3、文件系统属性chattr权限

1. 命令格式

   ![image-20211216155058938](Linux基础知识/image-20211216155058938.png)

   选项：

   ![image-20211216155419634](Linux基础知识/image-20211216155419634.png)

   -i：意思就是说把文件锁起来，只能看不能改。连root用户也能被管控。对文件夹使用的话，就相当于租房子给别人，你可以使用和改变里面的东西，但是你不可以自己搬进来或者搬出去东西。

   -a：也都可以赋予文件或者目录，与i相比，i选项更加严格，而对文件操作 的时候，-

   a只能追加文件内容，而不可以修改与删除。对目录进行操作的时候，a选项可以修改和创建文件夹下的文件，但是不可以删除。

   **需要注意：**使用普通的ls命令是无法查看attr权限的。

2. 查看文件系统属性

   ![image-20211216155618849](Linux基础知识/image-20211216155618849.png)

3. 操作示例

   ![image-20211216155839582](Linux基础知识/image-20211216155839582.png)

   

### 6.4、系统命令权限（sudo）

1. 简介

   ![image-20211216161702976](Linux基础知识/image-20211216161702976.png)

2. sudo的使用

   ![image-20211216162013075](Linux基础知识/image-20211216162013075.png)

3. 演示

   ![image-20211216163148331](Linux基础知识/image-20211216163148331.png)

4. 普通用户执行sudo命令

   ![image-20211216163215994](Linux基础知识/image-20211216163215994.png)

## 七、文件系统管理

### 7.1、回顾分区和文件系统

1. 分区类型

   ![image-20211216164736183](Linux基础知识/image-20211216164736183.png)

2. 分区之后的设备名

   ![image-20211222093553503](Linux基础知识/image-20211222093553503.png)

   

3. 文件系统

   ![image-20211222093759932](Linux基础知识/image-20211222093759932.png)

   ![img](Linux基础知识/image-20211222093851733.png)![img](Linux基础知识/image-20211222093915775.png)

### 7.2、文件系统常用命令

#### 7.2.1、df命令、du命令、fsck命令和dump2fs命令

##### 1、文件系统查看命令   df



![img](Linux基础知识/image-20211222101705880.png)

##### 2、统计文件或者目录大小du

![img](Linux基础知识/image-20211222102053640.png)

##### 3、df与du的区别

df：所有文件+系统+进程

du：所有文件

![image-20211222103405547](Linux基础知识/image-20211222103405547.png)

##### 3、文件修复命令fsck（不太推荐使用）



![img](Linux基础知识/image-20211222103437430.png)

##### 4、显示磁盘状态命令dumpe2fs



![img](Linux基础知识/image-20211222105324398.png)

#### 7.2.2、挂载命令

##### 1、查询与自动挂载

##### ![img](Linux基础知识/image-20211222110845526.png)2、挂载命令格式

![img](Linux基础知识/image-20211224100435877.png)-o：![image-20211224100635865](Linux基础知识/image-20211224100635865.png)

示例：

![img](Linux基础知识/image-20211224101110489.png)

#### 7.2.3、挂载U盘和光盘

![image-20211224111233773](Linux基础知识/image-20211224111233773.png)

挂载u盘

![image-20211226104047889](Linux基础知识/image-20211226104047889.png)

还有需要注意的一点，连接u盘或者移动硬盘，需要linux主机进行操作。

#### 7.2.4、支持NTFS文件系统

想要linux支持NTFS格式，两种方法：重新编译内核，或者下载软件。后者比较简单，选择后者。

1、下载NTFS-3G插件

2、安装NTFS-3G

![img](Linux基础知识/image-20211226105432180.png)3、使用

![image-20211226110231616](Linux基础知识/image-20211226110231616.png)

### 7.3、fdisk分区

1、添加新硬盘

2、查看新硬盘

![image-20211226110955703](Linux基础知识/image-20211226110955703.png)

3、使用fdisk命令进行分区

![image-20211226111504886](Linux基础知识/image-20211226111504886.png)

![image-20211226111628777](Linux基础知识/image-20211226111628777.png)

![image-20211226111657378](Linux基础知识/image-20211226111657378.png)

注意最后w保存之后，是需要重新启动来加载新的分区的。

4、重新读取分区信息

![image-20211229104321501](Linux基础知识/image-20211229104321501.png)

5、格式化分析

![img](Linux基础知识/image-20211229104521923.png)

### 7.4、分区自动挂载与fstab文件修复

1、/etc/fstab文件

![image-20211229110950724](Linux基础知识/image-20211229110950724.png)

使用uuid有什么好处？在分区出现变化的时候，不会出现错误。

那么，如何获取分区的uuid呢？

![image-20211229111408763](Linux基础知识/image-20211229111408763.png)

2、分区自动挂载

![image-20211229135012899](Linux基础知识/image-20211229135012899.png)

![img](Linux基础知识/image-20211229134956933.png)

3、/etc/fstab文件修复

![image-20211229135052454](Linux基础知识/image-20211229135052454.png)

如果在修改错误的情况下（前提是根分区没设置错误的情况下），可以使用这个命令来修复fstab的权限，因为fstab设置错误的话，开机会报错，使用此命令可以挽救一点。

所以在修改fstab文件的时候，需要仔细。

## 八、Shell基础

### 8.1、Shell概述

1、shell是什么？

![img](Linux基础知识/image-20211229142500920.png)层次关系

![img](Linux基础知识/image-20211229142518910.png)![img](Linux基础知识/image-20211229142538883.png)两个功能：

- 命令解释器，让用户有一个操作界面
- 支持强大的编程语言界面，允许用户编程

#### 2、shell的分类

![image-20211229142939151](Linux基础知识/image-20211229142939151.png)

![image-20211229142947153](Linux基础知识/image-20211229142947153.png)

![img](Linux基础知识/image-20211229143010069.png)![image-20211229143121918](Linux基础知识/image-20211229143121918.png)

#### 3、Linux支持的shell

![image-20211229143207555](Linux基础知识/image-20211229143207555.png)

![image-20211229143347846](Linux基础知识/image-20211229143347846.png)

### 8.2、脚本执行方式

1、echo输出命令

![image-20211229145154493](Linux基础知识/image-20211229145154493.png)

![img](Linux基础知识/image-20211229145203971.png)示例：

![img](Linux基础知识/image-20211229145349510.png)2、第一个脚本

![img](Linux基础知识/image-20211229145810490.png)3、执行脚本

![image-20211229150718692](Linux基础知识/image-20211229150718692.png)

如果是windows上的dos命令shell脚本，复制到linux上是不可以执行的，因为其中包含着特殊符号，需要转换-------使用dos2unix命令  转换就可以。

### 8.3、Bash的基本功能

#### 8.3.1、历史命令与命令补全

1、历史命令

![image-20220104084907517](Linux基础知识/image-20220104084907517.png)

![image-20220104101136980](Linux基础知识/image-20220104101136980.png)

历史命令的调用

![img](Linux基础知识/image-20220104103935103.png)2、命令与文件补全

![image-20220104104228640](Linux基础知识/image-20220104104228640.png)

#### 8.3.2、命令别名与常用快捷键

1、命令别名

![img](Linux基础知识/image-20220104110417864.png)2、命令执行顺序

![img](Linux基础知识/image-20220104110849397.png)简而言之：

执行顺序     绝对路径》相对路径》别名》bash内部命令》环境变量

比如定义vi="vim"   别名就会将vi这个命令覆盖，因为别名比vi本身的优先级高。

注意：这些赋值别名的操作只会临时生效，关机就会消失。

**让别名永久生效**

![image-20220104111357168](Linux基础知识/image-20220104111357168.png)

**删除别名**

![image-20220104111633296](Linux基础知识/image-20220104111633296.png)

3、Bash常用快捷键

![img](Linux基础知识/image-20220104111754921.png)

#### 8.3.3、输入输出重定向

1、标准输入输出

![img](Linux基础知识/image-20220104112549414.png)2、输出重定向

![img](Linux基础知识/image-20220104112751511.png)上边的输出重定向有一个缺点，就是还需要自己判断语句的正确与错误。

![image-20220105091624103](Linux基础知识/image-20220105091624103.png)

而加上&之后，就无需判断语句的正确或者错误了，正确与错误都会输出到文件中去。

3、输入重定向

![img](Linux基础知识/image-20220105092108558.png)![img](Linux基础知识/image-20220105092439057.png)

#### 8.3.4、多命令顺序执行与管道符

1、多命令顺序执行

![image-20220105093241423](Linux基础知识/image-20220105093241423.png)

分号的使用，意思是创建100M的文件需要的时间。（分号：连接多个命令）

![image-20220105093225950](Linux基础知识/image-20220105093225950.png)

2、管道符

![image-20220105093953886](Linux基础知识/image-20220105093953886.png)

需要注意的一点，命令1必须有正确的输出，否则命令2不能执行



![image-20220105094614350](Linux基础知识/image-20220105094614350.png)

#### 8.3.5、通配符与其他特殊符号

1、通配符

![img](Linux基础知识/image-20220105095150924.png)示例：

![img](Linux基础知识/image-20220105095341263.png)2、Bash中其他特殊符号

![image-20220105100332289](Linux基础知识/image-20220105100332289.png)

单引号和双引号：

示例：

![image-20220105100514902](Linux基础知识/image-20220105100514902.png)

反引号与$()

![image-20220105100728448](Linux基础知识/image-20220105100728448.png)

### 8.4、Bash的变量

#### 8.4.1、用户自定义变量

1、什么是变量？

![img](Linux基础知识/image-20220105101433089.png)2、变量设置规则

![image-20220105101648416](Linux基础知识/image-20220105101648416.png)

![image-20220105101923501](Linux基础知识/image-20220105101923501.png)

![img](Linux基础知识/image-20220105102105480.png)3、变量分类

![image-20220105102448040](Linux基础知识/image-20220105102448040.png)

4、本地变量（用户自定义变量）

![img](Linux基础知识/image-20220105105932548.png)![image-20220105110134029](Linux基础知识/image-20220105110134029.png)

#### 8.4.2、环境变量

1、环境变量是什么？

![image-20220105110650066](Linux基础知识/image-20220105110650066.png)

环境变量与自定义变量的区别就是，作用范围，相当于环境变量是静态变量。

2、设置环境变量

![image-20220105111101234](Linux基础知识/image-20220105111101234.png)

那么 如何查看shell的父类关系呢？

使用pstree命令。如下图：

![image-20220105111541687](Linux基础知识/image-20220105111541687.png)

系统定义的变量：

![image-20220105135943469](Linux基础知识/image-20220105135943469.png)



![image-20220105140041198](Linux基础知识/image-20220105140041198.png)

![image-20220105140311660](Linux基础知识/image-20220105140311660.png)

#### 8.4.3、位置参数变量

1、位置变量参数

![img](Linux基础知识/image-20220105141200540.png)$n 实际应用：

![img](Linux基础知识/image-20220105141326892.png)$*与$@ 实际应用：

![image-20220105142258198](Linux基础知识/image-20220105142258198.png)

$*与$@的区别：

![image-20220105142538131](Linux基础知识/image-20220105142538131.png)

#### 8.4.4、预定义变量

1、预定义变量($?,$$,$!)

![image-20220105143420341](Linux基础知识/image-20220105143420341.png)

2、接收键盘输入

![img](Linux基础知识/image-20220105143920695.png)示例：

![image-20220105144143406](Linux基础知识/image-20220105144143406.png)

### 8.5、Bash的运算符

#### 8.5.1、数值运算与运算符

1、declare声明变量属性

![image-20220105145710655](Linux基础知识/image-20220105145710655.png)

2、数值运算----方法1

![img](Linux基础知识/image-20220105145800150.png)方法2：expr或let数值运算工具

![img](Linux基础知识/image-20220105150020263.png)方法3："$((运算式))"或者"$[运算式]"   **推荐使用**

![image-20220105150215322](Linux基础知识/image-20220105150215322.png)

3、运算符

![image-20220105150530537](Linux基础知识/image-20220105150530537.png)

使用（）可以改变运算优先级

![image-20220105150925808](Linux基础知识/image-20220105150925808.png)

#### 8.5.2、变量测试与内容替换

![image-20220105151354569](Linux基础知识/image-20220105151354569.png)

不推荐记下来，什么时候用，什么时候回头来找。

示例：

![image-20220105151807225](Linux基础知识/image-20220105151807225.png)

![image-20220105151823590](Linux基础知识/image-20220105151823590.png)

### 8.6、环境变量配置文件

#### 8.6.1、环境变量配置文件简介

为什么配置环境变量配置文件呢？因为不配置的话，设置的环境变量重启就会消失。

要想永久性的配置，必须要配置配置文件。

1、source命令

![img](Linux基础知识/image-20220105153456996.png)2、环境变量配置文件简介

![image-20220105153635111](Linux基础知识/image-20220105153635111.png)

系统环境变量有哪些配置文件呢？

<img src="Linux基础知识/image-20220105154014722.png" alt="image-20220105154014722" style="zoom:50%;" />

主要是这五类。

#### 8.6.2、环境变量配置文件作用

![img](Linux基础知识/image-20220108110957077.png)注意：写在etc下的环境变量配置文件，对所有的用户都生效，而写在家目录下的配置文件只对此用户生效。

**配置文件调用顺序**

![image-20220108154936444](Linux基础知识/image-20220108154936444.png)

可以将需要更改的环境变量加到以上四个配置文件中，注意：/etc下的是对所有用户生效，~是当前用户。

##### 1、/etc/profile的作用（需要输入用户名密码）

![img](Linux基础知识/image-20220108153816803.png)2、~/.bash_profile的作用

![image-20220108155339403](Linux基础知识/image-20220108155339403.png)



##### 3、~/.bashrc的作用

![image-20220108155938262](Linux基础知识/image-20220108155938262.png)

##### 4、/etc/bashrc作用（不需要输入密码）

![image-20220108160008176](Linux基础知识/image-20220108160008176.png)

不和/etc/profile的定义冲突

#### 8.6.3、其他配置文件和登录信息

1、注销时生效的环境变量配置文件

![image-20220108160510497](Linux基础知识/image-20220108160510497.png)

2、其他配置文件（历史命令的保存文件）

![image-20220108160617753](Linux基础知识/image-20220108160617753.png)

3、shell登录信息

**本地**

![image-20220108160823112](Linux基础知识/image-20220108160823112.png)

**远程**

![img](Linux基础知识/image-20220108162959193.png)

注意哦，转义符在。net文件中不能使用

**远程+本地：**

![img](Linux基础知识/image-20220108163534657.png)8.7、正则表达式

#### 8.7.1、基础正则表达式

1、正则表达式与通配符

![image-20220108164001893](Linux基础知识/image-20220108164001893.png)

其他语言中或许通配符是属于正则的，但是在shell中，正则是匹配文件中的内容的，通配符是匹配系统中的文件名的；

2、基础正则表达式（抄十遍--------一定要记住！！！很重要！）

![image-20220108164847612](Linux基础知识/image-20220108164847612.png)

*的作用：

![image-20220108165821634](Linux基础知识/image-20220108165821634.png)

.的作用：

![img](Linux基础知识/image-20220108165924912.png)^和$的作用：

![img](Linux基础知识/image-20220108170104028.png)[^]的作用：

![image-20220108170522330](Linux基础知识/image-20220108170522330.png)

\的作用：

![img](Linux基础知识/image-20220108170556113.png)匹配前面的字符恰好出现n次。

![image-20220108171142795](Linux基础知识/image-20220108171142795.png)

匹配前面的字符出现n-m次；

![image-20220108171236429](Linux基础知识/image-20220108171236429.png)

#### 8.7.2、字符截取命令

##### 8.7.2.1、cut字段提取命令

语法：

![image-20220108172116304](Linux基础知识/image-20220108172116304.png)

示例：

![image-20220108172930627](Linux基础知识/image-20220108172930627.png)

![img](Linux基础知识/image-20220108173008884.png)cut命令的局限：

![image-20220108173521633](Linux基础知识/image-20220108173521633.png)



##### 8.7.2.2、printf命令

语法：

![image-20220111154618862](Linux基础知识/image-20220111154618862.png)

![img](Linux基础知识/image-20220111154659036.png)

示例：

![image-20220111155604607](Linux基础知识/image-20220111155604607.png)

![image-20220111155627330](Linux基础知识/image-20220111155627330.png)

![image-20220111155652091](Linux基础知识/image-20220111155652091.png)



##### 8.7.2.3、awk命令

语法：

![image-20220111160120094](Linux基础知识/image-20220111160120094.png)

**BEGIN命令：**

作用，在读入数据之前，先输出BEGIN后面的指令。

![image-20220111163729595](Linux基础知识/image-20220111163729595.png)

**END命令：**

与BEGIN作用相反，在所有命令执行之后，输出，，，

![image-20220111163845583](Linux基础知识/image-20220111163845583.png)

**FS内置变量：**

作用：指定分隔符，一般与BEGIN一起使用，因为不使用BEGIN的话，有可能造成第一行来不及处理的bug。

![image-20220111164142787](Linux基础知识/image-20220111164142787.png)



##### 8.7.2.4、sed命令

sed简介：

![image-20220111164822549](Linux基础知识/image-20220111164822549.png)

语法：

![img](Linux基础知识/image-20220111165049643.png)

支持哪些动作？

![image-20220111165150784](Linux基础知识/image-20220111165150784.png)



示例：

![img](Linux基础知识/image-20220111165333404.png)行数据操作：

![image-20220111165527016](Linux基础知识/image-20220111165527016.png)

 第一个结果是输出两个第二行，因为sed命令会把所有的数据都输出到屏幕上，所以可以加上-n选项。

**删除行：**

![img](Linux基础知识/image-20220111165735405.png)需要注意的是，sed只是对输出到屏幕的数据进行了修改，而没有修改文件本身。

**追加：**

![image-20220111170018178](Linux基础知识/image-20220111170018178.png)

**替换：**

![image-20220111170914400](Linux基础知识/image-20220111170914400.png)

![img](Linux基础知识/image-20220111172121130.png)

再次注意，这种替换是不影响文件本身的。

#### 8.7.3、字符处理命令

1、排序命令sort

语法：

![image-20220111172751702](Linux基础知识/image-20220111172751702.png)

示例：

![image-20220111172902275](Linux基础知识/image-20220111172902275.png)

![image-20220111173155849](Linux基础知识/image-20220111173155849.png)

2、统计命令wc

![image-20220111173528784](Linux基础知识/image-20220111173528784.png)



#### 8.7.4、条件判断

1、按照文件类型进行判断

![img](Linux基础知识/image-20220117110319536.png)两种判断格式：

![img](Linux基础知识/image-20220117111915110.png)![image-20220117111136709](Linux基础知识/image-20220117111136709.png)

2、按照文件权限进行判断

![image-20220117111634732](Linux基础知识/image-20220117111634732.png)

3、两个文件进行比较

![image-20220117111915110](Linux基础知识/image-20220117111915110.png)

示例：

![img](Linux基础知识/image-20220117111942293.png)4、两个整数之间的比较

![image-20220117112117509](Linux基础知识/image-20220117112117509.png)

5、字符串的判断

![img](Linux基础知识/image-20220117112243270.png)

示例：

![image-20220117112420784](Linux基础知识/image-20220117112420784.png)

![image-20220117112444633](Linux基础知识/image-20220117112444633.png)

6、多重条件判断

![image-20220117112745029](Linux基础知识/image-20220117112745029.png)

判断语句的结果不是给用户来看的，主要是给计算机看的。

与java判断语句有点差别，但是逻辑都是一样的！

#### 8.7.5、流程控制