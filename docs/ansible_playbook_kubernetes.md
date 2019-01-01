
```console
---
- hosts: all
  name: Setup Kubernetes
  gather_facts: false
  remote_user: ec2-user
  become: yes

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
