# Mayo
There's lots of links. This one is the right one: https://help.ubuntu.com/community/SettingUpNISHowTo
also this one is ok: https://junyonglee.me/ubuntu/nis/setting-up-NIS-for-ubuntu/



# worker nodes

apt-get install -y rpcbind nis


/etc/yp.conf: ypserver mayo.blt.lclark.edu
Hosts: 192.168.0.2 mayo mayo.blt.lclark.edu

sudo sed -i 's/compat$/compat nis/g;s/dns$/dns nis/g' /etc/nsswitch.conf


/etc/pam.d/common-session: session optional        pam_mkhomedir.so skel=/etc/skel umask=000

root@bacon:/home/glick# sudo systemctl restart rpcbind
root@bacon:/home/glick# sudo systemctl restart nis

test: ypcat passwd
