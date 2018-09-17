## DenyHosts {#denyhosts}

DenyHosts 介绍

DenyHosts是Python语言写的一个程序，它会分析sshd的日志文件（默认是/var/log/secure），当发现重复的攻击时就会记录IP到/etc/hosts.deny文件，启用tcp_wrappers，从而达到自动屏IP的功能。 通过 [http://denyhosts.sourceforge.net](http://denyhosts.sourceforge.net) 可以下载DenyHosts的程序，可以直接下载rpm包来安装，也可以通过src.rpm包重新编译并安装等，通过这种方式默认是安装在/usr/share/denyhosts目录下。

DenyHosts的配置

1、启动DenyHosts 手动执行/usr/bin/denyhosts.py并指定其配置文件即可启动DenyHosts服务，如下：

# /usr/bin/denyhosts.py --daemon --config=/usr/share/denyhosts/denyhosts.cfg-dist

其中-daemon表示在后台运行，也可以直接执行自带/usr/share/denyhosts/daemon-control-dist脚本，因为daemon-control-dist脚本指定的配置文件名是&quot;denyhosts.cfg&quot;，所以在执行该脚本前需要将配置文件重命名。如下：

# mv denyhosts.cfg-dist denyhosts.cfg

# ./daemon-control-dist start

若要设置开机自动启动，则将该脚本做一个链接到/etc/init.d目录下，如下：

# ln -s /usr/share/denyhosts/daemon-control-dist /etc/init.d

# chkconfig daemon-control-dist on

关于denyhosts.cfg配置文件的常用参数解释如下：

SECURE_LOG = /var/log/secure

#ssh 日志文件，如果是redhat系列是根据/var/log/secure文件来判断的。

#Mandrake、FreeBSD是根据 /var/log/auth.log来判断的，而SUSE则是用/var/log/messages来判断的。这些在配置文件里面都有很详细的解释。

HOSTS_DENY = /etc/hosts.deny

#控制用户登陆的文件

PURGE_DENY = 30m

#过多久后清除已经禁止的，

#            &amp;apos;m&amp;apos; = minutes

#            &amp;apos;h&amp;apos; = hours

#            &amp;apos;d&amp;apos; = days

#            &amp;apos;w&amp;apos; = weeks

#            &amp;apos;y&amp;apos; = years

BLOCK_SERVICE = sshd

#禁止的服务名，当然DenyHost不仅仅用于SSH服务，还可用于SMTP等等。

DENY_THRESHOLD_INVALID = 1

#允许无效用户失败的次数

DENY_THRESHOLD_VALID = 5

#允许普通用户登陆失败的次数

DENY_THRESHOLD_ROOT = 3

#允许root登陆失败的次数

HOSTNAME_LOOKUP=NO

#是否做域名反解

ADMIN_EMAIL = iakuf@163.com

#管理员邮件地址,它会给管理员发邮件

DAEMON_LOG = /var/log/denyhosts

#DenyHosts日志文件存放的路径

配置完成后，再重新启动DenyHosts服务，然后在一个客户端尝试输入密码错误，看看/etc/hosts.deny文件里面是不是有类似如下的记录：

# DenyHosts: Sat Oct 13 20:47:14 2007 | sshd: 172.16.78.110

sshd: 172.16.78.110

如果有表明DenyHosts启用成功。