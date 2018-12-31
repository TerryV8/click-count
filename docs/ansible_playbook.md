# ansible playbook 1




Edit ansible_setup.yml
```console
---
- hosts: all
  name: Setup everything
  gather_facts: false
  remote_user: ec2-user
  sudo: yes

  tasks:
    - command: echo "Thierry"


        
        
    
```
