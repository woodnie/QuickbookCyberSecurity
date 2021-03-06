### SYN Flood {#syn-flood}

TCP 三次握手

第三次握手中，如果服务器没有收到客户端的最终ACK确认报文，会一直处于SYN_RECV状态，将客户端IP加入等待列表，并重发第三步SNY+ACK报文。重发进行3-5次，大约间隔30秒左右轮询一次等待列表重试所有客户端。另一方面，服务器在自己发出了SYN+ACK报文后，会与分配资源为即将建立的TCP连接存储信息做准备，这个资源在等待重试期间一直保留。更为重要的是，服务器资源有限，可以维护的SYN_RECV状态超过极限后就不再接受新的SYN报文，也就是拒绝新的TCP连接建立。

攻击者伪装大量的IP地址给服务器发送SYN报文，由于伪造的IP地址几乎不可能存在，也就几乎不可能返回任何应答。因此服务器会维护一个庞大的等待队列，不停的重试发送SYN+ACK报文，同时占用着大量的资源无法释放，而且服务器将不能在接受新的SYN请求。合法用户无法完成3次握手！

工具： [http://www.icylife.net/yunshu/show.php?id=367](http://www.icylife.net/yunshu/show.php?id=367)