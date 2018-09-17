### SQL inject Principle {#sql-inject-principle}

[http://netsecurity.51cto.com/art/200808/87178.htm](http://netsecurity.51cto.com/art/200808/87178.htm)

一、注射式攻击的原理

注射式攻击的根源在于，程序命令和用户数据（即用户输入）之间没有做到泾渭分明。这使得攻击者有机会将程序命令当作用户输入的数据提交给We程序，以发号施令，为所欲为。

为了发动注射攻击，攻击者需要在常规输入中混入将被解释为命令的&quot;数据&quot;，要想成功，必须要做三件事情：

1.确定Web应用程序所使用的技术

注射式攻击对程序设计语言或者硬件关系密切，但是这些可以通过适当的踩点或者索性将所有常见的注射式攻击都搬出来逐个试一下就知道了。为了确定所采用的技术，攻击者可以考察Web页面的页脚，查看错误页面，检查页面源代码，或者使用诸如Nessus等工具来进行刺探。

2.确定所有可能的输入方式

Web应用的用户输入方式比较多，其中一些用户输入方式是很明显的，如HTML表单；另外，攻击者可以通过隐藏的HTML表单输入、HTTP头部、 cookies、甚至对用户不可见的后端AJAX请求来跟Web应用进行交互。一般来说，所有HTTP的GET和POST都应当作用户输入。为了找出一个 Web应用所有可能的用户输入，我们可以求助于Web代理，如Burp等。

3.查找可以用于注射的用户输入

在找出所有用户输入方式后，就要对这些输入方式进行筛选，找出其中可以注入命令的那些输入方式。这个任务好像有点难，但是这里有一个小窍门，那就是多多留意Web应用的错误页面，很多时候您能从这里得到意想不到的收获。

二、SQL注射原理

上面对注射攻击做了一般性的解释，下面我们以SQL注射为例进行讲解，以使读者对注射攻击有一个感性的认识，至于其他攻击，原理是一致的。

SQL注射能使攻击者绕过认证机制，完全控制远程服务器上的数据库。SQL是结构化查询语言的简称，它是访问数据库的事实标准。目前，大多数Web 应用都使用SQL数据库来存放应用程序的数据。几乎所有的Web应用在后台都使用某种SQL数据库。跟大多数语言一样，SQL语法允许数据库命令和用户数据混杂在一起的。如果开发人员不细心的话，用户数据就有可能被解释成命令，这样的话，远程用户就不仅能向Web应用输入数据，而且还可以在数据库上执行任意命令了。

三、绕过用户认证

我们这里以一个需要用户身份认证的简单的Web应用程序为例进行讲解。假定这个应用程序提供一个登录页面，要求用户输入用户名和口令。用户通过 HTTP请求发送他们的用户名和口令，之后，Web应用程序检查用户传递来用户名和口令跟数据库中的用户名和口令是否匹配。这种情况下，会要求在SQL数据库中使用一个数据库表。开发人员可以通过以下SQL语句来创建表：

CREATE TABLE user_table

(id             INTEGER           PRIMARYKEY,

username   VARCHAR(32),

password   VARCHAR(41)

);

上面的SQL代码将建立一个表，该表由三栏组成。

第一栏存放的是用户ID，如果某人经过认证，则用此标识该用户。

第二栏存放的是用户名，该用户名最多由32字符组成。

第三栏存放的是口令，它由用户的口令的hash值组成，因为以明文的形式来存放用户的口令实在太危险，所以通常取口令的散列值进行存放。

我们将使用SQL函数PASSWORD（）来获得口令的hash值，在MySQL中，函数PASSWORD（）的输出由41字符组成。

对一个用户进行认证，实际上就是将用户的输入即用户名和口令跟表中的各行进行比较，

如果跟某行中的用户名和口令跟用户的输入完全匹配，那么该用户就会通过认证，并得到该行中的ID。

假如用户提供的用户名和口令分别为lonelynerd15和mypassword，那么检查用户ID过程如下所示：

SELECT id FROM user_table WHERE username= &amp;apos;lonelynerd15&amp;apos; AND password= PASSWORD(&amp;apos;mypassword&amp;apos;)&lt; /FONT&gt;

如果该用户位于数据库的表中，这个SQL命令将返回该用户相应的ID，这就意味着该用户通过了认证；否则，这个SQL命令的返回为空，这意味着该用户没有通过认证。

下面是用来实现自动登录的Java代码，它从用户那里接收用户名和口令，然后通过一个SQL查询对用户进行认证：

Stringusername=req.getParameter(&quot;username&quot;);

Stringpassword=req.getParameter(&quot;password&quot;);

Stringquery= &quot;SELECTidFROMuser_table WHERE&quot;+&quot;username=&amp;apos;&quot;+username+&quot;&amp;apos;AND&quot;+&quot;password=PASSWORD(&amp;apos;&quot;+password+&quot;&amp;apos;)&quot;;

ResultSetrs=stmt.executeQuery(query);

intid=-1;//-1 implies that the user is unauthenticated.

while(rs.next()){

id=rs.getInt(&quot;id&quot;);

}

开头两行代码从HTTP请求中取得用户输入，然后在下一行开始构造一个SQL查询。执行查询，然后在while（）循环中得到结果，如果一个用户名和口令对匹配，就会返回正确的ID。否则，id的值仍然为-1，这意味着用户没有通过认证。表面上看，如果用户名和口令对匹配，那么该用户通过认证；否则，该用户不会通过认证--但是，事实果真如此吗？非也！读者也许已经注意到了，这里并没有对SQL命令进行设防，所以攻击者完全能够在用户名或者口令字段中注入 SQL语句，从而改变SQL查询。为此，我们仔细研究一下上面的SQL查询字符串：

SELECT idFROMuser_table WHERE&quot;+&quot;username=&amp;apos;&quot;+username+&quot;&amp;apos;AND&quot;+&quot;password=PASSWORD(&amp;apos;&quot;+password+&quot;&amp;apos;)；

上述代码认为字符串username和password都是数据，不过，攻击者却可以随心所欲地输入任何字符。如果一位攻击者输入的用户名为

&amp;apos;OR1=1-

而口令为

x

那么查询字符串将变成下面的样子：

SELECT idFROMuser_tableWHEREusername=&amp;apos;&amp;apos;OR1=1--&amp;apos;ANDpassword=PASSWORD(&amp;apos;x&amp;apos;)

该双划符号--告诉SQL解析器，右边的东西全部是注释，所以不必理会。这样，查询字符串相当于：

SELECT id FROM user_tableWHEREusername=&amp;apos;&amp;apos;OR1=1

如今的SELECT语句跟以前的已经大相径庭了，因为现在只要用户名为长度为零的字符串&amp;apos;&amp;apos;或1=1这两个条件中一个为真，就返回用户标识符ID--我们知道，1=1是恒为真的。所以这个语句将返回user_table中的所有ID。在此种情况下，攻击者在username字段放入的是SQL指令&amp;apos;OR1=1--而非数据。

四、构造SQL注射代码

为了成功地注入SQL命令，攻击者必须将开发人员的现有SQL命令转换成一个合法的SQL语句，当然，要盲注是有些难度的，但一般都是这样：

&amp;apos;OR1=1-

或者

&amp;apos;)OR1=1--

