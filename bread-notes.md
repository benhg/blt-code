https://unix.stackexchange.com/questions/339011/how-do-i-mount-an-lvm-partitionBREAD is a PITA....

## U20 netplan

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

## Volume group create/mount

Then, there was a VG conflict: https://unix.stackexchange.com/questions/179855/device-already-mounted-or-resource-is-busy
- https://askubuntu.com/questions/926698/wipefs-device-or-resource-busy wiped disk
- https://unix.stackexchange.com/questions/339011/how-do-i-mount-an-lvm-partition Tried this
- https://www.linuxtechi.com/how-to-create-lvm-partition-in-linux/ created a new one (use xfs)


Now set up fstab:
```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda3 during curtin installation
/dev/disk/by-uuid/7ababf1a-41d1-4725-a917-ab6cc91c93ee / ext4 defaults 0 1
# /boot was on /dev/sda2 during curtin installation
/dev/disk/by-uuid/c5bdf60e-9cc0-486b-a3bc-b938f50ff6af /boot ext4 defaults 0 1
/swap.img	none	swap	sw	0	0
/dev/mapper/bread_vg-bread_lg       /bread  xfs     defaults  0     0
mayo.blt.lclark.edu:/local       /local  nfs     defaults  0     0
mayo.blt.lclark.edu:/home       /home  nfs     defaults  0     0
```

## NFS

`sudo apt install nfs-common nfs-kernel-server` everywhere


Edit bread `/etc/exports`:

```
root@bread:/etc# touch /bread/ben
root@bread:/etc# cat exports
# /etc/exports: the access control list for filesystems which may be exported
#		to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/bread    bacon(rw,sync,no_subtree_check) lettuce(rw,sync,no_subtree_check) tomato(rw,sync,no_subtree_check) sprouts(rw,sync,no_subtree_check) mayo(rw,sync,no_subtree_check)
```

Add to all FSTAB:

```
bread:/bread                    /bread nfs     defaults 0 0
```

## YPbind/NIS

For YPBind, since this is U22, it's a little different. First, manually create /etc/defaultdomain. Copy from Bacon:
```
glick@bacon:~$ cat /etc/defaultdomain 
mayo.blt.lclark.edu
```

Then follow this one: https://www.server-world.info/en/note?os=Ubuntu_22.04&p=nis&f=2


