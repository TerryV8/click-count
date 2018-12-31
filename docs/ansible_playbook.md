# ansible playbook 1




Edit ansible_setup.yml
```console
---
- hosts: alll
  name: Setup everything
  gather_facts: true
  sudo: yes
  become_user: ec2-user
  
  tasks:
    - command: cat /etc/motd

        
        
    
```
