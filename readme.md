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
* nano /etc/hosts

# set a static ip address
* nano /etc/network/interfaces


```
auto enp0s8
iface enp0s8 inet static
address 192.168.56.101
```

# install docker

# Run following commands before installing Kube environments

* curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
* cat <<EOF >/etc/apt/sources.list.d/kubernetes.list  
> deb http://apt.kubernetes.io/ kubernetes-xenial main
> EOF
