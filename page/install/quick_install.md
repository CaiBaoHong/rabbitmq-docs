# RabbitMQ快速安装配置指南

> 官网的安装教程由于需要解释原理很多废话，这里总结一下在CentOS7环境下的安装配置过程。

## 1. 安装RabbitMQ server

```
## 安装epel源
yum install -y epel-release

## 安装Erlang
yum install -y erlang

## 安装RabbitMQ server，请自行到官网下载rpm包
yum install -y rabbitmq-server-3.6.12-1.el7.noarch.rpm
```

## 2. 启动RabbitMQ server

```
## 设置RabbitMQ以后台方式运行
systemctl enable rabbitmq-server.service

## 启动
systemctl start rabbitmq-server.service

## 查询状态
systemctl status rabbitmq-server.service
```

## 3.调整系统限制

**调整操作系统允许打开文件的最大数量**

```
## vi工具打开，没有这文件就创建一个
vi /etc/systemd/system/rabbitmq-server.service.d/limits.conf

## 文件添加内容：
[Service]
LimitNOFILE=300000
```

**hard limit方式设置每个用户允许打开文件的最大数量：**

```
## vi工具打开，没有这文件就创建一个
vi /etc/security/limits.conf

# 行末添加：
* soft nofile 65536
* hard nofile 65536
* soft nproc 65536
* hard nproc 65536
```

**启用pam\_limits.so模块：**

```
# 编辑文件
vi /etc/pam.d/login

## 文件末尾添加内容：
session required pam_limits.so
# 这是告诉Linux在用户完成系统登录后，应该调用pam_limits.so模块设置
# 系统对该用户可使用的各种资源数量的最大限制(包括用户可打开的最大文件数限制)
```



