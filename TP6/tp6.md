# TP 6 

## Sommaire 

<ul>
<li><a href="#lab-1--simple-ospf">Lab 1 : Simple OSPF</a></li>
<li><a href="#lab-2--un-peu-de-complexit%C3%A9-et-dutilit%C3%A9-">Lab 2 : Un peu de complexité (et d'utilité ?)</a>
<ul>
<li><a href="#1-pr%C3%A9sentation-du-lab-et-contexte">1. Présentation du lab et contexte</a>
<ul>
<li><a href="#sch%C3%A9ma-de-la-topologie">Schéma de la topologie</a></li>
<li><a href="#aires-ospf">Aires OSPF</a></li>
<li><a href="#r%C3%A9seaux-ip-et-aires-ospf">Réseaux IP et aires OSPF</a></li>
<li><a href="#adressage-ip-de-chacune-des-machines">Adressage IP des machines</a></li>
</ul>
</li>
<li><a href="#2-mise-en-place-du-lab">2. Mise en place du lab</a>
<ul>
<li><a href="#checklist-ip-routeurs">Adressage statique - routeurs</a></li>
<li><a href="#checklist-vms">Adressage statique - VMs</a></li>
<li><a href="#configuration-de-ospf">OSPF</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#lab-3--lets-end-this-properly">Lab 3 : Let's end this properly</a>
<ul>
<li><a href="#1-nat--acc%C3%A8s-internet">NAT (accès internet dans tout le réseau)</a></li>
<li><a href="#2-un-service-dinfra">Un service d'infra</a></li>
<li><a href="#3-serveur-dhcp">DHCP (adressage dynamique pour les clients)</a></li>
<li><a href="#4-serveur-dns">DNS</a></li>
<li><a href="#5-serveur-ntp">NTP (synchronisation de l'heure)</a></li>
</ul>
</li>
<li><a href="#bilan">Bilan</a></li>
<li><a href="#aller-plus-loin">Aller plus loin</a></li>
</ul>

## Lab 1 : Simple OSPF

Petit lab simple pour comprendre le concept.

## Lab 2 : Un peu de complexité (et d'utilité ?...)

<p align="center">
  <a target="_blank" rel="noopener noreferrer" href="https://github.com/It4lik/B1-Reseau-2018/blob/master/tp/6/pic/lab-2-topo.png"><img src="https://github.com/It4lik/B1-Reseau-2018/blob/master/tp/6/pic/lab-2-topo.png" title="Topologie du Lab 2" style="max-width:100%;"></a>
</p>


**Adressage IP de chacune des machines :**
<table>
<thead>
<tr>
<th>Machines</th>
<th><code>10.6.100.0/30</code></th>
<th><code>10.6.100.4/30</code></th>
<th><code>10.6.100.8/30</code></th>
<th><code>10.6.100.12/30</code></th>
<th><code>10.6.101.0/30</code></th>
<th><code>10.6.201.0/24</code></th>
<th><code>10.6.202.0/24</code></th>
</tr>
</thead>
<tbody>
<tr>
<td><code>r1.tp6.b1</code></td>
<td><code>10.6.100.1</code></td>
<td><code>10.6.100.5</code></td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td><code>10.6.202.254</code></td>
</tr>
<tr>
<td><code>r2.tp6.b1</code></td>
<td><code>10.6.100.2</code></td>
<td>-</td>
<td><code>10.6.100.9</code></td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
</tr>
<tr>
<td><code>r3.tp6.b1</code></td>
<td>-</td>
<td>-</td>
<td><code>10.6.100.10</code></td>
<td><code>10.6.100.14</code></td>
<td><code>10.6.101.1</code></td>
<td>-</td>
<td>-</td>
</tr>
<tr>
<td><code>r4.tp6.b1</code></td>
<td>-</td>
<td><code>10.6.100.6</code></td>
<td>-</td>
<td><code>10.6.100.13</code></td>
<td>-</td>
<td>-</td>
<td>-</td>
</tr>
<tr>
<td><code>r5.tp6.b1</code></td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td><code>10.6.101.2</code></td>
<td><code>10.6.201.254</code></td>
<td>-</td>
</tr>
<tr>
<td><code>client1.tp6.b1</code></td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td><code>10.6.201.10</code></td>
<td>-</td>
</tr>
<tr>
<td><code>client2.tp6.b1</code></td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td><code>10.6.201.11</code></td>
<td>-</td>
</tr>
<tr>
<td><code>server1.tp6.b1</code></td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td>-</td>
<td><code>10.6.202.10</code></td>
</tr>
</tbody>
</table>

### 2. Mise en place du lab

<p>On parle de <code>r1.tp6.b1</code>, <code>r2.tp6.b1</code>, <code>r3.tp6.b1</code>, <code>r4.tp6.b1</code> et <code>r5.tp6.b1</code> :</p>

 
:white_check_mark: Définition des IPs statiques

:white_check_mark: Définition du nom de domaine

### Checklist VMs

<p>On parle de <code>client1.tp6.b1</code>, <code>client2.tp6.b1</code> et <code>server1.tp6.b1</code> :</p>

:white_check_mark: Désactiver SELinux

:white_check_mark: Installation de certains paquets réseau

:white_check_mark: Enlever la carte NAT

:white_check_mark: Définition des IPs statiques

:white_check_mark:  Définition du nom de domaine

### Configuration de OSPF

<p>Ca se passe sur les routeurs uniquement. On parle de <code>r1.tp6.b1</code>, <code>r2.tp6.b1</code>, <code>r3.tp6.b1</code>, <code>r4.tp6.b1</code> et <code>r5.tp6.b1</code> :</p>

:white_check_mark: activation de OSPF

:white_check_mark: configurer le <code>router-id</code>

:white_check_mark: configurer les routes à partager

#### Verification : 

    Show ip route 
    
    r1.tp6.b1#
    10.0.0.0/8 is variably subnetted, 7 subnets, 2 masks
    O       10.6.100.8/30 [110/20] via 10.6.100.2, 00:07:50, Ethernet0/0
    O       10.6.100.12/30 [110/20] via 10.6.100.6, 00:07:50, Ethernet0/1
    C       10.6.100.0/30 is directly connected, Ethernet0/0
    O       10.6.101.0/30 [110/30] via 10.6.100.6, 00:07:50, Ethernet0/1
                          [110/30] via 10.6.100.2, 00:07:50, Ethernet0/0
    C       10.6.100.4/30 is directly connected, Ethernet0/1
    O       10.6.201.0/24 [110/40] via 10.6.100.6, 00:07:50, Ethernet0/1
                          [110/40] via 10.6.100.2, 00:07:51, Ethernet0/0
    C       10.6.202.0/24 is directly connected, Ethernet0/2

_

    show ip protocols
    
    r2.tp6.b1#show ip protocol
    Routing Protocol is "ospf 1"
      Outgoing update filter list for all interfaces is not set
      Incoming update filter list for all interfaces is not set
      Router ID 2.2.2.2
      Number of areas in this router is 1. 1 normal 0 stub 0 nssa
      Maximum path: 4
      Routing for Networks:
        10.6.100.0 0.0.0.3 area 0
        10.6.100.8 0.0.0.3 area 0
     Reference bandwidth unit is 100 mbps
      Routing Information Sources:
        Gateway         Distance      Last Update
        5.5.5.5              110      00:08:31
        3.3.3.3              110      00:08:31
        1.1.1.1              110      00:08:31
      Distance: (default is 110)

_

    show ip ospf neighbor
    
    r3.tp6.b1#show ip ospf neighbor

    Neighbor ID     Pri   State           Dead Time   Address         Interface
    5.5.5.5           1   FULL/BDR        00:00:32    10.6.101.2      Ethernet0/2
    4.4.4.4           1   FULL/BDR        00:00:35    10.6.100.14     Ethernet0/1
    2.2.2.2           1   FULL/BDR        00:00:32    10.6.100.9      Ethernet0/0


- Ping : 

ping r1.tp6.b1 vers r5.tp6.b1 : 

    r1.tp6.b1#ping 10.6.101.2
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 10.6.101.2, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 28/49/68 ms


client1.tp6.b1 vers server1.tp6.b1 : 

    [root@client1 ~]$ ping server1
    PING server1 (10.6.202.10) 56(84) bytes of data.
    64 bytes from server1 (10.6.202.10): icmp_seq=1 ttl=60 time=75.6 ms
    64 bytes from server1 (10.6.202.10): icmp_seq=2 ttl=60 time=76.9 ms
    
- Traceroute : 

    <code>[root@client1 ~]$ traceroute server1
    traceroute to server1 (10.6.202.10), 30 hops max, 60 byte packets
     1  gateway (10.6.201.254)  7.689 ms  7.568 ms  7.533 ms
     2  10.6.101.1 (10.6.101.1)  18.444 ms  18.792 ms  18.707 ms
     3  10.6.100.14 (10.6.100.14)  29.667 ms  29.993 ms  30.380 ms
     4  10.6.100.5 (10.6.100.5)  41.408 ms  41.526 ms  41.674 ms
     5  server1 (10.6.202.10)  64.223 ms !X  64.562 ms !X  64.557 ms !X</code>

    <code>[root@server1 ~]$ traceroute client1
    traceroute to client1 (10.6.201.10), 30 hops max, 60 byte packets
     1  gateway (10.6.202.254)  6.788 ms  6.669 ms  6.504 ms
     2  10.6.100.6 (10.6.100.6)  17.985 ms  18.223 ms  18.370 ms
     3  10.6.100.13 (10.6.100.13)  30.119 ms  30.063 ms  30.029 ms
     4  10.6.101.2 (10.6.101.2)  40.576 ms  40.493 ms  40.527 ms
     5  client1 (10.6.201.10)  51.504 ms !X  51.435 ms !X  51.347 ms !X</code>

    <code>[root@server1 ~]$ traceroute client2
    traceroute to client2 (10.6.201.11), 30 hops max, 60 byte packets
     1  gateway (10.6.202.254)  8.920 ms  8.600 ms  8.484 ms
     2  10.6.100.2 (10.6.100.2)  18.993 ms  19.841 ms  19.768 ms
     3  10.6.100.10 (10.6.100.10)  41.857 ms  41.759 ms  42.042 ms
     4  10.6.101.2 (10.6.101.2)  52.811 ms  52.799 ms  52.650 ms
     5  client2 (10.6.201.11)  64.941 ms !X  65.515 ms !X  65.320 ms !X</code>
     

## Lab 3 : Let's end this properly

### 1. NAT : accès internet

    r4.tp6.b1#telnet trip-hop.net 80
    Trying trip-hop.net (213.186.33.4, 80)... Open
    GET/
    HTTP/1.1 400 Bad Request
    Server: squid
    Mime-Version: 1.0
    Date: Tue, 12 Mar 2019 10:31:22 GMT
    Content-Type: text/html;charset=utf-8
    Content-Length: 4104
    X-Squid-Error: ERR_INVALID_REQ 0
    Vary: Accept-Language
    Content-Language: fr
    X-Cache: MISS from PF1-BOR1FR
    X-Cache-Lookup: NONE from PF1-BOR1FR:3128
    Connection: close


    <html><head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>ERREUR : l'URL demandée n'a pas pu être chargée</title>
        ETC...

### 2. Un service d'infra

Curl sur Client1 depuis Serveur1:

    [root@client1 ~]# curl serveur1tp6.b1
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

    <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
        <head>
            <title>Test Page for the Nginx HTTP Server on Fedora</title>
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
            
            ETC....

### 3. Serveur DHCP 


Mettre en route le DHCP:

    [root@client2 ~]# sudo systemctl start dhcpd

Configurez en DHCP client1.tp6.b1:

    NAME=enp0s3
    DEVICE=enp0s3

    BOOTPROTO=dhcp
    ONBOOT=yes

Ip all du Client1 : 

    [root@client1 network-scripts]# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 08:00:27:c8:bd:0b brd ff:ff:ff:ff:ff:ff
        inet 10.6.201.50/24 brd 10.6.201.255 scope global noprefixroute dynamic enp0s3
           valid_lft 593sec preferred_lft 593sec
        inet6 fe80::a00:27ff:fec8:bd0b/64 scope link
           valid_lft forever preferred_lft forever

### 4. Serveur DNS 


#### Mise en place

Sur server1.tp6.b1 :

    # Installation du serveur DNS
    sudo yum install -y bind*

    # Edition du fichier de configuration
    sudo vi /etc/named.conf

    # Edition des fichiers de zone
    sudo vi /var/named/forward.tp6.b1
    sudo vi /var/named/reverse.tp6.b1

    # Ouvrir les ports firewall concernés
    sudo firewall-cmd --add-port=53/tcp --permanent
    sudo firewall-cmd --add-port=53/udp --permanent
    sudo firewall-cmd --reload

    # Démarrage du service DNS
    sudo systemctl start named

    # Faire en sorte que le service démarre au boot de la VM
    sudo systemctl enable named

Ouvrir les ports firewall 53/TCP et 53/UDP : 

    [root@server1 etc]# sudo firewall-cmd --add-port=53/tcp --permanent
    success
    [root@server1 etc]# sudo firewall-cmd --add-port=53/udp --permanent
    success
    [root@server1 etc]# sudo firewall-cmd --reload
    success

A quelle IP correspond server1.tp6.b1 ?

    dig server1.tp6.b1
    ;; ANSWER SECTION:
    server1.tp6.b1.         604800  IN      A       10.6.202.10

A quelle IP correspond client2.tp6.b1 ?

    dig client2.tp6.b1
    ;; ANSWER SECTION:
    client2.tp6.b1.         604800  IN      A       10.6.201.10

    ;; AUTHORITY SECTION:
    tp6.b1.                 604800  IN      NS      server1.tp6.b1.

A quel nom de domaine correspond l'IP 10.6.201.10 ?

    dig -x 10.6.201.10
    ;; ANSWER SECTION:
    10.201.6.10.in-addr.arpa. 86400 IN      PTR     client1.tp6.b1.

    ;; AUTHORITY SECTION:
    6.10.in-addr.arpa.      86400   IN      NS      server1.tp6.b1.


### 5. Serveur NTP

Dans le fichier /etc/chrony.conf :

    # Servers to synchronize with
    # Replace XXX with needed server names
    server 0.fr.pool.ntp.org
    server 1.fr.pool.ntp.org
    server 2.fr.pool.ntp.org
    server 3.fr.pool.ntp.org

__

    [root@server1 etc]# chronyc sources
    210 Number of sources = 4
    MS Name/IP address         Stratum Poll Reach LastRx Last sample
    ===============================================================================
    ^? cluster010.linocomm.net       2   6     1     4   +415ms[ +415ms] +/-   55ms
    ^? dedi2-2018.mainguet.eu        3   6     1     5   +402ms[ +402ms] +/-   34ms
    ^? ns0.serverhouse.com           2   6     1     8   +399ms[ +399ms] +/-   91ms
    ^? web01.webhd.nl                3   6     1     8   +412ms[ +412ms] +/-  113ms
    
__

    [root@server1 etc]# chronyc tracking
    Reference ID    : 7F7F0101 ()
    Stratum         : 10
    Ref time (UTC)  : Tue Mar 05 11:18:04 2019
    System time     : 0.006055588 seconds slow of NTP time
    Last offset     : -0.009833601 seconds
    RMS offset      : 0.009833601 seconds
    Frequency       : 0.000 ppm slow
    Residual freq   : +0.000 ppm
    Skew            : 0.000 ppm
    Root delay      : 0.000000000 seconds
    Root dispersion : 0.000000000 seconds
    Update interval : 62.7 seconds
    Leap status     : Normal
