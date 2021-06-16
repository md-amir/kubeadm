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


```
auto enp0s8
iface enp0s8 inet static
address 192.168.56.101
```

# install docker

# Run following commands before installing Kube environments

``` curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list  
> deb http://apt.kubernetes.io/ kubernetes-xenial main
> EOF
```
 
# Install keubeadm, kubelet & kubectl
* install -y kubelet kubeadm kubectl

# Update the kubernetes configuration
* nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf


add line below
>Environment="cgroup-driver=systemd/cgroup-driver=cgroupfs"


# On master
* sudo kubeadm init --pod-network-cidr=<> --apiserver-advertise-address=<ip-address-of-master>
//for starting a Calico CNI: 192.168.0.0/16 or for starting a flannel CNI 10.244.0.0/16 

# Run the following command a normal user 
To start using your cluster, you need to run the following as a regular user:

```  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Alternatively, if you are the root user, you can run:

``` export KUBECONFIG=/etc/kubernetes/admin.conf ```

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

```kubeadm join 192.168.1.23:6443 --token 6nkbau.r4h0xwavszviwohi \
	--discovery-token-ca-cert-hash sha256:02cd68ea1e9a5748d045fbcb0c9ab1aaaed4b3fa2d39c7e3d6872861158f6319 
```

	
# Some commont kubectl commands

* kubectl get nodes // status of Nodes
* kubectl get pods --all-namespaces // status of pods
* kubectl get -o wide pods --all-namespaces // Detailed status of PODS

# For creating a POD based on Calico 
* https://docs.projectcalico.org/getting-started/kubernetes/quickstart
* kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

# Install dashboard
* kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml

# You can access Dashboard using the kubectl command-line tool by running the following command:
 > kubectl proxy

# creating service account for your Dashboard
* kubectl create serviceaccount dashboard -n default

# To add cluster binding rules for your roles on dahshboard
* kubectl create clusterrolebinding dashboard-admin -n default \
  --clusterrole=cluster-admin \
  --serviceaccount=default:dashboard

  # To get the secret key ot be pasted into the dashboard token pwd. Copy the outcoming secret key
  * kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}"
