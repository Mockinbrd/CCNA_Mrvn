# TP 5 

## II. Lancement et configuration du lab

On configuera notre lab suivant ce tableau : 

<table>
<thead>
<tr>
<th>Machine</th>
<th><code>net1</code></th>
<th><code>net2</code></th>
<th><code>net12</code></th>
</tr>
</thead>
<tbody>
<tr>
<td><code>client1.tp5.b1</code></td>
<td>X</td>
<td><code>10.5.2.10</code></td>
<td>X</td>
</tr>
<tr>
<td><code>client2.tp5.b1</code></td>
<td>X</td>
<td><code>10.5.2.11</code></td>
<td>X</td>
</tr>
<tr>
<td><code>router1.tp5.b1</code></td>
<td><code>10.5.1.254</code></td>
<td>X</td>
<td><code>10.5.12.1</code></td>
</tr>
<tr>
<td><code>router2.tp5.b1</code></td>
<td>X</td>
<td><code>10.5.2.254</code></td>
<td><code>10.5.12.2</code></td>
</tr>
<tr>
<td><code>server1.tp5.b1</code></td>
<td><code>10.5.1.10</code></td>
<td>X</td>
<td>X</td>
</tr>
</tbody>
</table>

### Checklist IP VMs : 


:white_check_mark: Installation de certains paquets réseau

    

:white_check_mark: Désactivation de la carte NAT
 

:white_large_square: Définition des IPs statiques

:white_large_square: La connexion SSH doit être fonctionnelle


:white_large_square: Définition du nom de domaine

#### :white_check_mark: Définition des IPs statiques : 

Pour le serveur1.tp5.b1 : 
``` 
nano /etc/sysconfig/network-scripts/ifcfg-enp0s3

NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.5.1.10
NETMASK=255.255.255.0
```

client1.tp5.b1 : 
``` 
nano /etc/sysconfig/network-scripts/ifcfg-enp0s3

NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.5.2.10
NETMASK=255.255.255.0
```

client2.tp5.b1 : 
``` 
nano /etc/sysconfig/network-scripts/ifcfg-enp0s3

NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.5.2.11
NETMASK=255.255.255.0
```

#### :white_check_mark: Etablissement de la connexion SSH : 

On a réussi à ouvrir une fenêtre SSH pour chaque VM à l'aide de l'IP DHCP de la deuxième carte réseau de chacune ce celles-ci.

#### :white_check_mark: Définition du nom de domaine : 

serveur1.tp5 : 
``` 
echo 'serveur1.tp5' | sudo tee /etc/hostname
```

client1.tp5 : 
``` 
echo 'client1.tp5' | sudo tee /etc/hostname
```

client2.tp5 : 
``` 
echo 'client2.tp5' | sudo tee /etc/hostname
```

### Checklist IP Routeurs : 

:white_large_square: Définition des IPs statiques
:white_large_square: Définition du nom de domaine


#### :white_check_mark: Définition des IPs statiques des routeurs : 

routeur 1 : 
```
conf t
(config)# interface ethernet 0/0

(config-if)# ip address 10.5.1.254 255.255.255.0

exit

(config)# interface ethernet 0/1

(config-if)# ip address 10.5.12.1 255.255.255.252


```

```
router1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.5.2.254      YES NVRAM  up                    up
Ethernet0/1                10.5.12.1       YES manual up                    up

```

routeur 2 :

```
conf t
(config)# interface ethernet 0/0

(config-if)# ip address 10.5.2.254 255.255.255.0

exit

(config)# interface ethernet 0/1

(config-if)# ip address 10.5.12.2 255.255.255.252
```

```
router2#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.5.2.254      YES NVRAM  up                    up
Ethernet0/1                10.5.12.2       YES manual up                    up
```

#### :white_check_mark: Définition des noms de domaines:

router1.tp5 :
```
router1(config)#hostname router1.tp5
router1.tp5(config)#exit
router1.tp5#
```

router2.tp5 : 
```
router2(config)#hostname router2.tp5
router2.tp5(config)#exit
router2.tp5#
```

### Ajout des différentes routes : 

