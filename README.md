# DevOps CI/CD with :
# GitHub, Jenkins, Maven, and Tomcat on AWS Amazon EC2.

## Step 1 (Install Tools):
#### 1. Launch new Linux server on AWS EC2 Instance
    # Please make sure to enable incoming traffic from port 22(SSH), 80(HTTP), and 443(HTTPS) on Security Group !
#### 2. Install Java, Git, & Maven (https://maven.apache.org/download.cgi)
    yum install java-1.8*
    yum install git
#### 3. Install Jenkins on Linux server (https://www.jenkins.io/download/)
#### 4. Start Jenkins
    systemctl start jenkins
#### 5. Open Jenkins (Use Jenkins IP-public on EC2 Instance !)
    {ip-public}:8080 # put it on your browser !
    cat /var/lib/jenkins/secrets/initialAdminPassword #copy Jenkin's password !
#### 6. Associate Java & Maven on Jenkins
    find / -name javac
    vim .bash_profile
#### 7. Install these plugin on Jenkins :
    Maven Integration Plugin
    Maven Invoker Plugin
    Deploy to Container
#### 8. Create new Project
    # please test your installation with new freestyle project first !
#### 9. Create new Maven-Project
    # Source : https github repo

## Step 2 Create new Project > Deploy to Tomcat Server :
#### Launch new Linux server AWS EC2 Instance
#### Install Java 8 or 11
    yum install java-1.8* -y
#### Install Tomcat on Linux server (https://tomcat.apache.org/download-90.cgi)
    cd /opt/tomcat/bin
    chmod +x startup.sh
    ./startup.sh
    # comment on --> /opt/tomcat9/webapps/manager/META-INF/context.xml
    # modify --> conf/tomcat-users.xml
        <role rolename="manager-gui"/>
        <role rolename="manager-jmx"/>
        <role rolename="manager-status"/>
        <role rolename="manager-script"/>
        <user username="tomcat" password="tomcat123" roles="manager-gui, manager-status, manager-jmx, manager-script"/>
        <user username="deployer" password="deployer" roles="manager-script"/>
        <user username="role1" password="role1" roles="role1"/>
#### Open Tomcat server with it's IP-Public followed by port 8080 !
    # {ip-public}:8080
#### Create New Project "Deploy **.war file on Tomcat server"
    # put github repo of maven-project
    # instal clean package
    # create Tomcat Credentials from the rolename we've create before (deployer), followed with tomcat ip
    # Build now !
#### Open Tomcat server !
    # {ip-public}:8080/webapp

## Step 3 Create new Project > Deploy with Docker :
#### Launch new Linux server AWS EC2 Instance
#### Create User for docker admin
    useradd dockadmin
    passwd dockadmin
    visudo # enable dockadmin to access sudo without password
    vim /etc/ssh/sshd_config # enable password auth from outside access
#### Docker credential
    docker images # check docker images on root
    su dockadmin # login to user dockadmin
    docker images # check docker images on dockadmin
    id dockadmin # find-out dockadmin group status
    cat /etc/group # find-put docker in group
    usermod -aG docker dockadmin # put dockadm on docker group
    chown -R dockadmin dockadmin .
#### Install this plugin on Jenkins :
    Publish over SSH
#### Lets put **.war file on to Docker server :
    cd /var/lib/jenkins/workspace/{project-name}/webapp
    # compare it with origin repo, while it has no target folder & **.war file !
    # Create New Project on Jenkins to put **.war file first on Docker server
    # Manage Jenkins > Configure System > SSH Server
    # Put Docker private IP, then test configuration !
    # Open the Project > Post Build Actions > Send build artifacts over SSH > select the Docker Server we've create before !
    # Source File : webapp/target/*.war
    # Remove prefix : webapp/target
    # Remote Directory : .
    # Save, then Build Now !
#### Create Dockerfile
    FROM tomcat:latest
    MAINTAINER coelgber
    COPY *.war /usr/local/tomcat/webapps
#### Build your Image
    docker build -t myimg .
    docker run -d --name mycontainer -p 8080:8080 myimg
    # Browse {public-ip}:8080/webapp
#### Provisioning Docker with Ansible
    # Launch new Linux server on AWS EC2 Instance
    # Install Ansible on Linux server
    yum install python -y
    yum update -y
    yum install python-pip
    pip install ansible
    ansible --version
    # Create user for ansadmin
    useradd ansadmin
    passwd ansadmin
    visudo # enable ansadmin to access sudo without password
    vim /etc/ssh/sshd_config #Enable password auth from outside access
    # add user ansadmin on Docker server !
    useradd ansadmin
    passwd ansadmin
    visudo # enable ansadmin to access sudo without password
    # Create Hosts on ansadmin
    mkdir /opt/docker #Create docker folder
    vim /opt/docker/hosts #Create hosts
        localhost
        {docker-privateIP}
    # Create SSH connection to docker server & localhost
    ssh-keygen
    ssh-copy-id ansadmin@{docker-privateIP} #Create connection to docker-server
    ssh -i ansadmin@{docker-privateIP} #Connection test
    ssh -i localhost #Create connection to localhost
    ssh -i localhost #Connection test to localhost
    # install docker on Ansible server
    yum install docker -y
    



#### Install Tomcat on Linux server 
    


##### Install 
    Source : https github repo

## Step 4 :
#### Install Ubuntu on AWS EC2 as Tomcat WebServer

#### Install these plugin on Jenkins :
    Publish over SSH
