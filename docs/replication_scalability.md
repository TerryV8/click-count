# Replication/Scalability (With Kubernetes)

We created a Kubernetes cluster of 3 nodes

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

