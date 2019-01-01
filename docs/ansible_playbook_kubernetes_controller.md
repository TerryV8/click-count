Edit ansible_playbook_kubernetes_controller.yml:

```console

---
- hosts: kubernetes_controller
  remote_user: ec2-user
  become: yes
  gather_facts: no

  tasks:
    - name: kubeadm init
      command: "kubeadm init --pod-network-cidr=10.123.0.0/16 --kubernetes-version=v1.11.3"
      #command: "sudo kubeadm init --pod-network-cidr=10.123.0.0/16"
      ignore_errors: yes

    - name: create .kube directory
      file:
        path: /home/ec2-user/.kube
        state: directory
        mode: 0755

    - name: copy admin.conf to user's kube config
      copy:
        src: /etc/kubernetes/admin.conf
        dest: /home/ec2-user/.kube/config
        remote_src: yes
        owner: ec2-user
        group: ec2-user

    - name: install Pod network
      become: no
      become_user: ec2-user
      command: "kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml"

```
