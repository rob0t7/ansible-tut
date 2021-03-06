# Ansible Breakout Session - Install Nginx with config

In this break out we will be using Amazon AWS for the 1st time and
we will be using ansible.

In this breakout we will be installing *nginx* with the following:

1. Configuration for virtual hosts
2. SSL for encryption

## Requirements

1. Ansible installed on your host machine
2. Amazon AWS account (you could do this tutorial with a virtual machine if you would like
3. Basic commandline skill

## Tutorial

We will be using a centos image for this tutorial. Feel free to use debian
based images if you'd like, just change the package command and package
names accordingly.

1. Install ansible
2. Launch virtual machine
3. Ensure that you can connect to it and your ssh key is added to your agent
4. Test the ansible connection

```SHELL
$ ansible -i hosts all -m ping
```

5. Create common playbook for all servers (branch: *part1*)

site.yml
```YAML
---
# Applies the whole application stack to all nodes

- name: apply common configuration to all nodes
  hosts: all
  roles:
    - common

```

roles/common/tasks/main.yml

```YAML
---
# This playbook contains common plays that will be run on all nodes

- name: Install EPEL
  sudo: yes
  yum: name=epel-release state=present

- name: Install NTP
  sudo: yes
  yum: name=ntp state=present
  tags: ntp

- name: Setup Timezone
  sudo: yes
  command: timedatectl set-timezone America/Toronto
  tags: ntp
  notify: restart ntp

- name: Start the ntp service
  sudo: yes
  service: name=ntpd state=started enabled=yes
  tags: ntp

```

6. Install and configure nginx

modify site.yml

```YAML

- name: configure the webservers
  hosts: webservers
  roles:
    - web

```

roles/web/tasks/main.yml

```YAML
---
# These tasks install nginx

- name: Install nginx
  sudo: yes
  yum: name=nginx state=present

- name: Enable nginx service
  sudo: yes
  service: name=nginx state=started enabled=yes

```


## Generating SSL key using OpenSSL

```SHELL
$ openssl genrsa -des3 -out lighthouse.key 2048
$ openssl req -new -key lighthouse.key -out lighthouse.csr
$ cp lighthouse.key lighthouse.key.orig
$ openssl rsa -in lighthouse.key.orig -out lighthouse.key
$ rm lighthouse.key.orig
$ openssl x509 -req -days 365 -in lighthouse.csr -signkey lighthouse.key -out lighthouse.crt
```
