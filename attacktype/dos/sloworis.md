### Sloworis {#sloworis}

慢速连接攻击

HTTP协议规定，HTTP Request以\r\n\r\n结尾表示客户端发送结束，服务端开始处理。

攻击者在HTTP请求中将Connection设置为keep-alive,要求web服务器保持TCP连接不要断开，随后缓慢的每个几分钟发送一个key-value格式的数据到服务端，如a:b\r\n,导致服务端认为HTTP头部没有接收完而一直等待。

工具：

ha.ckers.org/slowloris/slowloris.pl

变种：使用POST的方法向Web Server提交数据、填充一个大大Content-Length但缓慢的一个字节一个字节的POST真正数据内容。