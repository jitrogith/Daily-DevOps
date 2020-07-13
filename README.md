# DevOps CI/CD with :
# GitHub, Jenkins, Maven, and Tomcat on AWS Amazon EC2.

## Step 1 (Install Tools):
#### 1. Launch new Linux server on AWS EC2 Instance
    # Please make sure to enable incoming traffic from port 22(SSH), 80(HTTP), and 443(HTTPS) on Security Group !
#### 2. Install Java, Git, & Maven (https://maven.apache.org/download.cgi)
    yum update -y
    yum install java-1.8*
    yum install git
#### 3. Install Jenkins on Linux server (https://www.jenkins.io/download/)
#### 4. Start Jenkins
    systemctl start jenkins
#### 5. Open Jenkins (Use Jenkins IP-public on EC2 Instance !)
    {ip-public}:8080 # put it on your browser !
    cat /var/lib/jenkins/secrets/initialAdminPassword #copy Jenkin's password !
#### 6. Associate Java & Maven on Jenkins
    # Download Maven, then save it on /opt/maven
    find / -name javac
    vim .bash_profile
        JAVA_HOME={java_directory}
        M2=/opt/maven
        M2_HOME=$M2/bin
        PATH=$JAVA_HOME:$M2:$M2_HOME
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
    # install clean package
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
#### Install Jenkins plugin & apply Docker server :
    # Plugin : Publish over SSH
    # Apply Docker server on SSH publisher
#### Lets put **.war file on to Docker server :
    cd /var/lib/jenkins/workspace/{project-name}/webapp
    # compare it with origin repo, while it has no target folder & **.war file !
    # Create New Project on Jenkins to put **.war file first on Docker server
    # Manage Jenkins > Configure System > SSH Server
    # Put Docker private IP, then test configuration !
    # Open the Project > Post Build Actions > Send build artifacts over SSH > select the Docker Server we've create before !
        Source File : webapp/target/*.war
        Remove prefix : webapp/target
        Remote Directory : .
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
    # Create hosts file on root
    vim /etc/ansible/hosts
        localhost
        {docker-privateIP}
    # add user ansadmin on Docker server !
    useradd ansadmin
    passwd ansadmin
    visudo # enable ansadmin to access sudo without password
    copy /home/dockadmin/* /home/ansadmin
    su - ansadmin
    sudo usermod -aG docker ansadmin
    sudo chown -R ansadmin:ansadmin .
    # Install Docker on Ansible server
    yum install docker -y
    systemctl start docker
    su - ansadmin
    sudo usermod -aG docker ansadmin
    sudo chown -R ansadmin:ansadmin .
    # Create Docker folder
    su - ansadmin
    sudo mkdir /opt/docker #Create docker folder
    cd /opt/docker
    sudo chown -R ansadmin:ansadmin .
    # Create Hosts on ansadmin
    vim /opt/docker/hosts #Create hosts file and put all hosts inside
        [local]
        localhost
        [docker]
        {docker-privateIP}
    # Create SSH connection to docker server & localhost
    ssh-keygen
    ssh-copy-id ansadmin@{docker-privateIP} #Create connection to docker-server
    ssh ansadmin@{docker-privateIP} #Connection test
    ssh-copy-id localhost #Create connection to localhost
    ssh localhost #Connection test to localhost
    # Ansible ping test on hosts
    ansible all -m ping
    # Create/copy Dockerfile from Docker server to /opt/docker directory
    # Create Ansible Server as SSH Publisher
        Manage Jenkins > System Configure > SSH Server
    # Put webapp.war file on /opt/docker directory
        SSH Server : Ansible Server
        Source File : webapp/target/*.war
        Remove prefix : webapp/target
        Remote Directory : //opt//docker   #Make sure use "//" not "/"
    # Create Ansible-Playbook file for CI-docker
    vim /opt/docker/CI-docker.yml
        ---
        - name: upload image to dockerhub
          hosts: local
          user: ansadmin
          tasks:
            - name: create docker image
              command: docker build -t myimg .
              args:
                chdir: /opt/docker
            - name: tag docker image
              command: docker tag myimg rootdock/myimg:1.4
            - name: push docker image to dockerhub
              command: docker push rootdock/myimg
            - name: remove image
              command: docker rmi myimg:latest rootdock/myimg:1.4
    # Login your dockerhub credential login with:
    docker login
    # Run Ansible Playbook for CI-docker
    ansible-playbook -i hosts CI-docker.yml
    # Check your dockerhub repo !
    # Crate Ansible-Playbook file for CD-docker
    vim /opt/docker/CD-docker.yml
        ---
        - name: pull & deploy image
          hosts: docker
          become: true
          user: ansadmin
          tasks:
            - name: stop run container
              command: docker stop mycontainer
              ignore_errors: yes
            - name: remove container
              command: docker rm mycontainer
              ignore_errors: yes
            - name: remove image
              command: docker rmi rootdock/myimg:1.4
              ignore_errors: yes
            - name: pull image from dockerhub
              command: docker pull rootdock/myimg:1.4
            - name: run image
              command: docker run -d --name mycontainer -p 8080:8080 rootdock/myimg:1.4
    # Create full CICD on Jenkins with Ansible playbook by put these command on "Exec Command":
        ansible-playbook -i /opt/docker/hosts /opt/docker/CI-docker.yml; ansible-playbook -i /opt/docker/hosts /opt/docker/CD-docker.yml
    # Build Triggers:
        Configure > Build Triggers > Pull SCM > * * * * *  #Every minutes
    # Let's try to change the source file
        cd webapp/src/main/webapp
        vim index.jsp
        # Change text and bgcolor !
    # HAPPY DEVOPS !


## Step 4 : CICD Pipeline with Kubernetes
#### Install Linux server on AWS EC2 Instance
    Create Roles that allow these Policies, then attach them on instance :
        AmazonEC2FullAccess
        IAMFullAccess
        AmazonS3FullAccess
        AmazonRoute53FullAccess
        AmazonVPCFullAccess
    Install AWS-CLI to enable remote AWS from our CLI command
        curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
        yum update -y
        yum install unzip
        yum install python -y
        unzip awscli-bundle.zip
        #sudo yum-get install unzip - if you dont have unzip in your system
        ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
    Install Kubeclt to enable Kubectl command and control k8s cluster
        curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
        chmod +x ./kubectl
        sudo mv ./kubectl /usr/local/bin/kubectl
    Install KOPS to enable AWS create Kubernetes cluster
        curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
        chmod +x kops-linux-amd64
        sudo mv kops-linux-amd64 /usr/local/bin/kops
    On Route53 :
        Create Hosted Zone with our domain name (select Private Hosted zone)
        Apply name-servers to our DNS hosting
    Configure AWS CLI
        aws configure
    Create bucket on S3 with our domain name
        aws s3 mb s3://demo.k8s.myshop81.com
    Create ssh-keygen
        ssh-keygen
    Create k8s cluster:
        export KOPS_STATE_STORE=s3://demo.k8s.myshop81.com
        aws ec2 describe-availability-zones
        kops create cluster --cloud=aws --zones=ap-southeast-1a,ap-southeast-1b,ap-southeast-1c --name=demo.k8s.myshop81.com --dns-zone=myshop81.com --dns private
        
        






