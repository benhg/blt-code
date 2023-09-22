https://unix.stackexchange.com/questions/339011/how-do-i-mount-an-lvm-partitionBREAD is a PITA....

First, problem was that in U22, we need to do the netplan a little different. This is what worked:
```
bread@bread:~$ cat /etc/netplan/00-installer-config.yaml
# This is the network config written by 'subiquity'
network:
  renderer: networkd
  ethernets:
    eno1:
      dhcp4: true
      dhcp4-overrides:
        use-routes: false
      routes:
        - to: default
          via: 192.168.0.2
      nameservers:
        search:
          - "mayo.blt.lclark.edu"
        addresses:
          - 192.54.243.2
    eno2:
      dhcp4: true
  version: 2

```


Then, there was a VG conflict: https://unix.stackexchange.com/questions/179855/device-already-mounted-or-resource-is-busy
- https://askubuntu.com/questions/926698/wipefs-device-or-resource-busy wiped disk
- https://unix.stackexchange.com/questions/339011/how-do-i-mount-an-lvm-partition Tried this
- https://www.linuxtechi.com/how-to-create-lvm-partition-in-linux/ created a new one (use xfs)
