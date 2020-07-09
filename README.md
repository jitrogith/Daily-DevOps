# DevOps CI/CD with :
# GitHub, Jenkins, Maven, and Tomcat on AWS Amazon EC2.

## Step 1:
#### Install Linux on AWS EC2 Instance
#### Install Jenkins on AWS EC2 Instance (https://www.jenkins.io/download/)
#### Install Java, Git, & Maven (https://maven.apache.org/download.cgi)
    yum install java-1.8*
    yum install git
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
## Step 2:
#### Create new Project
    please test your installation with new freestyle project first !
#### Create new Maven-Project
    Source : https github repo
#### Create new Project > Deploy with Docker    
    Source : https github repo

## Step 4 :
#### Install Ubuntu on AWS EC2 as Tomcat WebServer

#### Install these plugin on Jenkins :
    Publish over SSH
