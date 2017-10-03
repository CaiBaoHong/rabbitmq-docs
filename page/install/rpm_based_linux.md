# Installing on RPM-based Linux \(RHEL, CentOS, Fedora, openSUSE\)

## 1. Download the Server {#install-1}

|  | Description | Download |
| :--- | :--- | :--- |
| RPM for RHEL 7.x, CentOS 7.x, Fedora 19+ \(supports systemd, from[Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)\) | [rabbitmq-server-3.6.12-1.el7.noarch.rpm](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el7.noarch.rpm) | [\(Signature\)](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el7.noarch.rpm.asc) |
| RPM for RHEL Linux 6.x, CentOS 6.x, Fedora prior to 19 \(from[Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)\) | [rabbitmq-server-3.6.12-1.el6.noarch.rpm](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el6.noarch.rpm) | [\(Signature\)](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el6.noarch.rpm.asc) |
| RPM for RHEL Linux 7.x, CentOS 7.x, Fedora 19+ \(supports systemd, from[GitHub](https://github.com/rabbitmq/rabbitmq-server/releases)\) | [rabbitmq-server-3.6.12-1.el7.noarch.rpm](https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_12/rabbitmq-server-3.6.12-1.el7.noarch.rpm) | [\(Signature\)](https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_12/rabbitmq-server-3.6.12-1.el7.noarch.rpm.asc) |
| RPM for RHEL Linux 6.x, CentOS 6.x, Fedora prior to 19 \(from[GitHub](https://github.com/rabbitmq/rabbitmq-server/releases)\) | [rabbitmq-server-3.6.12-1.el6.noarch.rpm](https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_12/rabbitmq-server-3.6.12-1.el6.noarch.rpm) | [\(Signature\)](https://github.com/rabbitmq/rabbitmq-server/releases/download/rabbitmq_v3_6_12/rabbitmq-server-3.6.12-1.el6.noarch.rpm.asc) |
| RPM for openSUSE Linux \(from[Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)\) | [rabbitmq-server-3.6.12-1.suse.noarch.rpm](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.suse.noarch.rpm) | [\(Signature\)](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.suse.noarch.rpm.asc) |
| RPM for SLES 11.x \(from[Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm)\) | [rabbitmq-server-3.6.12-1.sles11.noarch.rpm](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.sles11.noarch.rpm) | [\(Signature\)](https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.sles11.noarch.rpm.asc) |

## 2. Overview {#install-2}

rabbitmq-serveris included in Fedora. However, the versions included are often quite old. You will probably get better results installing the .rpm from[PackageCloud](https://packagecloud.io/rabbitmq/rabbitmq-server/)or[Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm). Check the[Fedora package](https://admin.fedoraproject.org/updates/rabbitmq-server)details for which version of the server is available for which versions of the distribution.

The package is distributed via Yum repositories on[PackageCloud](https://packagecloud.io/rabbitmq/rabbitmq-server/)and[Bintray](https://bintray.com/rabbitmq/rabbitmq-server-rpm).

## 3. Supported Distributions {#install-3}

Below is a list of supported RPM-based distributions as of RabbitMQ 3.6.3:

* CentOS 6.x and 7.x \(note: there are two separate RPM packages for 6.x and 7.x\)
* RedHat Enterprise Linux 6.x and 7.x \(same as for CentOS, there are two separate packages\)
* Fedora 23 through 25 \(use the RHEL 7.x package\)

The packages may work on other RPM-based distributions if dependencies \(see below\) are satisfied \(e.g. using the Wheezy backports repository\) but their testing and support is done on a best effort basis.

## 4. Install Erlang {#install-4}

Before installing RabbitMQ, you must install a[supported version](https://www.rabbitmq.com/which-erlang.html)of Erlang/OTP. We strongly recommend using a packaged version. There are three suggested sources for Erlang packages:

* We produce
  [a package](https://github.com/rabbitmq/erlang-rpm)
  stripped down to only provide those components needed to run RabbitMQ.
  **It may not be as up to date as Erlang Solutions' packages**
  , but it might be easiest to use if installing Erlang's dependencies is proving difficult.
* [Erlang Solutions](https://www.erlang-solutions.com/resources/download.html)
  produces packages that are usually up to date. They produce two sets of packages - ones which are split up and are more convenient to use if you can add a yum repository, and a monolithic package which might be easier if you have to download manually.
* [EPEL](http://fedoraproject.org/wiki/EPEL)
  \("Extra Packages for Enterprise Linux"\); part of the Red Hat / Fedora organisation, provides many additional packages, including Erlang. These are the most official packages, and are split into many small packages, but are not always up to date.

### 4.1 Install zero-dependency Erlang from RabbitMQ {#install-4-1}

1. Download and install the
   [zero dependency Erlang RPM package for running RabbitMQ](https://github.com/rabbitmq/erlang-rpm)
   . As the name suggests, the package strips off some Erlang modules and dependencies that are not essential for running RabbitMQ.

### 4.2 Install Erlang from the EPEL repositoryor {#install-4-2}

1. Follow the steps in the
   [EPEL FAQ](http://fedoraproject.org/wiki/EPEL/FAQ#howtouse)
   to enable EPEL on your machine.
2. Issue the following command as 'root':
   ```
   yum install erlang
   ```

### 4.3 Install Erlang from the Erlang Solutions repositoryor {#install-4-3}

1. Follow the instructions under "Installation using repository" at
   [Erlang Solutions](https://www.erlang-solutions.com/resources/download.html)
   . Note that Erlang Solutions tend to provide cutting edge Erlang versions that may or may not be
   [supported by RabbitMQ](https://www.rabbitmq.com/which-erlang.html)
   . Version locking \(see below\) is recommended when Erlang installed using this option.

### 4.4 Install monolithic Erlang from Erlang Solutionsor {#install-4-4}

1. Download and install the
   [appropriate](https://www.rabbitmq.com/which-erlang.html)
   esl-erlang
   RPM from
   [Erlang Solutions](https://www.erlang-solutions.com/resources/download.html)
   .
2. Download and install the
   [esl-erlang-compat](https://github.com/jasonmcintosh/esl-erlang-compat)
   RPM \(
   [direct download](https://github.com/jasonmcintosh/esl-erlang-compat/releases/download/1.1.1/esl-erlang-compat-18.1-1.noarch.rpm)
   \) produced by
   [Jason McIntosh](https://github.com/jasonmcintosh/)
   .
   This is needed since Erlang Solutions' monolithic packages provide "esl-erlang"; this package just requires "esl-erlang" and provides "erlang".

### 4.5 Package Version Locking in Yum {#install-4-5}

[yum version locking](https://access.redhat.com/solutions/98873)plugin is recommended to prevent unwanted Erlang upgrades. This is highly recommended when Erlang is installed via the Erlang Solutions repository.

## 5. Install RabbitMQ Server {#install-5}

### 5.1 With rpm and Downloaded RPM {#install-5-1}

After downloading the server package, issue the following command as 'root':

```
rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
        yum install rabbitmq-server-3.6.12-1.noarch.rpm
```

Our public signing key is also[available from Bintray](https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc):

```
rpm --import https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
        yum install rabbitmq-server-3.6.12-1.noarch.rpm
```

### 5.2 Using PackageCloud RPM Repository {#install-5-2}

PackageCloud installs packages via HTTPS and signs them using their GPG key. There are multiple ways to install:

* Provided installation scripts
* Using PackageCloud Chef cookbook
* Using PackageCloud Puppet module
* Manually

See

[PackageCloud RabbitMQ repository instructions](https://packagecloud.io/rabbitmq/rabbitmq-server/install)

.

## 6. Run RabbitMQ Server {#install-6}

### 6.1 Customise RabbitMQ Environment Variables {#install-6-1}

The server should start using defaults. You can[customise the RabbitMQ environment](https://www.rabbitmq.com/configure.html#customise-general-unix-environment). Also see how to[configure components](https://www.rabbitmq.com/configure.html#configuration-file).

### 6.2 Start the Server {#install-6-2}

The server is not started as a daemon by default when the RabbitMQ server package is installed. To start the daemon by default when the system boots, as an administrator runchkconfig rabbitmq-server on.

As an administrator, start and stop the server as usual using/sbin/service rabbitmq-server stop/start/etc.

\_Note:\_The server is set up to run as system userrabbitmq. If you change the location of the Mnesia database or the logs, you must ensure the files are owned by this user \(and also update the environment variables\).

## 7. Port Access {#install-7}

SELinux, and similar mechanisms may prevent RabbitMQ from binding to a port. When that happens, RabbitMQ will fail to start. Firewalls can prevent nodes and CLI tools from communicating with each other. Make sure the following ports can be opened:

* 4369:
  [epmd](http://erlang.org/doc/man/epmd.html)
  , a peer discovery service used by RabbitMQ nodes and CLI tools
* 5672, 5671: used by AMQP 0-9-1 and 1.0 clients without and with TLS
* 25672: used by Erlang distribution for inter-node and CLI tools communication and is allocated from a dynamic range \(limited to a single port by default, computed as AMQP port + 20000\). See
  [networking guide](https://www.rabbitmq.com/networking.html)
  for details.
* 15672:
  [HTTP API](https://www.rabbitmq.com/management.html)
  clients and
  [rabbitmqadmin](https://www.rabbitmq.com/management-cli.html)
  \(only if the
  [management plugin](https://www.rabbitmq.com/management.html)
  is enabled\)
* 61613, 61614:
  [STOMP clients](https://stomp.github.io/stomp-specification-1.2.html)
  without and with TLS \(only if the
  [STOMP plugin](https://www.rabbitmq.com/stomp.html)
  is enabled\)
* 1883, 8883: \(
  [MQTT clients](http://mqtt.org/)
  without and with TLS, if the
  [MQTT plugin](https://www.rabbitmq.com/mqtt.html)
  is enabled
* 15674: STOMP-over-WebSockets clients \(only if the
  [Web STOMP plugin](https://www.rabbitmq.com/web-stomp.html)
  is enabled\)
* 15675: MQTT-over-WebSockets clients \(only if the
  [Web MQTT plugin](https://www.rabbitmq.com/web-mqtt.html)
  is enabled\)

It is possible to

[configure RabbitMQ](https://www.rabbitmq.com/configure.html)

to use

[different ports and specific network interfaces](https://www.rabbitmq.com/networking.html)

.

## 8. Default user access {#install-8}

The broker creates a userguestwith passwordguest. Unconfigured clients will in general use these credentials.**By default, these credentials can only be used when connecting to the broker as localhost**so you will need to take action before connecting from any other machine.

See the documentation on[access control](https://www.rabbitmq.com/access-control.html)for information on how to create more users, delete theguestuser, or allow remote access to theguestuser.

## 9. Controlling System Limits on Linux {#install-9}

RabbitMQ installations running production workloads may need system limits and kernel parameters tuning in order to handle a decent number of concurrent connections and queues. The main setting that needs adjustment is the max number of open files, also known asulimit -n. The default value on many operating systems is too low for a messaging broker \(eg. 1024 on several Linux distributions\). We recommend allowing for at least 65536 file descriptors for userrabbitmqin production environments. 4096 should be sufficient for most development workloads.

There are two limits in play: the maximum number of open files the OS kernel allows \(fs.file-max\) and the per-user limit \(ulimit -n\). The former must be higher than the latter.

### 9.1 With systemd \(Recent Linux Distributions\) {#install-9-1}

On distributions that use systemd, the OS limits are controlled via a configuration file at/etc/systemd/system/rabbitmq-server.service.d/limits.conf, for example:

```
[Service]
LimitNOFILE
=
300000
```

### 9.2 Without systemd \(Older Linux Distributions\) {#install-9-2}

The most straightforward way to adjust the per-user limit for RabbitMQ on distributions that do not use systemd is to edit the[rabbitmq-env.conf](http://www.rabbitmq.com/configure.html)to invokeulimitbefore the service is started.

```
ulimit
 -S -n 
4096
```

This\_soft\_limit cannot go higher than the\_hard\_limit \(which defaults to 4096 in many distributions\).[The hard limit can be increased](http://docs.basho.com/riak/latest/ops/tuning/open-files-limit/#Linux)via/etc/security/limits.conf. This also requires enabling the[pam\_limits.so](http://askubuntu.com/a/34559)module and re-login or reboot.

Note that limits cannot be changed for running OS processes.

For more information about controllingfs.file-maxwithsysctl, please refer to the excellent[Riak guide on open file limit tuning](http://docs.basho.com/riak/latest/ops/tuning/open-files-limit/#Linux).

### 9.3 Verifying the Limit {#install-9-3}

[RabbitMQ management UI](https://www.rabbitmq.com/management.html)displays the number of file descriptors available for it to use on the Overview tab.

```
rabbitmqctl
 status
```

includes the same value.

The following command

```
cat
 /proc/
$RABBITMQ_BEAM_PROCESS_PID
/limits
```

can be used to display effective limits of a running process.

$RABBITMQ\_BEAM\_PROCESS\_PID

is the OS PID of the Erlang VM running RabbitMQ, as returned by

rabbitmqctl status

.

### 9.4 Configuration Management Tools {#install-9-4}

Configuration management tools \(e.g. Chef, Puppet, BOSH\) provide assistance with system limit tuning. Our[developer tools](https://www.rabbitmq.com/devtools.html#devops-tools)guide lists relevant modules and projects.

## 10. Managing the Broker {#install-10}

To stop the server or check its status, etc., you can use package-specific scripts \(e.g. theservicetool\) or invokerabbitmqctl\(as an administrator\). It should be available on the path. Allrabbitmqctlcommands will report the node absence if no broker is running.

* Invoke
  rabbitmqctl stop
  to stop the server.
* Invoke
  rabbitmqctl status
  to check whether it is running.

More[info on rabbitmqctl](https://www.rabbitmq.com/man/rabbitmqctl.1.man.html).

### 10.1 Logging {#install-10-1}

Output from the server is sent to a**RABBITMQ\_NODENAME**.log file in the**RABBITMQ\_LOG\_BASE**directory. Additional log data is written to**RABBITMQ\_NODENAME**-sasl.log.

The broker always appends to the log files, so a complete log history is retained.

You can use thelogrotateprogram to do all necessary rotation and compression, and you can change it. By default, this script runs weekly on files located in default/var/log/rabbitmqdirectory. See/etc/logrotate.d/rabbitmq-serverto configurelogrotate.

