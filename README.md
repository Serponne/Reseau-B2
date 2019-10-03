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

### Table de routage 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNTgwNjY2MTUsLTUwNDUyMDYxMCwxND
I3MzczMjA4LDU1NTgxNzE4NV19
-->