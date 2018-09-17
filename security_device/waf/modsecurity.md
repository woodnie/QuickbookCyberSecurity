### ModSecurity {#modsecurity}

ModSecurity原本是Apache上的一款开源WAF模块，可以有效的增强Web安全性。目前已经支持Nginx和IIS，配合Nginx的灵活和高效可以打造成生产级的WAF，是保护和审核Web安全的利器。

在这篇文章中，我们将学习配置ModSecurity与OWASP的核心规则集。

ModSecurity是一个入侵侦测与防护引擎，它主要是用于Web应用程序，所以也被称为Web应用程序防火墙(WAF)。它可以作为Web服务器的模块或是单独的应用程序来运作。ModSecurity的功能是增强Web Application 的安全性和保护Web application以避免遭受来自已知与未知的攻击。

ModSecurity计划是从2002年开始，后来由Breach Security Inc.收购，但Breach Security Inc.允诺ModSecurity仍旧为Open Source，并开放源代码给大家使用。最新版的ModSecurity开始支持核心规则集(Core Rule Set)，CRS可用于定义旨在保护Web应用免受0day及其它安全攻击的规则。

ModSecurity还包含了其他一些特性，如并行文本匹配、Geo IP解析和信用卡号检测等，同时还支持内容注入、自动化的规则更新和脚本等内容。此外，它还提供了一个面向Lua语言的新的API，为开发者提供一个脚本平台以实现用于保护Web应用的复杂逻辑。

官网：https://www.modsecurity.org/