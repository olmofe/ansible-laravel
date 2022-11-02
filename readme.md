# Olmofe Ansible + Laravel Demo

This repository is a fairly modified repo of the [Automating Server Setup - a DigitalOcean Workshop Kit](https://www.digitalocean.com/community/meetup_kits/automating-server-setup-with-ansible-a-digitalocean-workshop-kit) and [laravel real world example](https://github.com/f1amy/laravel-realworld-example-app).

## Quick Setup

TL;DR:

Make sure you have [Ansible installed](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04) on your control node, which can be your local machine or a remote server dedicated to running Ansible.
The playbooks is verified for Ubuntu 18.04+ server to serve as node and host.

1. Clone this repository
2. edit the hosts file to your customer slave/php-server.You can also add for production(sample in inventory file `inventory-example`)
3. Adjust values on your `group_vars/all.yml` file.
The remote_user variable should be an existing can with sudo access, otherwise, you must launch ansible with -u root and uncomment lines in setup role that creates sudo user.
- -  if using existing remote_user, you should copy ansible server id_rsa.pub file (from wherever it is on the machine probably `~/.ssh`) into the authorized keys of the ansible slave/php server (probably founnd in `~/.ssh/authorized_keys`)
3. You can check if the ansible server is able using a ping eg. `ansible all -i hosts -m ping -u sudo-user`

4. Run the `server-setup.yml` playbook to set up the LEMP server
eg. `ansible-playbook -i hosts laravel-deploy.yml -u sudo-user`
5. Run the `laravel-deploy.yml` playbook to deploy the demo Laravel application.
6. Access your server's IP address or chosen hostname( same as http_host value in group vars ans should be set as an A record with your domain provider ) to test the setup.