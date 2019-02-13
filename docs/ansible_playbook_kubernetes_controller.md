Edit ansible_playbook_kubernetes_controller.yml:

```console

---
- hosts: k8s_master
  remote_user: ec2-user
  become: yes
  gather_facts: no

  tasks:
    - name: Kubeadm init
      command: "kubeadm init --pod-network-cidr=10.244.0.0/16 > result_kubeadm_init"
      ignore_errors: yes

    - name: Create .kube directory
      file:
        path: /home/ec2-user/.kube
        state: directory
        mode: 0755
        owner: ec2-user
        group: ec2-user

    - name: Copy admin.conf to user's kube config on the remote master machine
      copy:
        src: /etc/kubernetes/admin.conf
        dest: /home/ec2-user/.kube/config
        remote_src: yes
        owner: ec2-user
        group: ec2-user

    - name: Install Pod network - No need for root priviledge
      become: no
      become_user: ec2-user
      command: "kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml"

```
