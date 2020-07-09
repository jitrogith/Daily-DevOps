# DevOps CI/CD with :
# GitHub, Jenkins, Maven, and Tomcat on AWS Amazon EC2.

## Step 1 (Install Tools):
#### 1. Install Linux on AWS EC2 Instance
    Please make sure to enable incoming traffic from port 22(SSH), 80(HTTP), and 443(HTTPS) on Security Group !
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
    please test your installation with new freestyle project first !
#### 9. Create new Maven-Project
    Source : https github repo

## Step 2 Create new Project > Deploy to Tomcat Server :
#### Install Linux on AWS EC2 Instance
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
#### Install Linux on AWS EC2 Instance
#### Create User for docker admin
    useradd dockadmin
    passwd dockadmin
    visudo # add dockadmin to enable cmd login without cmd password
    vim /etc/ssh/sshd_config # enable password auth from outside access
    
#### Install Docker
    yum install java-1.8* -y
#### Install Tomcat on Linux server 



##### Install 
    Source : https github repo

## Step 4 :
#### Install Ubuntu on AWS EC2 as Tomcat WebServer

#### Install these plugin on Jenkins :
    Publish over SSH
