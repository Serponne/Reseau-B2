#  TP2 : Network low-level, Switching
## I. Simplest setup
## II. More switches
### faire communiquer les trois PCs
```
PC1> ping 10.2.2.3   
84 bytes from 10.2.2.3 icmp_seq=1 ttl=64 time=0.778 ms
84 bytes from 10.2.2.3 icmp_seq=2 ttl=64 time=0.596 ms
84 bytes from 10.2.2.3 icmp_seq=3 ttl=64 time=0.516 ms
84 bytes from 10.2.2.3 icmp_seq=4 ttl=64 time=0.606 ms
84 bytes from 10.2.2.3 icmp_seq=5 ttl=64 time=0.739 ms
```
```
PC2> ping  10.2.2.1   
84 bytes from 10.2.2.1 icmp_seq=1 ttl=64 time=0.553 ms
84 bytes from 10.2.2.1 icmp_seq=2 ttl=64 time=1.387 ms
84 bytes from 10.2.2.1 icmp_seq=3 ttl=64 time=0.984 ms
84 bytes from 10.2.2.1 icmp_seq=4 ttl=64 time=0.685 ms
84 bytes from 10.2.2.1 icmp_seq=5 ttl=64 time=0.784 ms
```
```
PC3> ping 10.2.2.2   
84 bytes from 10.2.2.2 icmp_seq=1 ttl=64 time=2.683 ms
84 bytes from 10.2.2.2 icmp_seq=2 ttl=64 time=1.001 ms
84 bytes from 10.2.2.2 icmp_seq=3 ttl=64 time=1.202 ms
84 bytes from 10.2.2.2 icmp_seq=4 ttl=64 time=0.992 ms
84 bytes from 10.2.2.2 icmp_seq=5 ttl=64 time=1.481 ms
```
### Analyser la table MAC d'un switch
```
IOU1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/2
   1    0050.7966.6801    DYNAMIC     Et0/0
   1    0050.7966.6802    DYNAMIC     Et0/1
   1    aabb.cc00.0200    DYNAMIC     Et0/0
   1    aabb.cc00.0300    DYNAMIC     Et0/1
   1    aabb.cc00.0310    DYNAMIC     Et0/0
Total Mac Addresses for this criterion: 6
```
La première colonne indique a quel Vlan correspond chaque port la colonne Ports indique sur quel port est connecté la machine, la colonne Mac Address est l'adresse mac de la machine connecté au switch.
### Mise en évidence du Spanning Tree Protocol
#### Déterminer les informations STP
```
IOU1#show spanning-tree         

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0100
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    Shr 
Et0/1               Desg FWD 100       128.2    Shr 
Et0/2               Desg FWD 100       128.3    Shr 
Et0/3               Desg FWD 100       128.4    Shr 
Et1/0               Desg FWD 100       128.5    Shr 
Et1/1               Desg FWD 100       128.6    Shr 
Et1/2               Desg FWD 100       128.7    Shr 
Et1/3               Desg FWD 100       128.8    Shr 
```
La commande 'show spanning-tree' nous permet de voir différente information en rapport avec le protocole STP. On peut voir ici que le switch 1 est le root bridge car l'information 'This bridge is the root' est présente.

```
IOU3#show spanning-tree         

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    32769
             Address     aabb.cc00.0100
             Cost        100
             Port        1 (Ethernet0/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.0300
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Root FWD 100       128.1    Shr 
Et0/1               Altn BLK 100       128.2    Shr 

```
Ici on vois que le port 0/1 du switch 3 est bloqué
#### Confirmation des informations STP
(a faire)
### Reconfigurer STP
#### Changer la priorité d'un switch qui n'est pas le root bridge.
```
IOU2(config)#spanning-tree vlan 1 priority 4096
```
Grace a cette commande je change la priorité du switch 2 a 4096.
```
IOU2#show spanning-tree 

VLAN0001
  Spanning tree enabled protocol rstp
  Root ID    Priority    4097
             Address     aabb.cc00.0200
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    4097   (priority 4096 sys-id-ext 1)
             Address     aabb.cc00.0200
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec
```
Il devient alors le root bridge on le vois grace a la ligne 'This bridge is the root'.

## III. Isolation
### 1. Simple
#### Mettre en place la topologie et faire communiquer les PCs.
```
IOU1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.

IOU1(config)#vlan 10
IOU1(config-vlan)#name client-10
IOU1(config-vlan)#exit
IOU1(config)#interface Ethernet 0/0
IOU1(config-if)#switchport mode access 
IOU1(config-if)#switchport access vlan 10
```
Configuration du VLAN 10 sur le port 0/0.
```
IOU1(config)#vlan 20
IOU1(config-vlan)#name client-20
IOU1(config-vlan)#exit
IOU1(config)#interface Ethernet 0/1
IOU1(config-if)#switchport mode access 
IOU1(config-if)#switchport access vlan 20
IOU1(config-if)#exit
IOU1(config)#interface Ethernet 0/2
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 20
IOU1(config-if)#exit
IOU1(config)#exit
```
Configuration du VLAN 20 sur les ports 0/1 et 0/2.

