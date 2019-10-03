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
On trouve dans le dossier `/NetworkManager` les deux baux :

    [root@localhost NetworkManager]# ls
    internal-0315ec43-c03d-3935-b5c9-9b9fcbb7e7de-enp0s8.lease  internal-b3cd966e-ca92-47c7-9c56-b4fc5ad6311b-enp0s3.lease  NetworkManager-intern.conf  NetworkManager.state  secret_key  timestamps

Pour la carte "NAT" on a : `internal-0315ec43-c03d-3935-b5c9-9b9fcbb7e7de-enp0s8.lease`
Pour la carte "Host Only" on a : `internal-b3cd966e-ca92-47c7-9c56-b4fc5ad6311b-enp0s3.lease`

Voici les information du bail de la carte "Host Only" :

     [root@localhost NetworkManager]# cat internal-0315ec43-c03d-3935-b5c9-9b9fcbb7e7de-enp0s8.lease
    # This is private data. Do not parse.
    ADDRESS=192.168.135.3
    NETMASK=255.255.255.0
    SERVER_ADDRESS=192.168.135.2
    T1=600
    T2=1050
    LIFETIME=1200
    CLIENTID=01080027353cd0

### Table de routage

    [root@localhost NetworkManager]# ip route list
    default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
    10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
    192.168.135.0/24 dev enp0s8 proto kernel scope link src 192.168.135.3 metric 101 

Cette route est vers le réseau enp0s8, elle est utilisée pour une connexion (locale|externe), la passerelle de cette route est à l'IP 10.0.2.0 et cette IP est portée par XXX
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NDA4NTc4MTcsLTgzMDYwODE3MCwtNT
A0NTIwNjEwLDE0MjczNzMyMDgsNTU1ODE3MTg1XX0=
-->