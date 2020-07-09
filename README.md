# DevOps CI/CD with :
# GitHub, Jenkins, Maven, and Tomcat on AWS Amazon EC2.

## Step 1 (Install Tools):
####* Install Linux on AWS EC2 Instance
    Please make sure to enable incoming traffic from port 22(SSH), 80(HTTP), and 443(HTTPS) on Security Group !
####* Install Java, Git, & Maven (https://maven.apache.org/download.cgi)
    yum install java-1.8*
    yum install git
#### *Install Jenkins on Linux server (https://www.jenkins.io/download/)
#### Start Jenkins
    systemctl start jenkins
#### Open Jenkins (Use Jenkins IP-public on EC2 Instance !)
    {ip-public}:8080 # put it on your browser !
    cat /var/lib/jenkins/secrets/initialAdminPassword #copy Jenkin's password !
#### Associate Java & Maven on Jenkins
    find / -name javac
    vim .bash_profile
#### Install these plugin on Jenkins :
    Maven Integration Plugin
    Maven Invoker Plugin
    Deploy to Container
#### Create new Project
    please test your installation with new freestyle project first !
#### Create new Maven-Project
    Source : https github repo

## Step 2 Create new Project > Deploy to Tomcat Server :
#### Install Linux on AWS EC2 Instance
#### Install Tomcat on Linux server



##### Install 
    Source : https github repo

## Step 4 :
#### Install Ubuntu on AWS EC2 as Tomcat WebServer

#### Install these plugin on Jenkins :
    Publish over SSH
