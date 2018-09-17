## note {#note}

Web 应用是指采用B/S架构、通过HTTP/HTTPS协议提供服务的统称。随着互联网的广泛使用，Web应用已经融入到日常生活中的各个方面：网上购物、网络银行应用、证券股票交易、政府行政审批等等。在这些Web访问中，大多数应用不是静态的网页浏览，而是涉及到服务器侧的动态处理。此时，如果Java、PHP、ASP等程序语言的编程人员的安全意识不足，对程序参数输入等检查不严格等，会导致Web应用安全问题层出不穷。

本文根据当前Web应用的安全情况，列举了Web应用程序常见的攻击原理及危害，并给出如何避免遭受Web攻击的建议。    

1  Web应用漏洞原理

Web应用攻击是攻击者通过浏览器或攻击工具，在URL或者其它输入区域（如表单等），向Web服务器发送特殊请求，从中发现Web应用程序存在的漏洞，从而进一步操纵和控制网站，查看、修改未授权的信息。

1.1  Web应用的漏洞分类

1、信息泄露漏洞

信息泄露漏洞是由于Web服务器或应用程序没有正确处理一些特殊请求，泄露Web服务器的一些敏感信息，如用户名、密码、源代码、服务器信息、配置信息等。

造成信息泄露主要有以下三种原因：

    Web服务器配置存在问题，导致一些系统文件或者配置文件暴露在互联网中；

    Web服务器本身存在漏洞，在浏览器中输入一些特殊的字符，可以访问未授权的文件或者动态脚本文件源码；

    Web网站的程序编写存在问题，对用户提交请求没有进行适当的过滤，直接使用用户提交上来的数据。

2、目录遍历漏洞

目录遍历漏洞是攻击者向Web服务器发送请求，通过在URL中或在有特殊意义的目录中附加&quot;../&quot;、或者附加&quot;../&quot;的一些变形（如&quot;..\&quot;或&quot;..//&quot;甚至其编码），导致攻击者能够访问未授权的目录，以及在Web服务器的根目录以外执行命令。

3、命令执行漏洞

命令执行漏洞是通过URL发起请求，在Web服务器端执行未授权的命令，获取系统信息，篡改系统配置，控制整个系统，使系统瘫痪等。

命令执行漏洞主要有两种情况：

   通过目录遍历漏洞，访问系统文件夹，执行指定的系统命令；

   攻击者提交特殊的字符或者命令，Web程序没有进行检测或者绕过Web应用程序过滤，把用户提交的请求作为指令进行解析，导致执行任意命令。

4、文件包含漏洞

文件包含漏洞是由攻击者向Web服务器发送请求时，在URL添加非法参数，Web服务器端程序变量过滤不严，把非法的文件名作为参数处理。这些非法的文件名可以是服务器本地的某个文件，也可以是远端的某个恶意文件。由于这种漏洞是由PHP变量过滤不严导致的，所以只有基于PHP开发的Web应用程序才有可能存在文件包含漏洞。

5、SQL注入漏洞

SQL注入漏洞是由于Web应用程序没有对用户输入数据的合法性进行判断，攻击者通过Web页面的输入区域(如URL、表单等) ，用精心构造的SQL语句插入特殊字符和指令，通过和数据库交互获得私密信息或者篡改数据库信息。SQL注入攻击在Web攻击中非常流行，攻击者可以利用SQL注入漏洞获得管理员权限，在网页上加挂木马和各种恶意程序，盗取企业和用户敏感信息。

6、跨站脚本漏洞

跨站脚本漏洞是因为Web应用程序时没有对用户提交的语句和变量进行过滤或限制，攻击者通过Web页面的输入区域向数据库或HTML页面中提交恶意代码，当用户打开有恶意代码的链接或页面时，恶意代码通过浏览器自动执行，从而达到攻击的目的。跨站脚本漏洞危害很大，尤其是目前被广泛使用的网络银行，通过跨站脚本漏洞攻击者可以冒充受害者访问用户重要账户，盗窃企业重要信息。

