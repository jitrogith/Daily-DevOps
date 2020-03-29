# Docker Installation

##### Step 1: Install Docker
      $ yum install docker -y

##### Step 2: Create User
      $ useradd dockeradmin
      $ passwd dockeradmin
      
##### Step 3: Add User to Docker Group to manage User
      $ usermod -aG docker dockeradmin
      
##### Allow access without Password:
      $ visudo
##### Activate Password Authentication
      $ vi /etc/ssh/sshd_config
            PasswordAuthentication yes
      $ service sshd reload

##### Install Plugin on Jenkins : 
            Publish over SSH
##### Create SSH Docker Credential  
