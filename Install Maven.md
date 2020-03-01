# Install Maven :

### Got to folder "opt", then Create Maven Folder [opt/maven]:
    mkdir maven
    cd maven

### Install wget :
    sudo apt install wget

### Download Maven :
    wget https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
  
### Unzip File Downloaded !
    tar -xvzf apache-maven-3.6.3-bin.tar.gz
  
### Go in to Maven Path  
    cd apache-maven-3.6.3-bin.tar.gz/bin
So, this is the Maven Path

### Go to root folder, then create maven.sh file !
    cd etc/profile.d
    touch maven.sh
    vim maven.sh
        
        export M2_HOME=/opt/maven
        export PATH=${M2_HOME}/bin:${PATH}