根据前期各个漏洞研究机构的调查显示，SQL注入漏洞和跨站脚本漏洞的普遍程度排名前两位，造成的危害也更加巨大。

1.2   SQL注入攻击原理

SQL注入攻击是通过构造巧妙的SQL语句，同网页提交的内容结合起来进行注入攻击。比较常用的手段有使用注释符号、恒等式（如1＝1）、使用union语句进行联合查询、使用insert或update语句插入或修改数据等，此外还可以利用一些内置函数辅助攻击。

通过SQL注入漏洞攻击网站的步骤一般如下：

第一步：探测网站是否存在SQL注入漏洞。

第二步：探测后台数据库的类型。

第三步：根据后台数据库的类型，探测系统表的信息。

第四步：探测存在的表信息。

第五步：探测表中存在的列信息。

第六步：探测表中的数据信息。

1.3  跨站脚本攻击原理

跨站脚本攻击的目的是盗走客户端敏感信息,冒充受害者访问用户的重要账户。跨站脚本攻击主要有以下三种形式：

1、本地跨站脚本攻击

B给A发送一个恶意构造的Web URL，A点击查看了这个URL，并将该页面保存到本地硬盘（或B构造的网页中存在这样的功能）。A在本地运行该网页，网页中嵌入的恶意脚本可以A电脑上执行A持有的权限下的所有命令。

2、反射跨站脚本攻击

A经常浏览某个网站，此网站为B所拥有。A使用用户名/密码登录B网站，B网站存储下A的敏感信息（如银行帐户信息等）。C发现B的站点包含反射跨站脚本漏洞，编写一个利用漏洞的URL，域名为B网站，在URL后面嵌入了恶意脚本（如获取A的cookie文件），并通过邮件或社会工程学等方式欺骗A访问存在恶意的URL。当A使用C提供的URL访问B网站时，由于B网站存在反射跨站脚本漏洞，嵌入到URL中的恶意脚本通过Web服务器返回给A，并在A浏览器中执行，A的敏感信息在完全不知情的情况下将发送给了C。

3、持久跨站脚本攻击

B拥有一个Web站点，该站点允许用户发布和浏览已发布的信息。C注意到B的站点具有持久跨站脚本漏洞，C发布一个热点信息，吸引用户阅读。A一旦浏览该信息，其会话cookies或者其它信息将被C盗走。持久性跨站脚本攻击一般出现在论坛、留言簿等网页，攻击者通过留言，将攻击数据写入服务器数据库中，浏览该留言的用户的信息都会被泄漏。

2   Web应用漏洞的防御实现

对于以上常见的Web应用漏洞漏洞，可以从如下几个方面入手进行防御：

1)对 Web应用开发者而言

大部分Web应用常见漏洞，都是在Web应用开发中，开发者没有对用户输入的参数进行检测或者检测不严格造成的。所以，Web应用开发者应该树立很强的安全意识，开发中编写安全代码；对用户提交的URL、查询关键字、HTTP头、POST数据等进行严格的检测和限制，只接受一定长度范围内、采用适当格式及编码的字符，阻塞、过滤或者忽略其它的任何字符。通过编写安全的Web应用代码，可以消除绝大部分的Web应用安全问题。

2) 对Web网站管理员而言

作为负责网站日常维护管理工作Web管理员，应该及时跟踪并安装最新的、支撑Web网站运行的各种软件的安全补丁，确保攻击者无法通过软件漏洞对网站进行攻击。除了软件本身的漏洞外，Web服务器、数据库等不正确的配置也可能导致Web应用安全问题。Web网站管理员应该对网站各种软件配置进行仔细检测，降低安全问题的出现可能。此外，Web管理员还应该定期审计Web服务器日志，检测是否存在异常访问，及早发现潜在的安全问题。

3）使用网络防攻击设备

