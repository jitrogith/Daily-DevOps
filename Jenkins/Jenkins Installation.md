# Before Install Jenkins, make sure Java has been Installed and Path with on location

### Check Java version
	sudo su -
	java -version
## Install JDK if Java is not installed yet :
	yum install java-1.8*
## Confirm Java :
	java -version
	find / -name javac
Copy /usr/lib/jvm/java-8-openjdk-amd64 for Java plugin on Jenkins !

------------------------
# Installing Jenkins:

### Linux:
##### RedHat, Fedora, CentOS:
	https://pkg.jenkins.io/redhat-stable/
##### Ubuntu & Debian
	https://pkg.jenkins.io/debian-stable/

### Others (Windows, MacOS, etc)
	https://jenkins.io/download

### After it has been installed successfully, we will start the jenkins service using the following commands :
	sudo systemctl start jenkins

### Lets check the status of the service :
	sudo systemctl status jenkins
	
### Open Jenkins on browser, for example if the IP is 10.10.10.1 :
	http://10.10.10.1:8080
	
### Then put password from this command :
	cat /var/lib/jenkins/secrets/initialAdminPassword
	
