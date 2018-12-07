# Installation for resilience

We created a Kubernetes cluster of 3 nodes running on Centos 7.
The installation uses a tool called kubeadm which is part of Kubernetes.

This process works with local VMs, physical servers and/or cloud servers. 
It is simple enough that you can easily integrate its use into your own automation (Terraform, Ansible).

kubeadm assumes you have a set of machines (virtual or real) that are up and running. It is designed to be part of a large provisioning system - or just for easy manual provisioning. kubeadm is a great choice where you have your own infrastructure (e.g. bare metal), or where you have an existing orchestration system (e.g. Ansible) that you have to integrate with.

If you are not constrained, there are some other tools built to give you complete clusters:

- On GCE, Google Container Engine gives you one-click Kubernetes clusters
- On AWS, kops makes cluster installation and management easy (and supports high availability)


- ### Prerequisites
One or more machines running Ubuntu 16.04+, CentOS 7 or HypriotOS v1.0.1+
1GB or more of RAM per machine (any less will leave little room for your apps)
Full network connectivity between all machines in the cluster (public or private network is fine)

- ### Objectives
Install a secure Kubernetes cluster on your machines
Install a pod network on the cluster so that application components (pods) can talk to each other
Install a sample microservices application (a socks shop) on the cluster


# Setting up
- ## (1/4) Installing kubelet and kubeadm on your hosts
You will install the following packages on all the machines:

- docker: the container runtime, which Kubernetes depends on. v1.11.2 is recommended, but v1.10.3 and v1.12.1 are known to work as well.
- kubelet: the most core component of Kubernetes. It runs on all of the machines in your cluster and does things like starting pods and containers.
- kubectl: the command to control the cluster once it’s running. You will only need this on the master, but it can be useful to have on the other nodes as well.
- kubeadm: the command to bootstrap the cluster.

Edit /etc/yum.repos.d/kubernetes.repo:
```console
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gcpcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
```

```console
sudo setenforce 0
sudo yum install -y kubelet-1.11.3 kubeadm-1.11.3 kubectl-1.11.3 --disableexcludes=kubernetes
sudo systemctl enable kubelet && sudo systemctl start kubelet
```

The kubelet is now restarting every few seconds, as it waits in a crashloop for kubeadm to tell it what to do.

Disabling SELinux by running setenforce 0 is required in order to allow containers to access the host filesystem, which is required by pod networks for example. You have to do this until kubelet can handle SELinux better.



Now, configure the bridge network

Edit /etc/sysctl.d/k8s.conf:
```console
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
```

Launch the command to apply the change:
```console
sysctl --system
```

First, make sure the swap is disabled:
```console
sudo swapoff -a
```

Add this line to /etc/systemd/system/kubelet.service.d/10-kubeadm.conf:
```console
Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"
```

Restart the kubelet service and docker service:
```console
systemctl restart docker && systemctl restart kubelet.service && systemctl daemon-reload
```


- ## (2/4) Initializing your master

The master is the machine where the “control plane” components run, including etcd (the cluster database) and the API server (which the kubectl CLI communicates with). All of these components run in pods started by kubelet.

To initialize the master of the Kubernetes cluster, pick one of the machines you previously installed kubelet and kubeadm on, and run:
```console
kubeadm init --pod-network-cidr=10.244.0.0/16 --kubernetes-version=v1.11.3
```

This will download and install the cluster database and “control plane” components. This may take several minutes.

Note: this will autodetect the network interface to advertise the master on as the interface with the default gateway. If you want to use a different interface, specify --api-advertise-addresses=<ip-address> argument to kubeadm init.


When initializing the master of the Kubernetes cluster, the output should look like :
```console
[init] using Kubernetes version: v1.11.3
[preflight] running pre-flight checks
	[WARNING Firewalld]: firewalld is active, please ensure ports [6443 10250] are open or your cluster may not function correctly
I1127 17:36:44.053697   23515 kernel_validator.go:81] Validating kernel version
I1127 17:36:44.053993   23515 kernel_validator.go:96] Validating kernel config
[preflight/images] Pulling images required for setting up a Kubernetes cluster
[preflight/images] This might take a minute or two, depending on the speed of your internet connection
[preflight/images] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[preflight] Activating the kubelet service
[certificates] Generated ca certificate and key.
[certificates] Generated apiserver certificate and key.
[certificates] apiserver serving cert is signed for DNS names [centos-linux.shared kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.211.55.4]
[certificates] Generated apiserver-kubelet-client certificate and key.
[certificates] Generated sa key and public key.
[certificates] Generated front-proxy-ca certificate and key.
[certificates] Generated front-proxy-client certificate and key.
[certificates] Generated etcd/ca certificate and key.
[certificates] Generated etcd/server certificate and key.
[certificates] etcd/server serving cert is signed for DNS names [centos-linux.shared localhost] and IPs [127.0.0.1 ::1]
[certificates] Generated etcd/peer certificate and key.
[certificates] etcd/peer serving cert is signed for DNS names [centos-linux.shared localhost] and IPs [10.211.55.4 127.0.0.1 ::1]
[certificates] Generated etcd/healthcheck-client certificate and key.
[certificates] Generated apiserver-etcd-client certificate and key.
[certificates] valid certificates and keys now exist in "/etc/kubernetes/pki"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
[controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
[controlplane] wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
[controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
[etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
[init] waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests" 
[init] this might take a minute or longer if the control plane images have to be pulled
[apiclient] All control plane components are healthy after 40.503617 seconds
[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.11" in namespace kube-system with the configuration for the kubelets in the cluster
[markmaster] Marking the node centos-linux.shared as master by adding the label "node-role.kubernetes.io/master=''"
[markmaster] Marking the node centos-linux.shared as master by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "centos-linux.shared" as an annotation
[bootstraptoken] using token: co7yxb.gw7vfym8a0i4p05f
[bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes master has initialized successfully!
```

