Install Kubernetes

master node
child node


# commands


# update repositories
* sudo su 
* apt-get update 
 
# turn off swap space
* swapoff -a
* nano /etc/fstab


# update the hostname
* nano /etc/hostname

# set a static ip address
* nano /etc/network/interfaces
```auto enp0s8
iface enp0s8 inet static
address 192.168.56.101
```
