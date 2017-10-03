# RabbitMQ Configuration

## Overview

RabbitMQ默认的设置能够适应部分使用场景，如开发和QA。如果运行没问题，那就不用特别配置。而对于其它情况，如[生产环境](https://www.rabbitmq.com/production-checklist.html), 我们是可以对消息中间件和插件进行很多的配置的。

在这里讨论配置相关的话题如下:

* 配置服务端和插件的途径
* 配置文件
* 环境变量
* 服务端常用配置
* 如何验证配置

Since configuration affects many areas of the system, including plugins, individual [documentation guides](https://www.rabbitmq.com/documentation.html) dive deeper into what can be configured.

## 配置的手段

RabbitMQ提供以下途径可以配置服务端:

| 途径 | 描述 |
| :--- | :--- |
| [环境变量](https://www.rabbitmq.com/configure.html#define-environment-variables) | 定义节点名、文件和目录的位置, runtime flags \(从shell中获取，或在环境配置文件中设置,即：rabbitmq-env.conf或rabbitmq-env-conf.bat\) |
| [配置文件](https://www.rabbitmq.com/configure.html#configuration-file) | defines server and plugin settings for[TCP listeners and other networking-related settings](https://www.rabbitmq.com/networking.html)[TLS](https://www.rabbitmq.com/ssl.html)[resource constraints \(alarms\)](https://www.rabbitmq.com/alarms.html)[authentication and authorization backends](https://www.rabbitmq.com/access-control.html)[message store settings](https://www.rabbitmq.com/persistence-conf.html)and so on. |
| [运行时参数和策略](https://www.rabbitmq.com/parameters.html) | defines cluster-wide settings which can change at run time as well as settings that are convenient to configure for groups of queues \(exchanges, etc\) such as including optional queue arguments. |

Most settings are configured using the first two methods. This guide, therefore, focuses on them.

### Config File Locations

因操作系统和[package types](https://www.rabbitmq.com/download.html)的不同，[默认配置文件的位置](https://www.rabbitmq.com/configure.html#config-location)也不尽相同。
This topic is covered in more details in the rest of this guide.

如果不知道RabbitMQ的配置文件的位置，可以查看日志文件和管理后台，就能知道配置文件的位置了。

### 如何确定配置文件的位置

我们可以在[日志文件](https://www.rabbitmq.com/relocate.html)中找到当前使用中的配置文件信息，如:

```
node           : rabbit@example
home dir       : /var/lib/rabbitmq
config file(s) : /etc/rabbitmq/rabbitmq.config
```

如果找不到配置文件，会有写明not found，如：

```
node           : rabbit@example
home dir       : /var/lib/rabbitmq
config file(s) : /var/lib/rabbitmq/hare.config (not found)
```

另外，在[管理后台](https://www.rabbitmq.com/management.html)中也可以看到配置文件的位置信息。

当我们调试解决配置问题时，查看配置文件位置是否正确、文件是否存在、文件是否能被加载，是非常有用的，

### 如何验证配置是否成功

通过[rabbitmqctl environment](https://www.rabbitmq.com/man/rabbitmqctl.1.man.html)命令可以打印配置信息并验证配置是否成功。

## 自定义RabbitMQ的环境

有些服务端参数是可以通过环境变量设置的：节点名，配置文件的位置，inter-node communication ports, Erlang VM flags等等。

### 在类Unix系统上自定义RabbitMQ的环境

在类Unix系统上，可以通过创建或编辑`rabbitmq-env.conf`文件来定义环境变量。`rabbitmq-env.conf`文件的[位置](https://www.rabbitmq.com/configure.html#config-location)可以通过`RABBITMQ_CONF_ENV_FILE`环境变量来设定。

`rabbitmq-env.conf`文件配置示例(不用加`RABBITMQ_`前缀)：

```
#example rabbitmq-env.conf file entries
#Rename the node

NODENAME=bunny@myhost

#Config file location and new filename bunnies.config

CONFIG_FILE=/etc/rabbitmq/testdir/bunnies
```

**注意：**
- 配置文件：rabbitmq.config-->可通过环境变量配置文件来设置
- 环境变量配置文件：rabbitmq-env.conf-->可通过`RABBITMQ_CONF_ENV_FILE`环境变量来设置


关于`rabbitmq-env.conf`文件的更多信息请看：[info on using rabbitmq-env.conf](https://www.rabbitmq.com/man/rabbitmq-env.conf.5.man.html)

### 在类Windows系统上自定义RabbitMQ的环境

If you need to customise names, ports, locations, it is easiest to configure environment variables in the Windows dialogue: Start &gt; Settings &gt; Control Panel &gt; System &gt; Advanced &gt; Environment Variables. Then create or edit the system variable name and value.

Alternatively, you can create/editrabbitmq-env-conf.batto define environment variables. Its[location](https://www.rabbitmq.com/configure.html#config-location)is configurable using theRABBITMQ\_CONF\_ENV\_FILEenvironment variable.

_For environment changes to take effect on Windows, the service must be re-installed_. It is\_not sufficient\_to restart the service. This can be done using the installer or on the command line with administrator permissions:

* [Start an admin command prompt](https://technet.microsoft.com/en-us/library/cc947813%28v=ws.10%29.aspx)
* cd into the sbin folder under
  _RabbitMQ server installation directory_
  \(e.g.
  C:\Program Files \(x86\)\RabbitMQ Server\rabbitmq\_server-3.6.12\sbin
  \)
* Run
  rabbitmq-service.bat remove
* Set environment variables via command line, i.e. run commands like the following:
  set RABBITMQ\_BASE=c:\Data\RabbitMQ
* Run
  rabbitmq-service.bat install

Alternative, if new configuration needs to take effect after next broker restart, one step can be skipped:

* [Start an admin command prompt](https://technet.microsoft.com/en-us/library/cc947813%28v=ws.10%29.aspx)
* cd into the sbin folder under
  _RabbitMQ server installation directory_
* Set environment variables via command line
* Run
  rabbitmq-service.bat install
  , which will only update service parameters

## RabbitMQ Environment Variables

RabbitMQ environment variable names have the prefixRABBITMQ\_. A typical variable calledRABBITMQ\_var\_nameis set as follows:

* a shell environment variable called
  RABBITMQ\_
  var\_name
  is used if this is defined;
* otherwise
  , a variable called
  var\_name
  is used if this is set in the
  rabbitmq-env.conf
  file;
* otherwise
  , a system-specified default value is used.

In this way, variables set in the shell environment take priority over variables set inrabbitmq-env.conf, which in turn over-ride RabbitMQ built-in defaults.

It is unlikely you will need to set any of these environment variables. If you have non-standard requirements, then RabbitMQ environment variables include, but are not limited to:

| Name | Default | Description |
| :--- | :--- | :--- |
| RABBITMQ\_NODE\_IP\_ADDRESS | the empty string - meaning bind to all network interfaces. | Change this if you only want to bind to one network interface. To bind to two or more interfaces, use thetcp\_listenerskey inrabbitmq.config. |
| RABBITMQ\_NODE\_PORT | 5672 |  |
| RABBITMQ\_DIST\_PORT | RABBITMQ\_NODE\_PORT + 20000 | Port used for inter-node and CLI tool communition. Ignored if your config file setskernel.inet\_dist\_listen\_minorkernel.inet\_dist\_listen\_maxkeys. See[Networking](https://www.rabbitmq.com/networking.html)for details. |
| RABBITMQ\_NODENAME | **Unix\*:**rabbit@$HOSTNAME**Windows:**rabbit@%COMPUTERNAME% | The node name should be unique per erlang-node-and-machine combination. To run multiple nodes, see the[clustering guide](https://www.rabbitmq.com/clustering.html). |
| RABBITMQ\_CONF\_ENV\_FILE | **Generic UNIX**-$RABBITMQ\_HOME/etc/rabbitmq/rabbitmq-env.conf**Debian**-/etc/rabbitmq/rabbitmq-env.conf**RPM**-/etc/rabbitmq/rabbitmq-env.conf**Mac OS X \(Homebrew\)**-${install\_prefix}/etc/rabbitmq/rabbitmq-env.conf, the Homebrew prefix is usually/usr/local**Windows**-%APPDATA%\RabbitMQ\rabbitmq-env-conf.bat | Location of the file that contains environment variable definitions \(without theRABBITMQ\_prefix\). Note that the file name on Windows is different from other operating systems. |
| RABBITMQ\_USE\_LONGNAME |  | When set totruethis will cause RabbitMQ to use fully qualified names to identify nodes. This may prove useful on EC2. Note that it is not possible to switch between using short and long names without resetting the node. |
| RABBITMQ\_SERVICENAME | **Windows Service:**RabbitMQ | The name of the installed service. This will appear inservices.msc. |
| RABBITMQ\_CONSOLE\_LOG | **Windows Service:** | Set this variable toneworreuseto redirect console output from the server to a file named%RABBITMQ\_SERVICENAME%.debug in the default**RABBITMQ\_BASE**directory.If not set, console output from the server will be discarded \(default\).newA new file will be created each time the service starts.reuseThe file will be overwritten each time the service starts. |
| RABBITMQ\_CTL\_ERL\_ARGS | None | Parameters for theerlcommand used when invokingrabbitmqctl. This should be overridden for debugging purposes only. |
| RABBITMQ\_SERVER\_ERL\_ARGS | **Unix\*:**+P 1048576 +t 5000000 +stbt db +zdbbl 32000**Windows:**None | Standard parameters for theerlcommand used when invoking the RabbitMQ Server. This should be overridden for debugging purposes only. Overriding this variable\_replaces\_the default value. |
| RABBITMQ\_SERVER\_ADDITIONAL\_ERL\_ARGS | **Unix\*:**None**Windows:**None | Additional parameters for theerlcommand used when invoking the RabbitMQ Server. The value of this variable is\_appended\_the default list of arguments \(**RABBITMQ\_SERVER\_ERL\_ARGS**\). This is the environment variable to use if+K trueneeds to be overwritten. |
| RABBITMQ\_SERVER\_START\_ARGS | None | Extra parameters for theerlcommand used when invoking the RabbitMQ Server. This will not override**RABBITMQ\_SERVER\_ERL\_ARGS**. |

\* Unix, Linux, MacOSX

In addition, there are several environment variables which tell RabbitMQ[where to locate its database, log files, plugins, configuration etc](https://www.rabbitmq.com/relocate.html).

Other variables upon which RabbitMQ depends are:

| Name | Default | Description |
| :--- | :--- | :--- |
| HOSTNAME | **Unix, Linux:**env hostname**MacOSX:**env hostname -s | The name of the current machine |
| COMPUTERNAME | **Windows:**localhost | The name of the current machine |
| ERLANG\_SERVICE\_MANAGER\_PATH | **Windows Service:**%ERLANG\_HOME%\erts-x.x.x\bin | This path is the location oferlsrv.exe, the Erlang service wrapper script. |

## Configuration File

### The rabbitmq.config File

The configuration filerabbitmq.configallows the RabbitMQ core application, Erlang services and RabbitMQ plugins to be configured. It is a standard Erlang configuration file, documented on the[Erlang Config Man Page](http://www.erlang.org/doc/man/config.html).

An minimalistic example configuration file follows:

```
  [
    {rabbit, [{tcp_listeners, [
5673
]}]}
  ].
```

This example will alter the port RabbitMQ listens on for AMQP 0-9-1 client connections from 5672 to 5673.

To override main RabbitMQ config file location, use theRABBITMQ\_CONFIG\_FILEenvironment variable.

Note that this configuration file is not the same as the environment configuration file,rabbitmq-env.conf, which can be used to set environment variables on non-Windows systems.

### Location of rabbitmq.config and rabbitmq-env.conf

The location of these files is distribution-specific. By default, they are not created, but expect to be located in the following places on each platform:

* **Generic UNIX**
  -
  $RABBITMQ\_HOME
  /etc/rabbitmq/
* **Debian**
  -
  /etc/rabbitmq/
* **RPM**
  -
  /etc/rabbitmq/
* **Mac OS X \(Homebrew\)**
  -
  ${install\_prefix}
  /etc/rabbitmq/
  , the Homebrew prefix is usually
  /usr/local
* **Windows**
  -
  %APPDATA%
  \RabbitMQ\

Ifrabbitmq-env.confdoesn't exist, it can be created manually in the location, specified by theRABBITMQ\_CONF\_ENV\_FILEvariable. On Windows systems, it is namedrabbitmq-env.bat.

Ifrabbitmq.configdoesn't exist, it can be created manually. Set the**RABBITMQ\_CONFIG\_FILE**environment variable if you change the location. The Erlang runtime automatically appends the .config extension to the value of this variable.

Restart the server after changes. Windows service users will need to re-install the service after adding or removing a configuration file.

### Example rabbitmq.config File

RabbitMQ server source repository contains[an example configuration file](https://github.com/rabbitmq/rabbitmq-server/blob/stable/docs/rabbitmq.config.example)namedrabbitmq.config.example. This example file contains an example of most of the configuration items you might want to set \(with some very obscure ones omitted\) along with documentation for those settings. All configuration items are commented out in the example, so you can uncomment what you need. Note that the example file is meant to be used as, well, example, and should not be treated as a general recommendation.

In most distributions we place this example file in the same location as the real file should be placed \(see above\). However, for the Debian and RPM distributions policy forbids doing so; instead you can find it in/usr/share/doc/rabbitmq-server/or/usr/share/doc/rabbitmq-server-3.6.12/respectively.

### Variables Configurable in rabbitmq.config

Many users of RabbitMQ never need to change any of these values, and some are fairly obscure. However, for completeness they are all listed here.

| Key | Documentation |
| :--- | :--- |
| tcp\_listeners | Ports or hostname/pair on which to listen for AMQP connections \(without TLS\). See the[Networking guide](https://www.rabbitmq.com/networking.html)for more details and examples.Default:\[5672\] |
| num\_tcp\_acceptors | Number of Erlang processes that will accept connections for the TCP listeners.Default:10 |
| handshake\_timeout | Maximum time for AMQP 0-8/0-9/0-9-1 handshake \(after socket connection and SSL handshake\), in milliseconds.Default:10000 |
| ssl\_listeners | As above, for SSL connections.Default:\[\] |
| num\_ssl\_acceptors | Number of Erlang processes that will accept connections for the SSL listeners.Default:1 |
| ssl\_options | SSL configuration. See the[SSL documentation](https://www.rabbitmq.com/ssl.html#enabling-ssl).Default:\[\] |
| ssl\_handshake\_timeout | SSL handshake timeout, in milliseconds.Default:5000 |
| vm\_memory\_high\_watermark | Memory threshold at which the flow control is triggered. See the[memory-based flow control](https://www.rabbitmq.com/memory.html)documentation.Default:0.4 |
| vm\_memory\_calculation\_strategy | Strategy for memory usage reporting. Can be one of the following:rss- use operating system RSS memory reportingerlang- use Erlang memory reportingDefault:rss |
| vm\_memory\_high\_watermark\_paging\_ratio | Fraction of the high watermark limit at which queues start to page messages out to disc to free up memory. See the[memory-based flow control](https://www.rabbitmq.com/memory.html)documentation.Default:0.5 |
| disk\_free\_limit | Disk free space limit of the partition on which RabbitMQ is storing data. When available disk space falls below this limit, flow control is triggered. The value may be set relative to the total amount of RAM \(e.g.{mem\_relative, 1.0}\). The value may also be set to an integer number of bytes. Or, alternatively, in information units \(e.g"50MB"\). By default free disk space must exceed 50MB. See the[Disk Alarms](https://www.rabbitmq.com/disk-alarms.html)documentation.Default:50000000 |
| log\_levels | Controls the granularity of logging. The value is a list of log event category and log level pairs.The level can be one of 'none' \(no events are logged\), 'error' \(only errors are logged\), 'warning' \(only errors and warning are logged\), 'info' \(errors, warnings and informational messages are logged\), or 'debug' \(errors, warnings, informational messages and debugging messages are logged\).At present there are four categories defined. Other, currently uncategorised, events are always logged.The categories are:channel- for all events relating to AMQP channelsconnection- for all events relating to network connectionsfederation- for all events relating to[federation](https://www.rabbitmq.com/federation.html)mirroring- for all events relating to[mirrored queues](https://www.rabbitmq.com/ha.html)Default:\[{connection, info}\] |
| frame\_max | Maximum permissible size of a frame \(in bytes\) to negotiate with clients. Setting to 0 means "unlimited" but will trigger a bug in some QPid clients. Setting a larger value may improve throughput; setting a smaller value may improve latency.Default:131072 |
| channel\_max | Maximum permissible number of channels to negotiate with clients. Setting to 0 means "unlimited". Using more channels increases memory footprint of the broker.Default:0 |
| channel\_operation\_timeout | Channel operation timeout in milliseconds \(used internally, not directly exposed to clients due to messaging protocol differences and limitations\).Default:15000 |
| heartbeat | Value representing the heartbeat delay, in seconds, that the server sends in theconnection.tuneframe. If set to 0, heartbeats are disabled. Clients might not follow the server suggestion, see the[AMQP reference](https://www.rabbitmq.com/amqp-0-9-1-reference.html#connection.tune)for more details. Disabling heartbeats might improve performance in situations with a great number of connections, but might lead to connections dropping in the presence of network devices that close inactive connections.Default:60\(580prior to release 3.5.5\) |
| default\_vhost | Virtual host to create when RabbitMQ creates a new database from scratch. The exchangeamq.rabbitmq.logwill exist in this virtual host.Default:&lt;&lt;"/"&gt;&gt; |
| default\_user | User name to create when RabbitMQ creates a new database from scratch.Default:&lt;&lt;"guest"&gt;&gt; |
| default\_pass | Password for the default user.Default:&lt;&lt;"guest"&gt;&gt; |
| default\_user\_tags | Tags for the default user.Default:\[administrator\] |
| default\_permissions | [Permissions](https://www.rabbitmq.com/access-control.html)to assign to the default user when creating it.Default:\[&lt;&lt;".\*"&gt;&gt;, &lt;&lt;".\*"&gt;&gt;, &lt;&lt;".\*"&gt;&gt;\] |
| loopback\_users | List of users which are only permitted to connect to the broker via a loopback interface \(i.e.localhost\).If you wish to allow the defaultguestuser to connect remotely, you need to change this to\[\].Default:\[&lt;&lt;"guest"&gt;&gt;\] |
| cluster\_nodes | Set this to cause clustering to[happen automatically](https://www.rabbitmq.com/clustering.html#auto-config)when a node starts for the very first time. The first element of the tuple is the nodes that the node will try to cluster to. The second element is eitherdiscorramand determines the node type.Default:{\[\], disc} |
| server\_properties | List of key-value pairs to announce to clients on connection.Default:\[\] |
| collect\_statistics | Statistics collection mode. Primarily relevant for the management plugin. Options are:none \(do not emit statistics events\)coarse \(emit per-queue / per-channel / per-connection statistics\)fine \(also emit per-message statistics\)You probably don't want to change this yourself.Default:none |
| collect\_statistics\_interval | Statistics collection interval in milliseconds. Primarily relevant for the[management plugin](https://www.rabbitmq.com/management.html#statistics-interval).Default:5000 |
| management\_db\_cache\_multiplier | Affects the amount of time the[management plugin](https://www.rabbitmq.com/management.html#statistics-interval)will cache expensive management queries such as queue listings. The cache will multiply the elapsed time of the last query by this value and cache the result for this amount of time.Default:5 |
| auth\_mechanisms | [SASL authentication mechanisms](https://www.rabbitmq.com/authentication.html)to offer to clients.Default:\['PLAIN', 'AMQPLAIN'\] |
| auth\_backends | List of[authentication and authorisation backends](https://www.rabbitmq.com/access-control.html)to use.Other databases thanrabbit\_auth\_backend\_internalare available through[plugins](https://www.rabbitmq.com/plugins.html).Default:\[rabbit\_auth\_backend\_internal\] |
| reverse\_dns\_lookups | Set totrueto have RabbitMQ perform a reverse DNS lookup on client connections, and present that information throughrabbitmqctland the management plugin.Default:false |
| delegate\_count | Number of delegate processes to use for intra-cluster communication. On a machine which has a very large number of cores and is also part of a cluster, you may wish to increase this value.Default:16 |
| trace\_vhosts | Used internally by the[tracer](https://www.rabbitmq.com/firehose.html). You shouldn't change this.Default:\[\] |
| tcp\_listen\_options | Default socket options. You probably don't want to change this.Default:\[{backlog,       128}, {nodelay,       true}, {linger,        {true,0}}, {exit\_on\_close, false}\] |
| hipe\_compile | Set totrueto precompile parts of RabbitMQ with HiPE, a just-in-time compiler for Erlang. This will increase server throughput at the cost of increased startup time.You might see 20-50% better performance at the cost of a few minutes delay at startup. These figures are highly workload- and hardware-dependent.HiPE support may not be compiled into your Erlang installation. If it is not, enabling this option will just cause a warning message to be displayed and startup will proceed as normal. For example, Debian / Ubuntu users will need to install theerlang-base-hipepackage.HiPE is not available at all on some platforms, notably including Windows.HiPE has known issues in Erlang/OTP versions prior to 17.5. Using a recent Erlang/OTP version is highly recommended for HiPE.Default:false |
| cluster\_partition\_handling | How to handle network partitions. Available modes are:ignorepause\_minority{pause\_if\_all\_down, \[nodes\], ignore \| autoheal}where\[nodes\]is a list of node names \(ex:\['rabbit@node1', 'rabbit@node2'\]\)autohealSee the[documentation on partitions](https://www.rabbitmq.com/partitions.html#automatic-handling)for more information.Default:ignore |
| cluster\_keepalive\_interval | How frequently nodes should send keepalive messages to other nodes \(in milliseconds\). Note that this is not the same thing as[net\_ticktime](https://www.rabbitmq.com/nettick.html); missed keepalive messages will not cause nodes to be considered down.Default:10000 |
| queue\_index\_embed\_msgs\_below | Size in bytes of message below which messages will be embedded directly in the queue index. You are advised to read the[persister tuning](https://www.rabbitmq.com/persistence-conf.html)documentation before changing this.Default:4096 |
| msg\_store\_index\_module | Implementation module for queue indexing. You are advised to read the[persister tuning](https://www.rabbitmq.com/persistence-conf.html)documentation before changing this.Default:rabbit\_msg\_store\_ets\_index |
| backing\_queue\_module | Implementation module for queue contents. You probably don't want to change this.Default:rabbit\_variable\_queue |
| msg\_store\_file\_size\_limit | Tunable value for the persister. You almost certainly should not change this.Default:16777216 |
| msg\_store\_credit\_disc\_bound | The credits that a queue process is given by the message store.By default, a queue process is given 4000 message store credits, and then 800 for every 800 messages that it processes.Messages which need to be paged out due to memory pressure will also use this credit.The Message Store is the last component in the credit flow chain.[Learn about credit flow.](https://www.rabbitmq.com/blog/2015/10/06/new-credit-flow-settings-on-rabbitmq-3-5-5/)This value only takes effect when messages are persisted to the message store. If messages are embedded on the queue index, then modifying this setting has no effect because credit\_flow is NOT used when writing to the queue index.Default:{4000, 800} |
| mnesia\_table\_loading\_retry\_limit | Number of times to retry while waiting for Mnesia tables in a cluster to become available.Default:10 |
| mnesia\_table\_loading\_retry\_timeout | Time to wait per retry for Mnesia tables in a cluster to become available.Default:30000 |
| queue\_index\_max\_journal\_entries | Tunable value for the persister. You almost certainly should not change this.Default:65536 |
| queue\_master\_locator | Queue master location strategy. Available strategies are:&lt;&lt;"min-masters"&gt;&gt;&lt;&lt;"client-local"&gt;&gt;&lt;&lt;"random"&gt;&gt;See the[documentation on queue master location](https://www.rabbitmq.com/ha.html#queue-master-location)for more information.Default:&lt;&lt;"client-local"&gt;&gt; |
| mirroring\_sync\_batch\_size | Batch size of messages to synchronise between queue mirrors See[Batch Synchronization](https://www.rabbitmq.com/ha.html#batch-sync)Default:4096 |
| lazy\_queue\_explicit\_gc\_run\_operation\_threshold | Tunable value only for lazy queues when under memory pressure. This is the threshold at which the garbage collector and other memory reduction activities are triggered. A low value could reduce performance, and a high one can improve performance, but cause higher memory consumption. You almost certainly should not change this.Default:1000 |
| queue\_explicit\_gc\_run\_operation\_threshold | Tunable value only for normal queues when under memory pressure. This is the threshold at which the garbage collector and other memory reduction activities are triggered. A low value could reduce performance, and a high one can improve performance, but cause higher memory consumption. You almost certainly should not change this.Default:1000 |

In addition, many plugins can have sections in the configuration file, with names of the formrabbitmq\_plugin. Our maintained plugins are documented in the following locations:

* [rabbitmq\_management](https://www.rabbitmq.com/management.html#configuration)
* [rabbitmq\_management\_agent](https://www.rabbitmq.com/management.html#configuration)
* [rabbitmq\_web\_dispatch](https://www.rabbitmq.com/web-dispatch.html)
* [rabbitmq\_stomp](https://www.rabbitmq.com/stomp.html)
* [rabbitmq\_shovel](https://www.rabbitmq.com/shovel.html)
* [rabbitmq\_auth\_backend\_ldap](https://www.rabbitmq.com/ldap.html)

### Configuration entry encryption

Sensitive configuration entries \(e.g. password, URL containing credentials\) can be encrypted in the RabbitMQ configuration file. The broker decrypts encrypted entries on start.

Note that encrypted configuration entries don't make the system meaningfully more secure. Nevertheless, they allow deployments of RabbitMQ to conform to regulations in various countries requiring that no sensitive data should appear in plain text in configuration files.

Encrypted values must be inside an Erlangencryptedtuple:{encrypted, ...}. Here is an example of a configuration file with an encrypted password for the default user:

```
[
  {rabbit, [
      {default_user, <<"guest">>},
      {default_pass,
        {encrypted,
         <<"cPAymwqmMnbPXXRVqVzpxJdrS8mHEKuo2V+3vt1u/fymexD9oztQ2G/oJ4PAaSb2c5N/hRJ2aqP/X0VAfx8xOQ==">>
        }
      },
      {config_entry_decoder, [
             {passphrase, <<"mypassphrase">>}
         ]}
    ]}
].
```

Note the

config\_entry\_decoder

key with the passphrase that RabbitMQ will use to decrypt encrypted values.

The passphrase doesn't have to be hardcoded in the configuration file, it can be in a separate file:

```
[
  {rabbit, [
      ...
      {config_entry_decoder, [
             {passphrase, {file, "/path/to/passphrase/file"}}
         ]}
    ]}
].
```

RabbitMQ can also request an operator to enter the passphrase when it starts by using

{passphrase, prompt}

.

Userabbitmqctland theencodecommand to encrypt values:

```
rabbitmqctl encode '<<"guest">>' mypassphrase
{encrypted,<<"... long encrypted value...">>}
rabbitmqctl encode '"amqp://fred:secret@host1.domain/my_vhost"' mypassphrase
{encrypted,<<"... long encrypted value...">>}
```

Add the `decode` command if you want to decrypt values:

```
rabbitmqctl decode '{encrypted, <<"...">>}' mypassphrase
<<"guest">>
rabbitmqctl decode '{encrypted, <<"...">>}' mypassphrase
"amqp://fred:secret@host1.domain/my_vhost"
```

Values of different types can be encoded. The example above encodes both binaries \(&lt;&lt;"guest"&gt;&gt;\) and strings \("amqp://fred:secret@host1.domain/my\_vhost"\).

The encryption mechanism uses PBKDF2 to produce a derived key from the passphrase. The default hash function is SHA512 and the default number of iterations is 1000. The default cipher is AES 256 CBC.

You can change these defaults in the configuration file:

```
[
  {rabbit, [
      ...
      {config_entry_decoder, [
             {passphrase, "mypassphrase"},
             {cipher, blowfish_cfb64},
             {hash, sha256},
             {iterations, 10000}
         ]}
    ]}
].
```

On the command line:

```
rabbitmqctl encode --cipher blowfish_cfb64 --hash sha256 --iterations 10000 \
                     '<<"guest">>' mypassphrase
```



