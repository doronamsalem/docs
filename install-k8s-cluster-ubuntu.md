# Install K8S Cluster Ubuntu 20.04
Follow this documentation to set up a Kubernetes cluster on __Ubuntu 20.04 LTS__.

This documentation guides you in setting up a cluster with one master node and one worker node.

## Assumptions
|Role|FQDN|IP|OS|RAM|CPU|
|----|----|----|----|----|----|
|Master|kmaster.example.com|172.16.16.100|Ubuntu 20.04|2G|2|
|Worker|kworker.example.com|172.16.16.101|Ubuntu 20.04|1G|1|

## On both Kmaster and Kworker
##### Login as `root` user
```
sudo su -
```
Perform all the commands as root user unless otherwise specified
##### Disable Firewall
##### Disable swap
##### Update sysctl settings for Kubernetes networking
##### Install docker engine
```
ufw disable
swapoff -a; sed -i '/swap/d' /etc/fstab
cat >>/etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
{
  apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
  add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  apt update
  apt install -y docker-ce=5:19.03.10~3-0~ubuntu-focal containerd.io
}
```
### Kubernetes Setup
##### Add Apt repository
##### Install Kubernetes components
```
{
  curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
  echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
}
apt update && apt install -y kubeadm=1.18.5-00 kubelet=1.18.5-00 kubectl=1.18.5-00
```

## On kmaster
##### Initialize Kubernetes Cluster
Update the below command with the ip address of kmaster
```
kubeadm init --apiserver-advertise-address=<172.16.16.100> --pod-network-cidr=192.168.0.0/16  --ignore-preflight-errors=all
```
##### Deploy Calico network
```
kubectl --kubeconfig=/etc/kubernetes/admin.conf create -f
https://docs.projectcalico.org/v3.14/manifests/calico.yaml
```

##### Cluster join command
```
kubeadm token create --print-join-command
```

##### To be able to run kubectl commands as non-root user
If you want to be able to run kubectl commands as non-root user, then as a non-root user perform these
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## On worker
##### Join the cluster
Use the output from __kubeadm token create__ command in previous step from the master server and run here.

## (On kmaster) Verifying the cluster 
##### Get Nodes status
```
kubectl get nodes
```
##### Get component status
```
kubectl get cs
```

Have Fun!!