此外，许多Web应用提供了带来错误报告和调试信息，例如，利用&amp;apos;OR1=1--对Web应用进行盲注时，经常看到如下所示的错误信息：

Error executing query:

You have an error in your SQLsyntax;

check the manual that corresponds to your MySQL server version for the right syntax to use near

&amp;apos;SELECT(title,body) FROM blog_table WHEREcat=&amp;apos;OR1=1&amp;apos;atline1

该错误信息详细地为我们展示了完整的SQL语句，在此种情况下，SQL数据库所期待的好象是一个整数，而非字符串，所以可以注入字符串OR1=1--，把单引号去掉就应该能成功注入了。对于大多数SQL数据库，攻击者可以在一行中放入多个SQL语句，只要各个语句的语法没有错误就行。在下面的代码中，我们展示了如何将username设为&amp;apos;OR1=1并把password设为x来返回最后的用户ID：

Stringquery=&quot;SELECTidFROMuser_tableWHERE&quot;+&quot;username=&amp;apos;&quot;+username+&quot;&amp;apos;AND&quot;+&quot;password=PASSWORD(&amp;apos;&quot;+password+&quot;&amp;apos;)&quot;;

当然，攻击者可以注入其它的查询，例如，把username设为：

&amp;apos;OR1=1;DROP TABLE user_table;--

而这个查询将变成：

SELECT id FROM user_table WHERE username=&amp;apos;&amp;apos;OR1=1;DROP TABLE user_table;--&amp;apos;AND password=PASSWORD(&amp;apos;x&amp;apos;);

它相当于：

SELECT id FROM user_table WHERE username=&amp;apos;&amp;apos;OR1=1;DROP TABLE user_table;

这个语句将执行句法上完全正确的SELECT语句，并利用SQLDROP命令清空user_table。

注射式攻击不必非要进行盲式攻击，因为许多Web应用是利用开放源代码工具开发的，为了提高注射式攻击的成功率，我们可以下载免费的或者产品的试用版，然后在自己的系统上搭建测试系统。如果在测试系统上发现了错误，那么很可能同样的问题也会存在于所有使用该工具的Web应用身上。

五、小结

我们在本文中向读者介绍了注射攻击的根本原因，即没有对数据和命令进行严格区分。然后通过一些程序源码对SQL的攻击进行了细致的分析，使我们对 SQL注射机理有了一个深入的认识。如果您是一名web应用开发人员，那么您就当心了，一定不要盲目相信用户端的输入，而要对用户输入的数据进行严格的 &quot;消毒&quot;处理，否则的话，SQL注射将会不期而至。