```
PC1> ping 10.2.3.2
host (10.2.3.2) not reachable

PC1> ping 10.2.3.3
host (10.2.3.3) not reachable
```
On voit ici que PC1 ne peut pas contacter PC2 et PC3
```
PC2> ping 10.2.3.1
host (10.2.3.1) not reachable

PC2> ping 10.2.3.3
84 bytes from 10.2.3.3 icmp_seq=1 ttl=64 time=0.893 ms
84 bytes from 10.2.3.3 icmp_seq=2 ttl=64 time=1.085 ms
```
Tandis que PC2 peut contacter PC3 et inversement mais PC2 ne peut pas contacter PC1
### 2. Avec trunk
```
IOU1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU1(config)#vlan 10       
IOU1(config-vlan)#name client-10
IOU1(config-vlan)#exit
IOU1(config)#interface Ethernet 0/0
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 10
IOU1(config-if)#exit
IOU1(config)#vlan 20
IOU1(config-vlan)#name client-20
IOU1(config-vlan)#exit
IOU1(config)#interface Ethernet 0/1
IOU1(config-if)#switchport mode access
IOU1(config-if)#switchport access vlan 20
IOU1(config-if)#exit
```
Configuration du switch 1 pour le VLAN 10 sur le port 0/0 et pour le vlan 20 sur le port 0/1.
```
IOU2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
IOU2(config)#vlan 10
IOU2(config-vlan)#name client-10
IOU2(config-vlan)#exit
IOU2(config)#interface Ethernet 
IOU2(config)#interface Ethernet 0/1
IOU2(config-if)#switchport mode access
IOU2(config-if)#switchport access vlan 10
IOU2(config-if)#exit 
IOU2(config)#vlan 20 
IOU2(config-vlan)#name client-20
IOU2(config-vlan)#exit
IOU2(config)#interface Ethernet 0/0
IOU2(config-if)#switchport mode access 
IOU2(config-if)#switchport access vlan 20
IOU2(config-if)#exit
```
Configuration du switch 2 pour le VLAN 10 sur le port 0/1 et pour le vlan 20 sur le port 0/0.
```
IOU1#conf t    
Enter configuration commands, one per line.  End with CNTL/Z.
IOU1(config)#interface Ethernet 0/2
IOU1(config-if)#switchport mode trunk
IOU1(config-if)#exit                      
IOU1(config)#exit
```
Configuration du switch 1 pour le mode trunk sur le port 0/2 qui est relier au switch 2.
```
IOU2(config)#interface ethernet 0/2
IOU2(config-if)#switchport trunk encapsulation dot1q 
IOU2(config-if)#switchport mode trunk
IOU2(config-if)#exit
IOU2(config)#exit
```
Configuration du switch 2 pour le mode trunk sur le port 0/2 qui est relier au switch 1.
```
PC1> ping 10.2.10.2
84 bytes from 10.2.10.2 icmp_seq=1 ttl=64 time=0.835 ms
84 bytes from 10.2.10.2 icmp_seq=2 ttl=64 time=1.910 ms
^C
PC1> ping 10.2.20.1   
No gateway found

PC1> ping 10.2.20.2
No gateway found
```
On vois ici que PC1 ne peut contacter PC3 et réciproquement mais il ne peut pas contacter PC2 et PC4.
```
PC4> ping 10.2.20.1
84 bytes from 10.2.20.1 icmp_seq=1 ttl=64 time=0.836 ms
84 bytes from 10.2.20.1 icmp_seq=2 ttl=64 time=4.575 ms
^C
PC4> ping 10.2.10.1
No gateway found

PC4> ping 10.2.10.2
No gateway found
```
On vois ici que PC4 ne peut contacter PC2 et réciproquement mais il ne peut pas contacter PC1 et PC3.
## IV. Need for speed.
### Configuration de LACP entre le switch 1 et le switch 2.
```
IOU1#conf t
IOU1(config)#interface range ethernet0/2-3
IOU1(config-if-range)#channel-group 1 mode active 
Creating a port-channel interface Port-channel 1 
IOU1(config-if-range)#channel-protocol lacp
IOU1(config-if-range)#exit
IOU1(config)#exit
```
Configuration du LACP sur le switch 1.

```
IOU2#conf t
IOU2(config)#interface range ethernet0/2-3
IOU2(config-if-range)#channel-group 1 mode active 
Creating a port-channel interface Port-channel 1 
IOU2(config-if-range)#channel-protocol lacp
IOU2(config-if-range)#exit
IOU2(config)#exit
```
Configuration du LACP sur le switch 2.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0OTc2MDY3ODRdfQ==
-->