# Playbook for setting up Kubernetes - on Master & Workers

First edit the inventory through /etc/ansible/host
according to the Public DNS (IPv4) for each servers found on the EC2 AWS UI Console,
```console
[k8s_master]
on_aws_public_ipv4_dns_of_machine_1

[k8s_worker]
on_aws_public_ipv4_dns_of_machine_2
on_aws_public_ipv4_dns_of_machine_3
on_aws_public_ipv4_dns_of_machine_4

```

Edit ansible_playbook_pre_kubernetes.yml:
```console
---
- hosts: all
  name: Setup the pre-requirement of Kubernetes Cluster on all machines
  gather_facts: false
  remote_user: ec2-user
  become: yes

  tasks:
    - name: Install the latest version of telnet, tc
      yum:
        name:
          - telnet
          - tc
        state: latest

    - name: Add Kubernetes repository
      yum_repository:
        name: kubernetes
        description: Kubernetes
        baseurl: https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
        enabled: 1
        gpgcheck: 0
        repo_gpgcheck: 0
        gpgkey: https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
        exclude: kube*

    - name: Disable SELinux
      command: "setenforce 0"
      ignore_errors: yes

    - name: Disable SELinux in the config file
      replace:
        path: /etc/selinux/config
        regexp: '^SELINUX=enforcing$'
        replace: 'SELINUX=permissive'

    - name: Install Kubelet, Kubeadm, Kubectl
      yum:
        name:
          - kubelet
          - kubeadm
          - kubectl
        state: latest
        disable_excludes: kubernetes

    - name: Start service kubelet
      service:
        name: kubelet
        state: started
        enabled: yes

    - name: Configure the bridge network
      block:
        - name: Configure the bridge network - ipv4
          lineinfile:
            path: /etc/sysctl.d/k8s.conf
            create: yes
            line: net.bridge.bridge-nf-call-ip6tables = 1

        - name: Configure the bridge network - ipv6
          lineinfile:
            path: /etc/sysctl.d/k8s.conf
            create: yes
           line: net.bridge.bridge-nf-call-iptables = 1

        - name: Apply the bridge network configuration"
          command: "sysctl --system"

- 
      command: "swapoff -a"

    - name: "Disable swap through kubeadm configuration"
    
    - name: Disable swap
      block:
      
        - name: Disable swap
          command: "swapoff -a"
          
        - name: Disable swap through kubeadm configuration
          lineinfile:
            path: /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
            line: Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"

    - name: Add Docker repository
      command: "yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo"

    - name: Install SeLinux from a remote repo - Requirement of Docker
      yum:
        name: http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.68-1.el7.noarch.rpm
        state: present

    - name: Install Docker
      yum:
        name: docker-ce-18.06.1.ce-3.el7
        state: present

    - name: Restart Docker, Kubelet
      service:
        name: "{{ item }}"
        state: restarted
        daemon_reload: yes
        enabled: yes
      with_items:
        - docker
        - kubelet



```




```console
---
- hosts: all
  name: Pre-setup all
  gather_facts: false
  remote_user: ec2-user
  become: yes

  tasks:
  - name: upgrade all packages
    yum:
      name: '*'
      state: latest

  - name: install the latest version of telnet, tc
    yum:
      name: 
        - telnet
        - tc
      state: latest   
   
   
  - name: define the repo of Kubernetes
    block:
      - file:
          path: /etc/yum.repos.d/kubernetes.repo
          owner: ec2-user
          group: ec2-user
          mode: 0644  
      - blockinfile:
        path: /etc/ssh/sshd_config
        block: |
          name=Kubernetes
          baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
          enabled=1
          gpgcheck=0
          repo_gpgcheck=0
          gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
          exclude=kube*

   
```        
```console
#- hosts: all
#  name: Setup Docker
#  gather_facts: false
#  remote_user: ec2-user
#  become: yes

  tasks:
    - name: "Installing Docker Prerequisite packages 1/2"
      yum:
        name: ['yum-utils', 'device-mapper-persistent-data', 'lvm2']
        state: latest

    - name: "Installing Docker (version 17.03)"
      yum:
        name:
          - https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-selinux-17.03.2.ce-1.el7.centos.noarch.rpm
          - https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-17.03.2.ce-1.el7.centos.x86_64.rpm
        state: present

    - name: "Starting and Enabling Docker service"
      service:
        name: docker
        state: started
        enabled: yes

#- hosts: all
#  name: Setup Kubernetes
#  gather_facts: false
#  remote_user: ec2-user
#  become: yes

  tasks:
    - name: "Add Kubernetes repository"
      yum_repository:
        name: kubernetes
        description: Kubernetes
        baseurl: https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
        enabled: 1
        gpgcheck: 1
        repo_gpgcheck: 0
        gpgkey: https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
        exclude: kube*

    - name: "Disable SELinux"
      command: "setenforce 0"
      ignore_errors: yes

    - name: "Disable SELinux in the config file"
      replace:
        path: /etc/selinux/config
        regexp: '^SELINUX=.*$'
        replace: 'SELINUX=permissive'

    - name: "Install Kubelet, Kubeadm, Kubectl"
      yum:
        name:
          - kubelet-1.11.3
          - kubeadm-1.11.3
          - kubectl-1.11.3
        disable_excludes: kubernetes

    - name: "Start service kubelet"
      service:
        name: kubelet
        state: started
        enabled: yes

    - name: "Configure the bridge network - ipv6"
      lineinfile:
        path: /etc/sysctl.d/k8s.conf
        create: yes     
        line: net.bridge.bridge-nf-call-ip6tables = 1

    - name: "Configure the bridge network - ipv4"
      lineinfile:
        path: /etc/sysctl.d/k8s.conf
        create: yes
        line: net.bridge.bridge-nf-call-iptables = 1

    - name: "Apply the bridge network configuration"
      command: "sysctl --system"

    - name: "Disable swap"
      command: "swapoff -a"

    - name: "Disable swap through kubeadm configuration"
      lineinfile:
        path: /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
        line: Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"

    - name: "Restart Docker, Kubelet"
      service:
        name: "{{ item }}"
        state: restarted
        daemon_reload: yes
      with_items:
        - docker
        - kubelet
        
       
```

Finally, launch:
```console
ansible-playbook ansible_playbook_pre_kubernetes.yml 
```
