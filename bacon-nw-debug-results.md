# Debugging Bacon Networking learnings:

`ip` command has built in routing support, didn't know that:

`sudo ip route add default via 192.168.0.2` (setting up Mayo as gateway)

Then enabling IPTables on Mayo:
```
glick@mayo:~$ sudo iptables -t nat -A POSTROUTING -o enp5s0f0 -j MASQUERADE
[sudo] password for glick: 
glick@mayo:~$ sudo iptables -t nat -A POSTROUTING -o enp5s0f1 -j MASQUERADE

```

Kernel packet forwarding boot param has stuck, so we just won't change that.

Next step is to figure out how to assign the host name and IP mapping.
