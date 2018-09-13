## Cookie {#cookie}

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。作者：余弦链接：https://www.zhihu.com/question/21304076/answer/29722319来源：知乎**1\. 攻破 XX**你上着 XX 聊天，XX 认证与消息都密文传输，在这样的 Wi-Fi 环境下，黑客怎么黑掉你呢？[本着负责任的态度，隐掉细节，且用 XX 代表某庞大业务]XX 是一个多么大的体系，这个体系下太多资源的传输是非 HTTPS 的，既然非 HTTPS，那基本就是明文，明文里的 Cookies 很有趣，Cookie 有这么几个关键字段：

name - Cookie 名；value - Cookie 值；domain - Cookie 设置在的域名，比如 .[http://xx.com](//link.zhihu.com/?target=http%3A//xx.com)，这个太关键了，但是很多人不懂为什么；path - Cookie 设置在的路径，比如 /xxx/，这个也比较关键，还是很多人不懂为什么；expires - Cookie 的过期时间，这个算关键；httponly - Cookie 的 HttpOnly 标志，如果有则表明无法通过 JavaScript 获取到，但这个在 Wi-Fi 环境下没什么意义，很多人无所谓这个……secure - Cookie 的 Secure 标志，如果有则表明 Cookie 需强制在 HTTPS 下传输，很多人无所谓这个……

我这样一列，很多人这样一看，发现太复杂了。是的，正因为复杂且重要，Cookies 安全才那么多人没做好。不是你安全意识不够，是黑客太厉害，且你用的业务太复杂……到这，黑客拿到这个传输的明文 Cookies 时，真的能黑掉 XX 吗？小黑客摇头叹气了，但是在大黑客眼里这是分分钟的事，由于 XX 业务复杂与庞大，导致 Cookies 这块的安全实际上想做完美是非常难的事（其他家的业务同理），拿到明文 Cookies 后，有几个关键的 Cookie 是可以攻破 XX 安全体系的。