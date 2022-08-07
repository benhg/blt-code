# Installing and configuring slurm

## Mayo
1. install MUNGE https://github.com/dun/munge/wiki/Installation-Guide
    a. Install zlib
    b. Install pkg-config
    c. Install bzibp2
    d. Make munge user
    e. Munge release: https://github.com/dun/munge/releases (0.5.15)'
    f. Note munge key `/etc/munge/munge.key`
2. 
3. 

Slurm configuration generator: https://slurm.schedmd.com/configurator.html

## worker nodes
1. Install Munge
2. Make sure munge key matches `/etc/munge/munge.key` on Mayo



### Installing Munge
```
sudo groupadd munge 
sudo useradd --system munge -gmunge
sudo apt install gcc autoconf libtool libssl-dev make
./bootstrap
./configure      --prefix=/usr      --sysconfdir=/etc      --localstatedir=/var      --runstatedir=/run --disable-dependency-tracking
make -j 48
sudo make install
sudo cp munge.key /etc/munge/munge.key # First, copy from Mayo
sudo chown munge:munge /etc/munge
sudo chown munge:munge /etc/munge/munge.key
sudo chmod 700 /etc/munge/munge.key
sudo systemctl start munge
sudo chown munge:munge /var/log/munge
sudo systemctl start munge
```
