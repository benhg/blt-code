```
systemctl restart isc-dhcp-server

file is `/var/lib/dhcpd/dhcpd.leases`  

refs:
http://www.ipamworldwide.com/ipam/isc-lease-file.html
https://askubuntu.com/questions/392599/how-to-reserve-ip-address-in-dhcp-server
https://askubuntu.com/questions/1403367/setup-dhcp-server-on-ubuntu-20-04

192.168.0.109 bacon
192.168.0.111 lettuce
192.168.0.103 tomato
192.168.0.104 sprouts
192.168.0.107 bread

Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.0.8                      (incomplete)                              enp5s0f0
192.168.0.109            ether   ac:1f:6b:05:4b:3a   C                     enp5s0f0
192.168.0.103            ether   ac:1f:6b:05:4b:38   C                     enp5s0f0
172.16.52.1              ether   b4:0c:25:e0:80:43   C                     enp5s0f1
192.168.0.111            ether   ac:1f:6b:05:4b:30   C                     enp5s0f0
192.168.0.107                    (incomplete)                              enp5s0f0
192.168.0.3              ether   3c:ec:ef:17:f2:3e   C                     enp5s0f0
192.168.0.7              ether   3c:ec:ef:1a:33:d4   C                     enp5s0f0
192.168.0.104            ether   0c:9d:92:c4:f3:aa   C                     enp5s0f0
192.168.0.4              ether   0c:c4:7a:de:72:85   C                     enp5s0f0
192.168.0.102                    (incomplete)                              enp5s0f0

```
