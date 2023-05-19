## 一、Linux安装

### 1.1、安装RabbitMQ

**1、下载**

官网下载地址：[https://www.rabbitmq.com/download.html(opens new window)](https://www.rabbitmq.com/download.html)

这里我们选择的版本号（注意这两版本要求）

- rabbitmq-server-3.8.8-1.el7.noarch.rpm

  GitHub：[https://github.com/rabbitmq/rabbitmq-server/releases/tag/v3.8.8(opens new window)](https://github.com/rabbitmq/rabbitmq-server/releases/tag/v3.8.8)

  加载下载：[https://packagecloud.io/rabbitmq/rabbitmq-server/packages/el/7/rabbitmq-server-3.8.8-1.el7.noarch.rpm(opens new window)](https://packagecloud.io/rabbitmq/rabbitmq-server/packages/el/7/rabbitmq-server-3.8.8-1.el7.noarch.rpm)

- erlang-21.3.8.21-1.el7.x86_64.rpm

  官网：https://www.erlang-solutions.com/downloads/

  加速：[https://packagecloud.io/rabbitmq/erlang/packages/el/7/erlang-21.3.8.21-1.el7.x86_64.rpm(opens new window)](https://packagecloud.io/rabbitmq/erlang/packages/el/7/erlang-21.3.8.21-1.el7.x86_64.rpm)

Red Hat 8, CentOS 8 和 modern Fedora 版本，把 “el7” 替换成 “el8”

**2、安装**

上传到 `/usr/local/software` 目录下(如果没有 software 需要自己创建)

```sh
rpm -ivh erlang-21.3.8.21-1.el7.x86_64.rpm
//依赖包
yum install socat -y
rpm -ivh rabbitmq-server-3.8.8-1.el7.noarch.rpm
```

**3、启动**

```sh
# 启动服务
systemctl start rabbitmq-server
# 查看服务状态
systemctl status rabbitmq-server
# 开机自启动
systemctl enable rabbitmq-server
# 停止服务
systemctl stop rabbitmq-server
# 重启服务
systemctl restart rabbitmq-server
```



### Web管理界面及授权操作

**1、安装**

默认情况下，是没有安装web端的客户端插件，需要安装才可以生效

```sh
rabbitmq-plugins enable rabbitmq_management
```



安装完毕以后，重启服务即可

```sh
systemctl restart rabbitmq-server
```



访问 http://192.168.140.128:15672 

如果不能访问，那么就查看虚拟机的防火墙是否关闭。

用默认账号密码(guest)登录，出现权限问题。

默认情况只能在 localhost 本机下访问，所以需要添加一个远程登录的用户

**2、添加用户**

```sh
# 创建账号和密码
rabbitmqctl add_user root root

# 设置用户角色
rabbitmqctl set_user_tags root administrator

# 为用户添加资源权限
# set_permissions [-p <vhostpath>] <user> <conf> <write> <read>
rabbitmqctl set_permissions -p "/" root ".*" ".*" ".*"
# 添加配置、写、读权限
# 查看所有用户列表
rabbitmqctl list_users
```



用户级别：

1. **administrator**：可以登录控制台、查看所有信息、可以对 rabbitmq 进行管理
2. **monitoring**：监控者 登录控制台，查看所有信息
3. **policymaker**：策略制定者 登录控制台，指定策略
4. **managment**：普通管理员 登录控制台

再次登录，用 admin 用户

重置命令

关闭应用的命令为：rabbitmqctl stop_app

清除的命令为：rabbitmqctl reset

重新启动命令为：rabbitmqctl start_app

## 二、Docker 安装

官网：[https://registry.hub.docker.com/_/rabbitmq/(opens new window)](https://registry.hub.docker.com/_/rabbitmq/)

```sh
docker run -id --name myrabbit -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=123456 -p 15672:15672 rabbitmq:3-management
```

