# Install Ansible

#### Update Server
    yum update -y
##### Install Python & pip:
    yum install python
    yum install python-pip
##### Install Ansible & Check version :    
    pip install ansible
    ansible --version
##### Add User & Pwd :
    adduser ansadmin
    passwd ansadmin
##### Add user so it can login without pwd on EC2 ssh :
    visudo
        ansadmin    ALL=(ALL)   NOPASSWD: ALL
##### Password Authetication :
    vi /etc/ssh/sshd_config
        PasswordAuthentication yes
##### Restart sshd :    
    service sshd restart

#### STEP 2 (INSIDE USER=ANSADMIN)
##### Login to User :
    su - ansadmin
##### Create ssh-keygen key :
    ssh-keygen -t rsa
##### Confirm keygen :
    cd .ssh
    ls
##### Remote Setting to host :
    ssh-copy-id {IP Host}
##### Lets Remote host :
    ssh {IP Host}

#### STEP 3 (Create Host List on Server/Controller)
##### Go to etc/ansible/hosts & paste this !
    vi /etc/ansible/hosts
        [webservers]
        172.31.25.50
##### Ansible Ping test the remote :
    ansible --version
    ansible all -m ping
##### We Can try Test copy file & install apache on remote host :    
    ansible all -m yum -a "name=httpd state=present" -b
    ansible all -m service -a "name=httpd enabled=yes" -b
    ansible all -m service -a "name=httpd state=started" -b
###### Test on Host :
    service httpd status

#### Change User Credential from 'root' to 'ansadmin'
    chown -R ansadmin:ansadmin {folder}
