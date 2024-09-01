### Ansible server first setup after Ansible installation. This guide will detail connecting to a remote server on ubuntu virtual machines and connecting. It will also detail how to run a playbook. The steps will ultimately show how to setup dynamic inventory and run a playbook on a remote server(AWS) using ansible. First, we need to install ansible on the server. 
```bash 
sudo apt update
sudo apt install ansible
```
### Next, we need to create a user on the remote server. 
```bash
sudo adduser ansible
sudo usermod -aG sudo ansible
```
### Next, we need to create a password for the user. 
```bash
sudo passwd ansible
```
### We have to add the user to the sudoers file. 
```bash
sudo visudo
```
### Add the following line to the file. 
```bash
ansible ALL=(ALL) NOPASSWD: ALL
```
### Next, we need to generate an ssh key on the ansible server. 
```bash
ssh-keygen
```
### Next, we need to copy the ssh key to the remote server. 
```bash
ssh-copy-id ansible@remote_server_ip
```
### Next,we will create the inventory file in the project directory. 
```bash
nano inventory
```
### Add the following to the inventory file. 
```bash
[remote]
remote_server_ip
```
### Next, we will test the connection to the remote server. 
```bash
ansible -i inventory remote_name -m ping
```
### In case the connection fails, we can use the following command to troubleshoot. 
```bash
ansible -i inventory remote_name -m ping -vvv
```
### Take these steps to fix the connection issue. 
```bash
sudo nano /etc/ssh/sshd_config
```
### Add the following to the file. 
```bash
PasswordAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```
### Next, we will restart the ssh service. 
```bash
sudo systemctl restart ssh
```
### In your inventory file, add the following. 
```bash
[remote]
remote_server_ip ansible_user=ansible
```

### Next, we will create a playbook file in the project directory. 
```bash
nano playbook.yml
```
### Add the following to the playbook file. 
```bash
---
- hosts: remote
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
    - name: Install nginx
      apt:
        name: nginx
        state: present
    - name: Start nginx
      service:
        name: nginx
        state: started
```
### Next, we will run the playbook. 
```bash
ansible-playbook -i inventory playbook.yml
```
### Next, we will look at steps to setup dynamic inventory for AWS. 
```bash
pip install boto
pip install boto3
pip install botocore
```
### Next, we will create a file called aws_ec2.yml in the project directory. 
```bash
nano aws_ec2.yml
```
### Add the following to the aws_ec2.yml file. 
```bash
---
plugin: aws_ec2
regions:
  - us-east-1
keyed_groups:
    - key: tags
        prefix: tag
    ``` 
### Next, we will run the following command to create the dynamic inventory.
```bash
ansible-inventory -i aws_ec2.yml --graph
```
### Next, we will run the playbook using the dynamic inventory. 
```bash
ansible-playbook -i aws_ec2.yml playbook.yml
```
### This guide has shown how to setup an ansible server, connect to a remote server, and run a playbook. It has also shown how to setup dynamic inventory and run a playbook on a remote server(AWS) using ansible. 
```

