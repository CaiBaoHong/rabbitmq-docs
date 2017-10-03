# RabbitMQ快速安装配置指南

>官网的安装教程由于需要解释原理很多废话，这里总结一下在CentOS7环境下的安装配置过程。

## 1. 安装RabbitMQ server

~~~
## 安装epel源
yum install -y epel-release

## 安装Erlang
yum install -y erlang

## 安装RabbitMQ server，请自行到官网下载rpm包
yum install -y rabbitmq-server-3.6.12-1.el7.noarch.rpm
~~~

## 2. 启动RabbitMQ server
~~~
## 设置RabbitMQ以后台方式运行
systemctl enable rabbitmq-server.service

## 启动
systemctl start rabbitmq-server.service

## 查询状态
systemctl status rabbitmq-server.service
~~~

## 3.调整系统限制
~~~
## vi工具打开，没有这文件就创建一个
vi /etc/systemd/system/rabbitmq-server.service.d/limits.conf

## 文件添加内容：
[Service]
LimitNOFILE=65000

~~~
