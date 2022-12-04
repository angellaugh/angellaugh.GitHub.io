1. `hostnamectl set-hostname ES-suse`

2. `reboot`


3. change the network config to generate ip
3.1 `vi /etc/sysconfig/network/ifcfg-eth0`
3.2 add one line `ZONE=public`
```
"/etc/sysconfig/network/ifcfg-eth0" 3L, 46B                                                                                        3,11         全部
BOOTPROTO='dhcp'
STARTMODE='auto'
ZONE=public
~
```

4. `zypper update`
5. `zypper install net-tools-deprecated`
6. `zypper install cnf`
7. `cnf yum`
