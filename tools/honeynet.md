## Honeynet {#honeynet}

www.honeynet.org

www.honeynet.org.tw

[http://netsecurity.51cto.com/art/201106/271995.htm](http://netsecurity.51cto.com/art/201106/271995.htm)

1.Kippo   工具实现

[http://code.google.com/p/kippo/](http://code.google.com/p/kippo/)

2.脚本实现

2.1当黑客以root身份登录时：

一般情况下，只要用户输入的用户名和口令正确，就能顺利进入系统。如果我们在进入系统时设置了陷阱，并使黑客对此防不胜防，就会大大提高入侵的难度系数。例如，当黑客已获取正确的root口令，并以root身份登录时，我们在此设置一个迷魂阵，提示它，你输入的口令错误，并让它重输用户名和口令。而其实，这些提示都是虚假的，只要在某处输入一个密码就可通过。黑客因此就掉入这个陷阱，不断地输入root用户名和口令，却不断地得到口令错误的提示，从而使它怀疑所获口令的正确性，放弃入侵的企图。

给超级用户也就是root用户设置陷阱，并不会给系统带来太多的麻烦，因为，拥有root口令的人数不会太多，为了系统的安全，稍微增加一点复杂性也是值得的。这种陷阱的设置时很方便的，我们只要在root用户的.profile中加一段程序就可以了。我们完全可以在这段程序中触发其他入侵检测与预警控制程序。陷阱程序如下：

#you need type correct key(not you password) to login

#disable ctrl+c key:

stty intr undef

clear

echo &quot;You had input an error password , please input again !&quot;

echo    

echo  -n &quot;Login:&quot;

read p

if [ &quot;$p&quot; = &quot;123456&quot; ]

then

       clear

else

       sh sh-logout

fi

#enable ctrl+c key:

stty intr ^c

2.2当黑客用su命令转换成root身份时：

在很多情况下，黑客会通过su命令转换成root身份，因此，必须在此设置陷阱。当黑客使用su命令，并输入正确的root口令时，也应该报错，以此来迷惑它，使它误认为口令错误，从而放弃入侵企图。这种陷阱的设置也很简单，你可以在系统的/etc/profile文件中设置一个alias，把su命令重新定义成转到普通用户的情况就可以了，例如alias su=&quot;su user1&quot;。这样，当使用su时，系统判断的是user1的口令，而不是root的口令，当然不能匹配。即使输入su root也是错误的，也就是说，从此屏蔽了转向root用户的可能性。

alias alias=&amp;apos;alias 1&gt;/dev/null&amp;apos;

alias su= &amp;apos;su nobody&amp;apos;

2.3当黑客以root身份成功登录后一段时间内：

如果前两种设置都失效了，黑客已经成功登录，就必须启用登录成功的陷阱。一旦root用户登录，就可以启动一个计时器，正常的root登录就能停止计时，而非法入侵者因不知道何处有计时器，就无法停止计时，等到一个规定的时间到，就意味着有黑客入侵，需要触发必要的控制程序，如关机处理等，以免造成损害，等待系统管理员进行善后处理。陷阱程序如下：

times=0

while [ $times -le 5 ]

do

sleep 1

times=$[times + 1]

done

logout