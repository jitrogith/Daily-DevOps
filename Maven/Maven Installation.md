# Install Maven :

### Create Maven Folder [opt/maven]:
    mkdir /opt/maven
    cd maven

### Install wget :
    sudo apt install wget

### Download Maven :
    wget https://downloads.apache.org/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
  
### Unzip File Downloaded !
    tar -xvzf apache-maven-3.6.3-bin.tar.gz
  
### Go in to Maven Path  
    cd apache-maven-3.6.3-bin.tar.gz/bin
    pwd
        /opt/maven/apache-maven-3.6.3
So, this is the Maven Path

### IF YOU find .bash_profile on root:
    vi .bash_profile
    
        JAVA_HOME={JAVA_HOME}                               #Put JAVA_HOME inside the {} like on Maven below !
        M2_HOME=/opt/maven/apache-maven-3.6.3
        M2=$M2_HOME/bin
        
        PATH=$PATH:$JAVA_HOME:$M2_HOME:$M2:$HOME/bin

### IF YOU CANNOT find .bash_profile on root, then create maven.sh file !
    cd etc/profile.d
    touch maven.sh
    vim maven.sh
        
        export M2_HOME=/opt/maven
        export PATH=${M2_HOME}/bin:${PATH}

### Then confirm the PATH by this command :
    . ~/.bash_profile
    echo $PATH
    
