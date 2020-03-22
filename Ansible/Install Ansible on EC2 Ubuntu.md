# Install Ansible on EC2 Ubuntu

### Create Ubuntu EC2 Instance

### SSH EC2 Instance and Install Python
    apt update -y
    apt install python-minimal
 
### Install Ansible
    apt install ansible
    ansible --version
    
=====================================

## If you want to create Ansible Master & Client

Create Ubuntu EC2 Instance for Ansible Client
Install Ansible on it !
-------------
### On Ansible Master:
#### Create ssh-keygen:
    [AnsMaster]$ ssh-keygen -t rsa
##### Enter & don't put or change anything until complete !
#### Copy id_rsa.pub
    [AnsMaster]$ cd .ssh/id_rsa.pub
##### Copy the keygen code !
#### Paste the keygen code to AnsClient:
    [AnsClient]$ cat > .ssh/authorized_keys
##### (paste in here !)    
### Test the connection :
###### (for example IP Public Ansible Client is 79.29.29.29)
    [AnsMaster]$ ssh -i id_rsa 79.29.29.29
###### Make sure to succeed the remote before continue to the next step !

#### Open hosts folder:
    [AnsMaster]$ cd etc/ansible/hosts
 
#### Put this inside hosts :
###### (for example IP Private Ansible Client is 172.31.20.26)
    #
    [webserver]
    172.31.20.26
    #
### Ping the Ansible Client from Ansible Master:
    [AnsMaster]$ ansible -m ping all
    
    172.31.20.26 | SUCCESS => {
    "changed": false,
    "ping": "pong"
    }

### Create Username & Password:
    useradd ansible
    passwd ansible

### Add User&Pwd to system:
    visudo
##### Add this user :
    ansible     ALL=(ALL)   NOPASSWD: ALL
    
### Enable PasswordAuthentication by activate it (change 'no' to 'yes'):
    cd ..
    vim etc/ssh/sshd_config
            PasswordAuthentication yes
##### Restart sshd:
    service sshd restart
    
    
            
