hostnamectl set-hostname ES-suse
reboot

vi /etc/sysconfig/network/ifcfg-eth0
add one line `ZONE=public`
```
"/etc/sysconfig/network/ifcfg-eth0" 3L, 46B                                                                                        3,11         全部
BOOTPROTO='dhcp'
STARTMODE='auto'
ZONE=public
~
```

zypper update
zypper install net-tools-deprecated
zypper install cnf
cnf yum
