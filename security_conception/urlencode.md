## URLencode {#urlencode}

ASCLL 字符 字符的含义 URL编码

# 用来标志特定的文档位置 %23

%    对特殊字符进行编码 %25

&amp;    分隔不同的变量值对 %26

+    在变量值中表示空格 %2B

/    表示目录路径  %2F

\    表示目录路径  %5C

=    用来连接键和值  %3D

?    表示查询字符串的开始 %3F

空格   空格   %20

.          句号   %2E

:          冒号   %3A

java对文字进行编码涉及3个函数：escape,encodeURI,encodeURIComponent，相应3个解码函数：unescape,decodeURI,decodeURIComponent

java中的编码方法：

escape() 方法：采用ISO Latin字符集对指定的字符串进行编码。所有的空格符、标点符号、特殊字符以及其他非ASCII字符都将被转化成%xx格式的字符编码（xx等于该字符在字符集表里面的编码的16进制数字）。比如，空格符对应的编码是%20。unescape方法与此相反。不会被此方法编码的字符： @ * / +

encodeURI()方法：把URI字符串采用UTF-8编码格式转化成escape格式的字符串。不会被此方法编码的字符：! @ # $&amp; * ( ) = : / ; ? + &amp;apos;

encodeURIComponent ()方法：把URI字符串采用UTF-8编码格式转化成escape格式的字符串。与encodeURI()相比，这个方法将对更多的字符进行编码，比如 / 等字符。所以如果字符串里面包含了URI的几个部分的话，不能用这个方法来进行编码，否则 / 字符被编码之后URL将显示错误。不会被此方法编码的字符：! * ( )