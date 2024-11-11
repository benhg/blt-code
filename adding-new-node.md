# Adding a new node:

1. Physical install
2. Ubuntu bringup & login
3. Figure out network interface names
```
glick@bacon:~$ cat /etc/netplan/00-installer-config.yaml
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
```
5. Create DHCP reservations for MAC addresses
    1. Test outgoing internet
```
/etc/dhcpd.conf
host bacon {
    hardware ethernet AC:1F:6B:05:4B:3A;
    fixed-address 192.168.0.109;
}
```



5. Add names to /etc/hosts

```
127.0.0.1 localhost
127.0.0.1 mayo
127.0.0.1 mayo.blt.lclark.edu mayo
192.168.0.2 mayo.blt.lclark.edu mayo

## Place this on each new worker node and add to it
192.168.0.109 bacon
192.168.0.111 lettuce
192.168.0.103 tomato
192.168.0.104 sprouts
192.168.0.107 bread
192.168.0.4 lettucebmc
192.168.0.3 breadbmc

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

6. Add NIS setup
```
glick@bacon:~$ cat /etc/yp.conf
#
# yp.conf	Configuration file for the ypbind process. You can define
#		NIS servers manually here if they can't be found by
#		broadcasting on the local net (which is the default).
#
#		See the manual page of ypbind for the syntax of this file.
#
# IMPORTANT:	For the "ypserver", use IP addresses, or make sure that
#		the host is in /etc/hosts. This file is only interpreted
#		once, and if DNS isn't reachable yet the ypserver cannot
#		be resolved and ypbind won't ever bind to the server.

ypserver mayo.blt.lclark.edu
```



7. Add FSTab mounts
```
...
existing fstab
...
mayo.blt.lclark.edu:/local       /local  nfs     defaults  0     0
mayo.blt.lclark.edu:/home       /home  nfs     defaults  0     0
bread:/bread                    /bread nfs     defaults 0 0
```
9. Add node to SLURM config with slot details
NOTE: SLURM is `slurm-wlm 19.05.5`
    a. Add munge key from other workers over


```

# COMPUTE NODES 
GresTypes=gpu
NodeName=bacon,lettuce,tomato CPUs=48 RealMemory=503000 Sockets=2 CoresPerSocket=12 ThreadsPerCore=2 State=UNKNOWN
NodeName=sprouts CPUs=48 RealMemory=377000 Sockets=2 CoresPerSocket=12 ThreadsPerCore=2 State=UNKNOWN Gres=gpu:rtx2080ti:4
NodeName=pickles,onions CPUs=64 RealMemory=503000 Sockets=2 CoresPerSocket=16 ThreadsPerCore=2 State=UNKNOWN
PartitionName=blt Nodes=ALL Default=YES MaxTime=INFINITE State=UP
```

10. Add `/etc/exports`
NOTE: must run `exportfs -ra`