As we can see, we set up a "secure (TLS)" Kubernetes cluster. We generated certificate and key (ca, apiserver, apiserver-kubelet-client, front-proxy-ca,  front-proxy-client, etcd, ...)

The key is used for mutual authentication between the master and the joining nodes.


The output also tells you that:
- You should deploy a pod network to the cluster:
```console
You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/
```

- You can join any number of machines by running the following on each node
as root:
```console
  kubeadm join 10.211.55.4:6443 --token co7yxb.gw7vfym8a0i4p05f --discovery-token-ca-cert-hash sha256:0e55d97ccc592def02237a424dca82d64fa383c63908af6161b2720177e58994
```




To start using the cluster, you need to run the following as a regular user:
```console
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```  

- ## (3/4) Installing a pod network

You must install a pod network add-on so that your pods can communicate with each other.

It is necessary to do this before you try to deploy any applications to your cluster, and before kube-dns will start up. Note also that kubeadm only supports CNI based networks and therefore kubenet based networks will not work.

Several projects provide Kubernetes pod networks using CNI, some of which also support Network Policy. See the add-ons page for a complete list of available network add-ons.

You can install a pod network add-on with the following command:
kubectl apply -f <add-on.yaml>

If you are on another architecture than amd64, you should use the flannel overlay network 

NOTE: You can install only one pod network per cluster.

Once a pod network has been installed, you can confirm that it is working by checking that the kube-dns pod is Running in the output of kubectl get pods --all-namespaces.

And once the kube-dns pod is up and running, you can continue by joining your nodes.

In order to set up the network properly, we are going to launch  Flannel:
```console
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml
```


What is Flannel ?:

If you want to use flannel as the pod network, specify --pod-network-cidr=10.244.0.0/16 if you’re using the daemonset manifest below. However, please note that this is not required for any other networks besides Flannel.

Flannel is a simple and easy way to configure a layer 3 network fabric designed for Kubernetes.
Flannel runs a small, single binary agent called flanneld on each host, and is responsible for allocating a subnet lease to each host out of a larger, preconfigured address space. Flannel uses either the Kubernetes API or etcd directly to store the network configuration, the allocated subnets, and any auxiliary data (such as the host's public IP). Packets are forwarded using one of several backend mechanisms including VXLAN and various cloud integrations.

Platforms like Kubernetes assume that each container (pod) has a unique, routable IP inside the cluster. The advantage of this model is that it removes the port mapping complexities that come from sharing a single host IP.

Flannel is responsible for providing a layer 3 IPv4 network between multiple nodes in a cluster. Flannel does not control how containers are networked to the host, only how the traffic is transported between hosts. However, flannel does provide a CNI plugin for Kubernetes and a guidance on integrating with Docker.

Flannel is focused on networking. For network policy, other projects such as Calico can be used.





- ## (4/4) Joining your nodes

The nodes are where your workloads (containers and pods, etc) run. 
If you want to add any new machines as nodes to your cluster, for each machine: SSH to that machine, become root (e.g. sudo su -) and run the command that was output by kubeadm init:
```console
  kubeadm join 10.211.55.4:6443 --token co7yxb.gw7vfym8a0i4p05f --discovery-token-ca-cert-hash sha256:0e55d97ccc592def02237a424dca82d64fa383c63908af6161b2720177e58994
```


> Note: If Kubeadm join getting error getsockopt: "no route to host" with the follow messages:

```
[preflight] Running pre-flight checks.
[WARNING] FileExisting-crict]: crictl not found in system path
   Suggestion: go get github.com/kubernetes-incubator/cri-tools/cmd/crictl
[discovery] Trying to connect to API Server "10.211.55.4:6443"
[discovery] Created cluster-info discovery client, requesting info from "https://199.230.107.137:6443"
[discovery] Failed to request cluster info, will try again: Get https://10.211.55.4:6443/api/v1/namespaces/kube-public/configmaps/cluster-info: dial tcp 10.211.55.4:6443: getsockopt: no route to host
```

whereas  I can ping the master, as well as ssh from the node to the master. 

The issue is that you have a firewall running on your master node that are blocking incoming traffic from slave nodes
- First solution: Open the 2 specific host ports on the master node.
```console
sudo firewall-cmd --zone=public --add-port=6443/tcp --permanent
sudo firewall-cmd --zone=public --add-port=10250/tcp --permanent
```

- Second solution if it still doesn't work: Disable the based firewall on the master node.
```console
systemctl disable firewalld
systemctl stop firewalld
```

After running those commands on your master, now go back to the node and attempt to join again.
That should resolve your issue.


A few seconds later, you should notice that running 
```console
kubectl get nodes
```
on the master shows a cluster with as many machines as you created.

The output should look like this:
```console
NAME                          STATUS    ROLES     AGE       VERSION
centos-linux-slave-1.shared   Ready     <none>    21m       v1.11.3
centos-linux-slave-2.shared   Ready     <none>    21m       v1.11.3
centos-linux.shared           Ready     master    22h       v1.11.3
```


