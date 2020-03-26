# Install Tomcat

### Install Java
	yum install java-1.8*
	java -version

### Download Tomcat Installer
	wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.31/bin/apache-tomcat-9.0.31.tar.gz
	
### Unzip Tomcat
	ls -la
	tar -xvzf apache-tomcat-9.0.31.tar.gz

### Move Tomcat to tomcat folder
	mv apache-tomcat-9.0.31.tar.gz tomcat
	
	cd tomcat/bin
	chmod +x startup.sh
	./startup.sh

### Access Tomcat server from browser, by type (for example 10.10.10.10):
	10.10.10.10:8080
	
### To activate access to Manager :
	find / -name context.xml
	vim /root/tomcat/webapps/manager/META-INF/context.xml
#### Comment/Delete this script :
	<Valve className="org.apache ... 
      ... 0:0:1" />
  				
### Add this script inside of /tomcat/conf/tomcat-users.xml
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>

### Then try to login in to manager by click "Manager App" on the right top corner.
Put username="admin" password="admin"
Succeed !!!

Don't forget to install "Deploy to Container" Plugin on Jenkins !

