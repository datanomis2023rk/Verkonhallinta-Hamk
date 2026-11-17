Viikko 1 – Verkon dokumentointi

1. Johdanto

Tämä on virtuaalinen laboratorioympäristö verkonhallinnan harjoitteluun.

2. Verkkokaavio

Katso reports/images/week01.2-Topology.jpeg

                  Branch Office
                  10.10.30.0/24

                  branch-client
                        |
                       r3
                        |
                 10.255.23.0/30
                        |
                       r2
          --------------+--------------
          |                           |
          |                           |
     10.10.20.0/24              10.10.99.0/24
        Server LAN             Management LAN

      	web1       	              ansible
        db1                	       grafana
                                   prometheus
                                   zabbix
				       syslog
				       netbox
                        |
                 10.255.12.0/30
                        |
                       r1
                        |
                 10.10.10.0/24
                    User LAN

	      	       client1
		       client2	
                     attacker

3. Laiteluettelo

Laite             Tarkoitus

r1		            Reititin
r2		            Reititin
r3	            	Reititin
client1	      	Käyttäjä
attacker	         Tietoturva
web1		         Palvelin
db1		         Palvelin
branch-client     Käyttäjä
ansible		      Hallinta
prometheus	      Hallinta
grafana	      	Hallinta
zabbix		      Hallinta

4. IP-suunnitelma

Verkko		        Tarkoitus		         Yhdyskäytävä
10.10.10.0/24	     User LAN 10		      10.10.10.1
10.10.20.0/24	     Server LAN 20		   10.10.20.1
10.10.30.0/24	     Remote LAN		      10.10.30.1
10.10.99.0/24	     Management LAN 30     10.10.99.1
10.255.12.0/30	     Link R1/R2	         10.255.12.1/2
10.255.23.0/30      Link R2/R3            10.255.23.1/2

5. Reitityksen analyysia

# root@client1:/# ip addr

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0@if98: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 36:22:4d:af:13:5f brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.20.20.9/24 brd 172.20.20.255 scope global eth0
       valid_lft forever preferred_lft forever

# root@client1:/# ip route

default via 172.20.20.1 dev eth0
172.20.20.0/24 dev eth0 proto kernel scope link src 172.20.20.9

# root@client1:/# ping -c 4 10.10.20.101

PING 10.10.20.101 (10.10.20.101) 56(84) bytes of data.

--- 10.10.20.101 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3055ms

# root@client1:/# ping -c 4 10.10.30.101
PING 10.10.30.101 (10.10.30.101) 56(84) bytes of data.

--- 10.10.30.101 ping statistics ---
4 packets transmitted, 0 received, 100% packet loss, time 3079ms

# root@client1:/# traceroute 10.10.30.101

traceroute to 10.10.30.101 (10.10.30.101), 30 hops max, 60 byte packets
 1  172.20.20.1 (172.20.20.1)  0.886 ms  0.644 ms  0.624 ms
 2  Lenovo7.mshome.net (172.22.128.1)  0.611 ms  0.562 ms  0.527 ms
 3  91-154-224-2.elisa-laajakaista.fi (91.154.224.2)  13.825 ms  13.684 ms  13.665 ms

Tutkitaan:

löytyykö yhteys kaikkiin verkkoihin
mitä reittiä liikenne kulkee
mitä reitittimiä reitille kuuluu

6. Yhteenveto
Virtuaalisen ympäristön vaihtuminen Packet Traceristä kontteihin vaatii totuttelua.
Verkon topologian visualisointi piirtotyökalulla selkeyttää sen hahmottamista. 
Dokumentaation integrointi osaksi git projektinhallintaa pitää sen päivitettynä.
