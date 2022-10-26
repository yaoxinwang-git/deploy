

### 1、下载好rpm，解压当前文件夹

```
##安装rabbitmq需要安装erlang
rpm -ivh erlang-23.2.7-1.el7.x86_64.rpm
##查看erlang版本检查erlang是否安装正确
erl -version

##安装rabbitmq
rpm -Uvh rabbitmq-server-3.9.15-1.el7.noarch.rpm

##安装完成后，可以查看下他的安装情况
whereis rabbitmqctl
```


### 2、配置下rabbitmq的Web管理页面：

```
rabbitmq-plugins enable rabbitmq_management
```

### 3、创建一个管理员账户
使用默认的guest/guest用户登录。 但是注意下，默认情况下，只允许在localhost本地登录，远程访问是无法登录的。这时，可以创建一个管理员账户来登录。
```
rabbitmqctl add_user admin admin
rabbitmqctl set_permissions -p / admin "." "." ".*"
rabbitmqctl set_user_tags admin administrator

```






### 4、常用操作

```
service rabbitmq-server start
service rabbitmq-server status


rabbitmq-server -deched  --后台启动服务
rabbitmqctl start_app  --启动服务 
rabbitmqctl stop_app  --关闭服务 
```