前两种为事前预防方式，是比较理想化的情况。然而在现实中，Web应用系统的漏洞还是不可避免的存在：部分Web网站已经存在大量的安全漏洞，而Web开发者和网站管理员并没有意识到或发现这些安全漏洞。由于Web应用是采用HTTP协议，普通的防火墙设备无法对Web类攻击进行防御，因此可以使用IPS入侵防御设备来实现安全防护。

++++++++++++++++++++++++++++++

Never trust data from the browser under any circumstances.

从不要相信来自浏览器端的数据。（因为你永远不可能知道在浏览器进行数据操作是你的用户还是正在寻找攻击漏洞的黑客）。

安全性问题包括的内容：

SQL injection is a common exploit in which an attacker alters Web page parameters (such as GET/POST data or URLs) to insert arbitrary SQL snippets that a na?ve Web application executes in its database directly.

SQL Injection（SQL注入）：SQL注入是最常见的攻击方式，它的主要原理是：攻击者通过改变WEB页的参数（如GET/POST数据或是URLS）直接将SQL片段提交到服务器，并在服务器端执行的过程。

Cross-Site scripting (XSS) is found in web applications that fail to escape user-submitted content properly before rendering it into HTML. This allows an attacker to insert arbitrary HTML into your Web page, usually in the form of &lt;script&gt; tags.

Attackers often use XSS attacks to steal cookie and session information, or to trick users into giving private information to the wrong person.

Cross-Site scripting (XSS)(跨站点脚本攻击)：XSS定义：是由于WEB程序没有对用户提交的HTML内容进行适当的转义，这样攻击者就可能在你的WEB页中插入一些HTML语句，这些语句通过以&lt;script&gt;标识符的形式出现。

攻击者通常使用XSS攻击来窃取cookie和session信息，或是欺骗用户将隐私信息暴露给错误对象（又称为钓鱼）。

Directory traversal is another injection-style attack, where in a malicious user tricks file system code reading and/or writing files that the Web server shouldn&amp;apos;t have access to.

Directory Traversal（目录遍历）：目录遍历是另一种注入类型的攻击，攻击者欺骗文件系统读或写服务器不允许操作的文件。

During development, being able to see trace backs and errors live in your browser is extremely useful. However, if these errors get displayed once the site goes live, they can reveal aspects of your code configuration that could aid an attacker.

Exposed Error Messages（暴露错误信息）：开发过程中，如果可以看到错误或历史纪录对FIX问题是非常有用的。但是如果这些错误信息被攻击者所获取，那么攻击者就可以通过错误信息而了解到应用程序代码或是数据库或是配置等方面的内容，并为其进行攻击提供有力的帮助。

漏洞扫描：

安全漏洞扫描通常是借助于特定的漏洞扫描器完成的。漏洞扫描器是一种自动检测远程或本地主机安全性弱点的程序。通过使用漏洞扫描器，系统管理员能够发现所维护信息系统存在的安全漏洞，及时修补漏洞。

按常规标准，可以将漏洞扫描分为两种类型：主机漏洞扫描（Host Scanner）和网络漏洞扫描器（Net Scanner）。

主机漏洞扫描器是指在本地运行检测系统漏洞的程序，如著名的COPS、Tripewire、Tiger等自由软件。

网络漏洞扫描器是指基于网络远程检测目标网络和主机系统漏洞的程序，如Satan、ISS、Internet Scanner等。

安全漏洞扫描可以用于日常安全防护，同时可以作为对软件产品或信息系统进行测试的手段，可以在安全漏洞造成严重危害前，发现漏洞并加以防范。

+++++

[http://www.51testing.com/html/16/n-172816.html](http://www.51testing.com/html/16/n-172816.html) Web安全渗透测试之信息搜集篇（上）

[http://www.51testing.com/html/21/n-172821.html](http://www.51testing.com/html/21/n-172821.html) Web安全渗透测试之信息搜集篇（下）

[http://www.51testing.com/html/16/n-235616.html](http://www.51testing.com/html/16/n-235616.html)  渗透测试技术与模型研究