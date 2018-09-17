### SQL inject OverView {#sql-inject-overview}

[http://netsecurity.51cto.com/art/201108/287651.htm](http://netsecurity.51cto.com/art/201108/287651.htm)

SQL 注入：利用现有应用程序，将(恶意)的SQL命令注入到后台数据库引擎执行的能力，这是SQL注入的标准释义。

随着B/S模式被广泛的应用，用这种模式编写应用程序的程序员也越来越多，但由于开发人员的水平和经验参差不齐，相当一部分的开发人员在编写代码的时候，没有对用户的输入数据或者是页面中所携带的信息（如Cookie）进行必要的合法性判断，导致了攻击者可以提交一段数据库查询代码，根据程序返回的结果，获得一些他想得到的数据。

SQL注入利用的是正常的HTTP服务端口，表面上看来和正常的web访问没有区别，隐蔽性极强，不易被发现。

SQL注入过程

第一步：判断Web环境是否可以SQL注入。如果URL仅是对网页的访问，不存在SQL注入问题，如： [http://news.xxx.com.cn/162414739931.shtml](http://news.xxx.com.cn/162414739931.shtml) 就是普通的网页访问。只有对数据库进行动态查询的业务才可能存在SQL注入，如： [http://www.google.cn/webhp?id](http://www.google.cn/webhp?id) ＝39，其中?id＝39表示数据库查询变量，这种语句会在数据库中执行，因此可能会给数据库带来威胁。

第二步：寻找SQL注入点。完成上一步的片断后，就要寻找可利用的注入漏洞，通过输入一些特殊语句，可以根据浏览器返回信息，判断数据库类型，从而构建数据库查询语句找到注入点。

第三步：猜解用户名和密码。数据库中存放的表名、字段名都是有规律可言的。通过构建特殊数据库语句在数据库中依次查找表名、字段名、用户名和密码的长度，以及内容。这个猜测过程可以通过网上大量注入工具快速实现，并借助破解网站轻易破译用户密码。

第四步：寻找WEB管理后台入口。通常WEB后台管理的界面不面向普通用户

开放，要寻找到后台的登陆路径，可以利用扫描工具快速搜索到可能的登陆地址，依次进行尝试，就可以试出管理台的入口地址。

第五步：入侵和破坏。成功登陆后台管理后，接下来就可以任意进行破坏行为，如篡改网页、上传木马、修改、泄漏用户信息等，并进一步入侵数据库服务器。

SQL注入攻击的特点:

变种极多，有经验的攻击者会手动调整攻击参数，致使攻击数据的变种是不可枚举的，这导致传统的特征匹配检测方法仅能识别相当少的攻击，难以防范。

攻击过程简单，目前互联网上流行众多的SQL注入攻击工具，攻击者借助这些工具可很快对目标WEB系统实施攻击和破坏。

危害大，由于WEB编程语言自身的缺陷以及具有安全编程能力的开发人员少之又少，大多数WEB业务系统均具有被SQL注入攻击的可能。而攻击者一旦攻击成功，可以对控制整个WEB业务系统，对数据做任意的修改，破坏力达到及至。

[http://www.51testing.com/html/33/n-200933.html](http://www.51testing.com/html/33/n-200933.html) 如何防范SQL注入--编程篇

[http://www.51testing.com/html/40/n-201140.html](http://www.51testing.com/html/40/n-201140.html)  如何防范SQL注入--测试篇

[http://www.51testing.com/html/43/n-86043.html](http://www.51testing.com/html/43/n-86043.html) SQL注入攻击的种类和防范手段