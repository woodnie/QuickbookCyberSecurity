## XSS {#xss}

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