router1.tp5.b1 :

• Directement connecté à net1 et net12

:white_check_mark: Route à ajouter : net2

```
router1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
router1(config)#ip route 10.5.2.0 255.255.255.0 10.5.12.2
router1(config)#ip route 0.0.0.0 0.0.0.0 10.5.12.2
router1(config)#exit
router1#show ip
*Mar  1 00:19:42.475: %SYS-5-CONFIG_I: Configured from console by console
router1#show ip route

Gateway of last resort is 10.5.12.2 to network 0.0.0.0

     10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
C       10.5.12.0/30 is directly connected, Ethernet0/1
S       10.5.2.0/24 [1/0] via 10.5.12.2
C       10.5.1.0/24 is directly connected, Ethernet0/0
S*   0.0.0.0/0 [1/0] via 10.5.12.2
```

router2.tp5.b1 : 

• Directement connecté à net2 et net12

:white_check_mark: Route à ajouter : net1
```
router2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
router2(config)#ip route 10.5.1.0 255.255.255.0 10.5.12.1
router2(config)#ip route 0.0.0.0 0.0.0.0 10.5.12.1
router2(config)#exit
router2#s
*Mar  1 00:48:25.047: %SYS-5-CONFIG_I: Configured from console by console
router2#show ip route

Gateway of last resort is 10.5.12.1 to network 0.0.0.0

     10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
C       10.5.12.0/30 is directly connected, Ethernet0/1
C       10.5.2.0/24 is directly connected, Ethernet0/0
S       10.5.1.0/24 [1/0] via 10.5.12.1
S*   0.0.0.0/0 [1/0] via 10.5.12.1
router2#
```

server1.tp5.b1 : 

• Directement connecté à net1

:white_check_mark: Route à ajouter : net2

```
nano /etc/sysconfig/network-scripts/route-enp0s3

10.5.2.0/24 via 10.5.1.254 dev enp0s3
```

client1.tp5.b1 : 

• Directement connecté à net2

:white_check_mark: Route à ajouter : net1

```
nano /etc/sysconfig/network-scripts/route-enp0s3

10.5.1.0/24 via 10.5.2.254 dev enp0s3
```

client2.tp5.b1 : 

• Directement connecté à net2

:white_check_mark: Route à ajouter : net1

```
nano /etc/sysconfig/network-scripts/route-enp0s3

10.5.1.0/24 via 10.5.2.254 dev enp0s3
```

### Test des différentes routes : 

Ping serveur1.tp5 vers client1.tp5 : 

```
[root@serveur1 ~]# ping client1.tp5
PING client1.tp5 (10.5.2.10) 56(84) bytes of data.
64 bytes from client1.tp5 (10.5.2.10): icmp_seq=1 ttl=62 time=34.7 ms
64 bytes from client1.tp5 (10.5.2.10): icmp_seq=2 ttl=62 time=35.0 ms
```

Ping serveur1.tp5 vers client2.tp5 : 

```
[root@serveur1 ~]# ping client2.tp5
PING client2.tp5 (10.5.2.11) 56(84) bytes of data.
64 bytes from client2.tp5 (10.5.2.11): icmp_seq=2 ttl=62 time=35.7 ms
64 bytes from client2.tp5 (10.5.2.11): icmp_seq=3 ttl=62 time=38.9 ms
```

Ping client1.tp5 vers serveur1.tp5 : 

```
[root@client1 ~]# ping server1.tp5
PING server1.tp5 (10.5.1.10) 56(84) bytes of data.
64 bytes from server1.tp5 (10.5.1.10): icmp_seq=1 ttl=62 time=36.6 ms
64 bytes from server1.tp5 (10.5.1.10): icmp_seq=2 ttl=62 time=34.0 ms
```

Ping client2.tp5 vers serveur1.tp5 : 

```
[root@client2 ~]# ping server1.tp5
PING server1.tp5 (10.5.1.10) 56(84) bytes of data.
64 bytes from server1.tp5 (10.5.1.10): icmp_seq=1 ttl=62 time=32.1 ms
64 bytes from server1.tp5 (10.5.1.10): icmp_seq=2 ttl=62 time=37.6 ms
```

