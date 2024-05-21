# Debugging Bacon Networking learnings:

`ip` command has built in routing support, didn't know that:

`sudo ip route add default via 192.168.0.2` (setting up Mayo as gateway)


permanently: 

```

netplan.conf

# This is the network config written by 'subiquity'
network:
  ethernets:
    eno1:
      dhcp4: true
      routes:
      - to: 0.0.0.0/0
        via: 192.168.0.2
    eno2:
      dhcp4: true
  version: 2
~                                
```

Then enabling IPTables on Mayo:
```
glick@mayo:~$ sudo iptables -t nat -A POSTROUTING -o enp5s0f0 -j MASQUERADE
[sudo] password for glick: 
glick@mayo:~$ sudo iptables -t nat -A POSTROUTING -o enp5s0f1 -j MASQUERADE

```
NOTE: must run this on both mayo and workers.


Kernel packet forwarding boot param has stuck, so we just won't change that.

Next step is to figure out how to assign the host name and IP mapping.


Assigning static IP:

bad way: 

glick@mayo:~$ cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 mayo
192.168.0.109 bacon
192.168.0.110 lettuce
192.168.0.103 tomato

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
