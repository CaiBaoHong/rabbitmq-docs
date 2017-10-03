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