## III. DHCP

### 1. Mise en place du serveur DHCP

```
[root@client2 ~]# systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
   Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; disabled; vendor preset: disabled)
   Active: active (running) since mar. 2019-02-12 17:15:41 CET; 15s ago
     Docs: man:dhcpd(8)
           man:dhcpd.conf(5)
 Main PID: 3421 (dhcpd)
   Status: "Dispatching packets..."
   CGroup: /system.slice/dhcpd.service
           └─3421 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -gr...

févr. 12 17:15:41 client2.tp5 dhcpd[3421]: No subnet declaration for enp0s8....
févr. 12 17:15:41 client2.tp5 dhcpd[3421]: ** Ignoring requests on enp0s8. ...t
févr. 12 17:15:41 client2.tp5 dhcpd[3421]:    you want, please write a subn...n
févr. 12 17:15:41 client2.tp5 dhcpd[3421]:    in your dhcpd.conf file for t...t
févr. 12 17:15:41 client2.tp5 dhcpd[3421]:    to which interface enp0s8 is ...*
févr. 12 17:15:41 client2.tp5 dhcpd[3421]: nt
févr. 12 17:15:41 client2.tp5 dhcpd[3421]: Listening on LPF/enp0s3/08:00:27...4
févr. 12 17:15:41 client2.tp5 dhcpd[3421]: Sending on   LPF/enp0s3/08:00:27...4
févr. 12 17:15:41 client2.tp5 dhcpd[3421]: Sending on   Socket/fallback/fal...t
févr. 12 17:15:41 client2.tp5 systemd[1]: Started DHCPv4 Server Daemon.
Hint: Some lines were ellipsized, use -l to show in full.
[root@client2 ~]#

```

Utilisation de dhclient dans client1.tp5 : 

```
[root@client1 ~]# sudo dhclient -v -r
Internet Systems Consortium DHCP Client 4.2.5
Copyright 2004-2013 Internet Systems Consortium.
All rights reserved.
For info, please visit https://www.isc.org/software/dhcp/

Listening on LPF/enp0s8/08:00:27:33:e4:f2
Sending on   LPF/enp0s8/08:00:27:33:e4:f2
Listening on LPF/enp0s3/08:00:27:98:07:31
Sending on   LPF/enp0s3/08:00:27:98:07:31
Sending on   Socket/fallback
```

Demande d'une nouvelle IP : 

```
[root@client1 ~]# sudo dhclient -v
Internet Systems Consortium DHCP Client 4.2.5
Copyright 2004-2013 Internet Systems Consortium.
All rights reserved.
For info, please visit https://www.isc.org/software/dhcp/

Listening on LPF/enp0s8/08:00:27:33:e4:f2
Sending on   LPF/enp0s8/08:00:27:33:e4:f2
Listening on LPF/enp0s3/08:00:27:98:07:31
Sending on   LPF/enp0s3/08:00:27:98:07:31
Sending on   Socket/fallback
DHCPDISCOVER on enp0s8 to 255.255.255.255 port 67 interval 3 (xid=0x38792f9e)
DHCPDISCOVER on enp0s3 to 255.255.255.255 port 67 interval 3 (xid=0x250e8912)
DHCPREQUEST on enp0s8 to 255.255.255.255 port 67 (xid=0x38792f9e)
DHCPOFFER from 192.168.34.2
DHCPACK from 192.168.34.2 (xid=0x38792f9e)
bound to 192.168.34.4 -- renewal in 560 seconds.
```

### 2. Explorer un peu DHCP

• Faire une demande DHCP avec dhclient et capturer avec Wireshark l'échange du DORA : 

```
[root@client1 ~]# tcpdump -i enp0s3 -w dhcp.pk
```

Puis on ping le client 2 :
```
[root@client1 ~]# ping 10.5.2.11
```

Capture du WireShark : 
![WireShark DHCP](https://github.com/Mockinbrd/CCNA_Mrvn/blob/master/TP5/images/wireshark.jpg?raw=True)