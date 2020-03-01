# Before Install Jenkins, make sure Java has been Installed and Path with on location

### Check Java version
	java -version
## Install JDK if Java is not installed yet :
	sudo apt install default-jdk
## Confirm Java :
	java -version
	find / -name javac
Copy /usr/lib/jvm/java-8-openjdk-amd64 for Java plugin on Jenkins !

------------------------
# Installing Jenkins:

### First, we’ll add the repository key to the system, run the below command from your terminal :
	wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -

### Next, we’ll append the Debian package repository address to the server’s sources.list:
	echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list

### Let’s run a system update:
	sudo apt-get update

### Now let’s install Java (must be installed first) and Jenkins :
	sudo apt-get install openjdk-8-jdk jenkins -y

### After it has been installed successfully, we will start the jenkins service using the following commands :
	sudo systemctl start jenkins

### Lets check the status of the service :
	sudo systemctl status jenkins
	
### Open Jenkins on browser, for example if the IP is 10.10.10.1 :
	http://10.10.10.1:8080
	
### Then put password from this command :
	cat /var/lib/jenkins/secrets/initialAdminPassword
	
