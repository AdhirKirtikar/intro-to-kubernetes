# Introduction to Kubernetes
This repository contains the source code for creating a local Kubernetes cluster setup using Vagrant and [Rancher Kubernetes Engine (RKE)](https://rancher.com/docs/rke/latest/en/). It also contains a set of manifest files to deploy a basic application to your local K8s cluster.

## Requirements/Prerequisites
- [Rancher Kubernetes Engine (RKE)](https://rancher.com/docs/rke/latest/en/installation/)
- [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
- [Vagrant](https://www.vagrantup.com/docs/installation)

## Create Virtual Machines (VMs)
To spin up the virtual machines, run the following command at the root level of the project directory:
```
vagrant up
```
Once the VMs are up and running, you can check their status with `vagrant status` or by connecting to anyone of them using `vagrant ssh [hostname]`. Once you've confirmed that all machines are running with no issues, copy over the generated SSH keys from your workstation/host to each guest machine with the following commands:
```
ssh-copy-id root@[relevant ip address]

#PowerShell equivalent:
ssh-keygen
# xxxxx type $env:USERPROFILE\.ssh\id_rsa.pub | ssh root@172.16.131.21 "cat >> /home/vagrant/.ssh/authorized_keys"

rke:~ # pwd
/root
rke:~ # mkdir /root/.ssh
rke:~ # chmod 600 .ssh/
rke:~ # vi /root/.ssh/authorized_keys
rke:~ # chmod 600 authorized_keys

type C:\Users\Developer\.ssh\id_rsa.pub | ssh root@172.16.131.21 'cat >> .ssh/authorized_keys'
```
When prompted, enter the root user password configured in the bootstrap node script.

## Provision/Create Kubernetes cluster with RKE
To provision the cluster on the VMs, run the `rke config` command. You will be presented with a series of questions to which the answers will be used to declare the cluster config in a generated `cluster.yml` file upon completion. Alternatively, you can create the cluster.yml file and populate it with your desired configuration. Once you have the `cluster.yml` file, run the following command:
```
rke up
rke up --ignore-docker-version --config cluster.yml --disable-port-check --update-only
```
When the cluster has been provisioned, the following files will be generated in the root directory:
- cluster.rkestate - the cluster state file 
- kube_config_cluster.yml - kube config file (see local machine folder, not virtual machine folder, copy to vm using vi)

To add the cluster to your context, copy the kube config file:
```
cp kube_config_cluster.yml ~/.kube/config
```
If you do not have a `./kube` directory on your machine you will have to create one. 

# download kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"


The last step will be to check that you can connect to your cluster:
```
kubectl cluster-info

Kubernetes control plane is running at https://172.16.131.21:6443
CoreDNS is running at https://172.16.131.21:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
or
```
kubectl config current-context

my-k8s-cluster
```

## Manifests
The Kubernetes resources can be found inside the *manifests* folder.

## Copy manifests by WinSCP then kubectl apply each one

kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission

kubectl apply -f ingress.yaml

http://172.16.131.21:30694/test

add follow to /etc/hosts & C:\Windows\System32\drivers\etc\hosts: 
172.16.131.21 express-api.com