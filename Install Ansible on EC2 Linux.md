# Install Ansible on Controller & Host
## (Ansible for Host only Step1 & Step2)
#### Step 1
    yum install python3 -y
    alternatives --set python /usr/bin/python3
    python --version
    yum -y install python3-pip
    useradd ansadmin
    passwd ansadmin

#### Step 2
    echo "ansadmin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
    sed -ie 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    sudo service sshd reload
    su - ansadmin
    pip3 install ansible --user
    ansible --version
    ssh-keygen

#### Step 3 (Test Remote)
##### 172.31.25.50 is Private IP Host
    ssh-copy-id ansadmin@172.31.25.50
    ssh 172.31.25.50

#### Step 4
##### Create etc/ansible/hosts
    [webservers]
    172.31.25.50
##### Try to ping test
    su - ansadmin
    ansible all -m ping
    
#### Step 5 (Test copy file & install apache on remote host)    
    ansible webservers -m yum -a "name=httpd state=present" -b
    ansible webservers -m service -a "name=httpd enabled=yes" -b
    ansible webservers -m service -a "name=httpd state=started" -b

