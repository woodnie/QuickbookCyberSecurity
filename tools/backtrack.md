## Backtrack {#backtrack}

/etc/network/interfaces

[http://www.backtrack-linux.org/](http://www.backtrack-linux.org/)

/lib/modules/2.4.20-8/kernel/drivers/net

2)cp 3c59x.o /lib/modules/2.4.20-8/kernel/drivers/net

3)modprobe 3c59x

4)vi /etc/modprobe.conf

alias eth0 3c59x

5 ）vi /etc/sysconfig/network-script/ifcfg-eth0

DEVICE=eth0

TYPE=Ethernet

HWADDR=00:0C:29:F0:78:4B

BOOTPROTO=dhcp

ONBOOT=yes

[http://book.51cto.com/art/201202/317838.htm](http://book.51cto.com/art/201202/317838.htm)