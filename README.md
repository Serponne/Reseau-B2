# TP1 - Back To Basics 

## I. Gather informations

### Liste cartes réseau: 

    [loki@localhost ~]$ ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:a7:54:f0 brd ff:ff:ff:ff:ff:ff
        inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
           valid_lft 84622sec preferred_lft 84622sec
        inet6 fe80::d212:7240:e789:9378/64 scope link noprefixroute
           valid_lft forever preferred_lft forever
    3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 08:00:27:35:3c:d0 brd ff:ff:ff:ff:ff:ff
        inet 192.168.135.56/24 brd 192.168.135.255 scope global dynamic noprefixroute enp0s8
           valid_lft forever preferred_lft forever
        inet6 fe80::90d3:2a4d:6c75:ffea/64 scope link noprefixroute
           valid_lft forever preferred_lft forever
### IP en DHCP

Avec `ip a` on peut remarquer que les cartes sont en "DHCP" avec la présence de `dynamic` à côté de l'IP de  chaque cartes
****

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI5MzU0ODM1OCwtNTA0NTIwNjEwLDE0Mj
czNzMyMDgsNTU1ODE3MTg1XX0=
-->