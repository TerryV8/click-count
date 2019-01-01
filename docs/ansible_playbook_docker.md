# Setup ansible playbook: Docker




Edit ansible_setup_docker.yml
```console
---
- hosts: all
  name: Setup Docker
  gather_facts: false
  remote_user: ec2-user
  become: yes

  tasks:
    - name: "Installing Docker Prerequisite packages"
      yum:
        name: ['yum-utils', 'device-mapper-persistent-data', 'lvm2']
        state: latest

    - name: "Installing another Docker Prerequisite package: container-selinux-2.9"
      command: "yum -y install ftp://bo.mirror.garr.it/1/slc/centos/7.1.1503/extras/x86_64/Packages/container-selinux-2.9-4.el7.noarch.rpm"
      register: command_result
      failed_when: command_result.rc!=0 and ("Nothing to do" not in command_result.stderr)

    - name: "Configuring docker-ce repo"
      get_url:
        url: https://download.docker.com/linux/centos/docker-ce.repo
        dest: /etc/yum.repos.d/docker-ce.repo
        mode: 0644

    - name: "Installing Docker latest version"
      yum:
        name: docker-ce
        state: latest

    - name: "Starting and Enabling Docker service"
      service:
        name: docker
        state: started
        enabled: yes
```


    
```
