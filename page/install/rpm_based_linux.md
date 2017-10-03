# 在支持RPM包管理器的Linux安装（RHEL, CentOS, Fedora, openSUSE）

## 1. 下载服务端程序 {#install-1}

|  | Description | Download |
| :--- | :--- | :--- |
| RPM for RHEL 7.x, CentOS 7.x, Fedora 19+ \(supports systemd, from [Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)\) | [rabbitmq-server-3.6.12-1.el7.noarch.rpm](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el7.noarch.rpm) | [\(Signature\)](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el7.noarch.rpm.asc) |
| RPM for RHEL Linux 6.x, CentOS 6.x, Fedora prior to 19 \(from [Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)\) | [rabbitmq-server-3.6.12-1.el6.noarch.rpm](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el6.noarch.rpm) | [\(Signature\)](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el6.noarch.rpm.asc) |
| RPM for RHEL Linux 7.x, CentOS 7.x, Fedora 19+ \(supports systemd, from [GitHub](https://github.com/rabbitmq/rabbitmq-server/releases)\) | [rabbitmq-server-3.6.12-1.el7.noarch.rpm](https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_12/rabbitmq-server-3.6.12-1.el7.noarch.rpm) | [\(Signature\)](https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_12/rabbitmq-server-3.6.12-1.el7.noarch.rpm.asc) |
| RPM for RHEL Linux 6.x, CentOS 6.x, Fedora prior to 19 \(from [GitHub](https://github.com/rabbitmq/rabbitmq-server/releases)\) | [rabbitmq-server-3.6.12-1.el6.noarch.rpm](https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_12/rabbitmq-server-3.6.12-1.el6.noarch.rpm) | [\(Signature\)](https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_12/rabbitmq-server-3.6.12-1.el6.noarch.rpm.asc) |
| RPM for openSUSE Linux \(from [Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)\) | [rabbitmq-server-3.6.12-1.suse.noarch.rpm](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.suse.noarch.rpm) | [\(Signature\)](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.suse.noarch.rpm.asc) |
| RPM for SLES 11.x \(from [Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)\) | [rabbitmq-server-3.6.12-1.sles11.noarch.rpm](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.sles11.noarch.rpm) | [\(Signature\)](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.sles11.noarch.rpm.asc) |

## 2. 概述 {#install-2}

Fedora系统自带有rabbitmq-server，但是版本很旧。 所以最好就是从[PackageCloud](https://packagecloud.io/rabbitmq/rabbitmq-server/)或者[Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)下载`.rpm`文件来安装. 可以在 [Fedora package](https://admin.fedoraproject.org/updates/rabbitmq-server) 找到对应每个系统版本的rabbitmq-server。

从[PackageCloud](https://packagecloud.io/rabbitmq/rabbitmq-server/) 和 [Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)这两个Yum仓库可以找到rpm安装包

## 3. 可用的rpm分发版本 {#install-3}

比如RabbitMQ 3.6.3这个版本，它能在以下的支持rpm包管理器Linux分发版本上安装运行：

* CentOS 6.x and 7.x \(note: there are two separate RPM packages for 6.x and 7.x\)
* RedHat Enterprise Linux 6.x and 7.x \(same as for CentOS, there are two separate packages\)
* Fedora 23 through 25 \(use the RHEL 7.x package\)

The packages may work on other RPM-based distributions if dependencies \(see below\) are satisfied \(e.g. using the Wheezy backports repository\) but their testing and support is done on a best effort basis.

## 4. 安装 Erlang {#install-4}

安装RabbitMQ前需要安装Erlang或OTP，具体支持的版本请看[这里](https://www.rabbitmq.com/which-erlang.html)。我们强烈建议使用一个打包好的版本，以下有三个建议的安装渠道：

* [RabbitMQ提供的Erlang包](https://github.com/rabbitmq/erlang-rpm)。**It may not be as up to date as Erlang Solutions' packages**, but it might be easiest to use if installing Erlang's dependencies is proving difficult.
* [Erlang Solutions提供的Erlang包](https://www.erlang-solutions.com/resources/download.html)。 They produce two sets of packages - ones which are split up and are more convenient to use if you can add a yum repository, and a monolithic package which might be easier if you have to download manually.
* [EPEL](http://fedoraproject.org/wiki/EPEL) \("Extra Packages for Enterprise Linux"\); part of the Red Hat / Fedora organisation, provides many additional packages, including Erlang. These are the most official packages, and are split into many small packages, but are not always up to date.

**以下是可选的安装Erlang的途径：**

### 4.1 从RabbitMQ安装零依赖的Erlang {#install-4-1}

1. 下载并安装 the [zero dependency Erlang RPM package for running RabbitMQ](https://github.com/rabbitmq/erlang-rpm) . 这个包只包含RabbitMQ必须的模块。

### 4.2 从EPEL仓库安装Erlang {#install-4-2}

1. 通过以下的方式可以启用EPEL（[EPEL FAQ](#)）  
   For EL5:

   ```
   su -c 'rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm'
   ...
   su -c 'yum install foo'
   ```

   For EL6:

   ```
   su -c 'rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm'
   ...
   su -c 'yum install foo'
   ```

   For EL7:

   ```
   su -c 'rpm -Uvh http://download.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-10.noarch.rpm'
   ...
   su -c 'yum install foo'
   ```

   For EL7:

   ```
   yum install -y epel-release
   ...
   su -c 'yum install foo'
   ```

2. 使用'root'用户执行一下命令即可安装Erlang：

   ```
   yum install erlang
   ```

### 4.3 从Erlang Solutions仓库安装Erlang {#install-4-3}

1. Follow the instructions under "Installation using repository" at [Erlang Solutions](https://www.erlang-solutions.com/resources/download.html). Note that Erlang Solutions tend to provide cutting edge Erlang versions that may or may not be [supported by RabbitMQ](https://www.rabbitmq.com/which-erlang.html). Version locking \(see below\) is recommended when Erlang installed using this option.

### 4.4 从Erlang Solutions安装完整的Erlang {#install-4-4}

1. Download and install the [appropriate](https://www.rabbitmq.com/which-erlang.html) esl-erlang RPM from [Erlang Solutions](https://www.erlang-solutions.com/resources/download.html).
2. Download and install the [esl-erlang-compat](https://github.com/jasonmcintosh/esl-erlang-compat) RPM \([direct download](https://github.com/jasonmcintosh/esl-erlang-compat/releases/download/1.1.1/esl-erlang-compat-18.1-1.noarch.rpm)\) produced by [Jason McIntosh](https://github.com/jasonmcintosh/). 

This is needed since Erlang Solutions' monolithic packages provide "esl-erlang"; this package just requires "esl-erlang" and provides "erlang".

### 4.5 Package Version Locking in Yum {#install-4-5}

[yum version locking](https://access.redhat.com/solutions/98873) plugin is recommended to prevent unwanted Erlang upgrades. This is highly recommended when Erlang is installed via the Erlang Solutions repository.

## 5. 安装RabbitMQ Server {#install-5}

### 5.1 rpm安装 {#install-5-1}

下载rabbitmq-server的rpm包后用'root'用户执行一下命令安装:

```
rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
        yum install rabbitmq-server-3.6.12-1.noarch.rpm
```

Our public signing key is also [available from Bintray](https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc):

```
rpm --import https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
        yum install rabbitmq-server-3.6.12-1.noarch.rpm
```

### 5.2 用PackageCloud RPM仓库安装 {#install-5-2}

PackageCloud installs packages via HTTPS and signs them using their GPG key. There are multiple ways to install:

* Provided installation scripts
* Using PackageCloud Chef cookbook
* Using PackageCloud Puppet module
* Manually

See [PackageCloud RabbitMQ repository instructions](https://packagecloud.io/rabbitmq/rabbitmq-server/install)

## 6. 运行 RabbitMQ Server {#install-6}

### 6.1 自定义RabbitMQ环境变量 {#install-6-1}

RabbitMQ服务端应该使用默认设置启动的，但你也可以[自定义RabbitMQ的环境变量](https://www.rabbitmq.com/configure.html#customise-general-unix-environment). Also see how to [configure components](https://www.rabbitmq.com/configure.html#configuration-file).

### 6.2 启动服务端 {#install-6-2}

刚安装的RabbitMQ server默认是不会以后台进程的方式启动的。执行命令： `chkconfig rabbitmq-server on` 即可设置当系统启动后以后台进程方式运行。

开启或关闭的命令：`/sbin/service rabbitmq-server stop/start`.

**注意**: 服务端是以名称为`rabbitmq`的系统用户身份运行的。如果你修改了`Mnesia`数据库的位置或日志文件的位置，需要确保新目录是`rabbitmq`这用户的（该用户具有访问权限）。

RabbitMQ的服务端默认是不会以后台进程的方式运行的。需要先设置后再启动。官网提供的`chkconfig`、`service`命令都**会重定向到**`systemctl`命令，因此我直接列出`systemctl`的命令：

| 功能 | 命令 |
| --- | --- |
| 设置后台运行 | **systemctl **`enable`** rabbitmq-server.service** |
| 检查设置状态 | **systemctl **`is-enabled`** rabbitmq-server.service** |
| 启动 | **systemctl **`start`** rabbitmq-server.service** |
| 检查运行状态 | **systemctl **`status`** rabbitmq-server.service** |
| 停止 | **systemctl **`stop`** rabbitmq-server.service** |

## 7. 关于端口 {#install-7}

SELinux（linux的安全子系统）及其它类似的机制会阻止RabbitMQ绑定端口。如果绑定端口失败，RabbitMQ会启动失败。防火墙可以阻止来自外部其它机器的访问。所以请确保以下端口是可以访问的：

| 端口 | 用途 |
| --- | --- |
| 4369 | [epmd](http://erlang.org/doc/man/epmd.html), a peer discovery service used by RabbitMQ nodes and CLI tools |
| 5672, 5671 | used by AMQP 0-9-1 and 1.0 clients without and with TLS |
| 25672 | used by Erlang distribution for inter-node and CLI tools communication and is allocated from a dynamic range \(limited to a single port by default, computed as AMQP port + 20000\). See [networking guide](https://www.rabbitmq.com/networking.html) for details. |
| 15672 | [HTTP API](https://www.rabbitmq.com/management.html) clients and [rabbitmqadmin](https://www.rabbitmq.com/management-cli.html) \(only if the [management plugin](https://www.rabbitmq.com/management.html) is enabled\) |
| 61613, 61614 | [STOMP clients](https://stomp.github.io/stomp-specification-1.2.html) without and with TLS \(only if the [STOMP plugin](https://www.rabbitmq.com/stomp.html) is enabled\) |
| 1883, 8883 | \( [MQTT clients](http://mqtt.org/) without and with TLS, if the [MQTT plugin](https://www.rabbitmq.com/mqtt.html) is enabled |
| 15674 | STOMP-over-WebSockets clients \(only if the [Web STOMP plugin](https://www.rabbitmq.com/web-stomp.html) is enabled\) |
| 15675 | MQTT-over-WebSockets clients \(only if the [Web MQTT plugin](https://www.rabbitmq.com/web-mqtt.html) is enabled\) |

It is possible to [configure RabbitMQ](https://www.rabbitmq.com/configure.html) to use [different ports and specific network interfaces](https://www.rabbitmq.com/networking.html).

## 8. 默认用户 {#install-8}

RabbitMQ默认用户帐号及密码都是\`guest\`. Unconfigured clients will in general use these credentials.默认只能从本机访问。因此如果需要从其它机器访问RabbitMQ，需要做些设置。

请看[access control](https://www.rabbitmq.com/access-control.html)相关的内容，里面会讲解如何创建用户，删除`guest`用户, 或设置允许`guest`用户远程访问RabbitMQ.

## 9. 调整Linux系统的限制 {#install-9}

RabbitMQ installations running production workloads may need system limits and kernel parameters tuning in order to handle a decent number of concurrent connections and queues. The main setting that needs adjustment is the max number of open files, also known asulimit -n. The default value on many operating systems is too low for a messaging broker \(eg. 1024 on several Linux distributions\). We recommend allowing for at least 65536 file descriptors for user rabbitmq in production environments. 4096 should be sufficient for most development workloads.

RabbitMQ如果是在生产环境中运行的话，需要调整系统限制和内核限制，以应对大量并发连接和高容量的队列。要调整的主要是**允许打开文件的最大数量**，即`ulimit -n`命令显示的值。该默认值对消息中间件来说太低了（一般是1024）。我们推荐在生产环境中对系统用户`rabbitmq`（默认运行RabbitMQ server的用户）至少要设置成**65536**，如果是开发环境则设置成**4096。**

实际上有两种限制：**操作系统**内核允许打开文件的最大数量\(fs.file-max\)、**每个用户**允许打开文件的最大数量\(ulimit -n\)。前者必须高于后者。

### 9.1 用systemd来调整（新版的Linux） {#install-9-1}

在使用systemd工具的Linux系统中,**操作系统的限制**是通过配置这个文件来控制的：`/etc/systemd/system/rabbitmq-server.service.d/limits.conf`, 例如可以设置成这样（如无文件请自行新建）:

```
[Service]
LimitNOFILE=300000
```

### 9.2 不用systemd来调整（旧版的Linux） {#install-9-2}

不用`systemd`工具来调整每个用户的限制的最直接方式是编辑[rabbitmq-env.conf](http://www.rabbitmq.com/configure.html)文件，编辑后，当RabbitMQ server重启时会执行ulimit命令，从而达到设置**每个用户允许打开文件的最大数量**的目的：

```
ulimit -S -n 4096
```

以上通过执行`ulimit`命令设置的方式，是**soft limit**，而**soft limit不能高于hard limit**（很多linux系统一般设置的hard limit值是4096）。hard limit可以在配置文件`/etc/security/limits.conf`中设置。设置hard limit同时还需要启用[pam\_limits.so](http://askubuntu.com/a/34559)模块，最后还要重新登录或重启一下系统。

**设置hard limit：**

```
# 编辑文件
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

# 行末添加：
session    required    pam_limits.so
# 这是告诉Linux在用户完成系统登录后，应该调用pam_limits.so模块设置
# 系统对该用户可使用的各种资源数量的最大限制(包括用户可打开的最大文件数限制)
```

**最后要重新登录或重启系统，以使hard limit设置生效。**

**注意：**运行中的RabbitMQ进程的限制值是无法修改的。因此设置后需要重启一下RabbitMQ：

```
systemctl daemon-reload
systemctl  restart rabbitmq-server.service
```

For more information about controlling fs.file-max with sysctl, please refer to the excellent [Riak guide on open file limit tuning](http://docs.basho.com/riak/latest/ops/tuning/open-files-limit/#Linux).

### 9.3 验证调整是否成功 {#install-9-3}

[RabbitMQ management UI](https://www.rabbitmq.com/management.html)可以通过网页的形式查看剩余可用的文件打开时数量。而以下是通过命令行方式查看：

```
# 查看RabbitMQ server的状态，记住进程id：pid
rabbitmqctl status

# 查看设置的limits，$RABBITMQ_BEAM_PROCESS_PID用实际的进程id代替
cat /proc/$RABBITMQ_BEAM_PROCESS_PID/limits
```

### 9.4 用配置管理工具来调整 {#install-9-4}

我们也可以用配置管理工具来调整系统限制的值（最大文件打开数量），如：Chef, Puppet, BOSH等。我们的[developer tools](https://www.rabbitmq.com/devtools.html#devops-tools)也列出了一些相关的模块和项目，欲了解请前往查阅。

## 10. 管理消息中间件 {#install-10}

我们可以用service工具来启动或停止RabbitMQ server。也可以用RabbitMQ提供的rabbitmqctl工具（管理员权限打开）。正常的话该工具已经加入到系统环境变量Path中的了，可以直接调用。调用示例如下：

* 停止：rabbitmqctl stop
* 查看状态：rabbitmqctl status

更多信息请看： [info on rabbitmqctl](https://www.rabbitmq.com/man/rabbitmqctl.1.man.html).

### 10.1 日志 {#install-10-1}

Output from the server is sent to a**RABBITMQ\_NODENAME**.log file in the**RABBITMQ\_LOG\_BASE**directory. Additional log data is written to**RABBITMQ\_NODENAME**-sasl.log.

The broker always appends to the log files, so a complete log history is retained.

You can use thelogrotateprogram to do all necessary rotation and compression, and you can change it. By default, this script runs weekly on files located in default/var/log/rabbitmqdirectory. See/etc/logrotate.d/rabbitmq-serverto configurelogrotate.

