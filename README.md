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

5. Start writing sites.yml file
