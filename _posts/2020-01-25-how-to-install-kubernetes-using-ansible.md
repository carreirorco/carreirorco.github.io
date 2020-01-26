---
layout: post
title:  "How to install kubernetes using ansible!"
categories: [ansible, kubernetes]
tags: [ansible, k8s, kubernetes, linux, fedora, centos]
---

# How to install kubernetes using ansible

This is an easy way to install k8s using the ansible.

In this moment I'm using Fedora 31 on my local machine and CentOS 8 on remote server.

---

1 - install ansible in your local machine:
  
`sudo dnf install -y ansible`

2 - create a folder to work:

`mkdir -p ~/Lab/ansible/k8s/roles && cd ~/Lab/ansible/k8s`

3 - install roles from ansible-galaxy:

```
ansible-galaxy install geerlingguy.kubernetes -p roles
ansible-galaxy install geerlingguy.docker -p roles
```

4 - create playbook:

```
cat << EOF >> playbook.yml
- hosts: all

  vars:
    kubernetes_allow_pods_on_master: true

  roles:
    - geerlingguy.docker
    - geerlingguy.kubernetes
EOF
```

5 - create a inventory file:

`echo -e '[server]\nYOUR_SERVER_IP  ansible_connection=ssh  ansible_ssh_user=root  ansible_ssh_private_key=~/.ssh/id_rsa' > inventory`

6 - configure a new ssh key:

`ssh-keygen -t rsa`

7 - copy your public key to server:

`ssh-copy-id -i ~/.ssh/id_rsa root@YOUR_SERVER_IP`

8 - execute the playbook:

`ansible-playbook -i inventory playbook.yml`

That's all folks.

Now you have k8s installed on your remote machine in your remote server.

See you...

--- 

The roles used here are from https://galaxy.ansible.com/geerlingguy.

Thanks @geerlingguy !