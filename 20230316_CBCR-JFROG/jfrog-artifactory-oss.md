#### JFROG 

    - JFrog Artifactory is a powerful and scalable binary repository manager developed by JFrog. 
    
    - It is designed to handle various types of artifacts such as software packages, containers, and binaries used in the development and deployment of applications.

    - Artifactory provides a central location for storing and managing these artifacts, allowing teams to share and reuse them across different projects and environments. 
    
    - It supports a wide range of package formats, including Java (Maven, Gradle, Ivy), npm, Docker, Helm, NuGet, PyPI, and more.

    - Artifactory provides powerful features like security and access control, metadata management, and build integration. 
    
    - It also includes advanced search capabilities, REST APIs, and a user-friendly web interface to help users find, download, and upload artifacts.

    - JFrog Artifactory is a critical component of modern software development and deployment pipelines, enabling teams to accelerate their software development process and deliver high-quality software faster.

# Hands-On: Setup Self-Hosted JFrog Artifactory For CI/CD Pipeline
    
#### Provision Linux i.e. Ubuntu/Amazon Linux2/CentOS/Redhat Linux Distributions

```
$ aws ec2 run-instances \

# Ubuntu
--image-id "ami-0557a15b87f6559cf" \ 

# Amazon Linux2/CentOS/Redhat Linux Distributions
# --image-id "ami-005f9685cb30f234b" \ 

--count 1 \
--instance-type t2.micro \
--key-name "us_east_1_keys" \
--security-group-ids "sg-09e7a75b97f33d7f1" \
--subnet-id "subnet-00a07bb8fefdfcfec" \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=Tomcat},{Key=Environment,Value=dev}]'
 
```

``` 
    # Below Script is For Ubuntu & Amazon Linux2/CentOS/Redhat Linux Distributions:

    #!/bin/bash

    # Setup Hostname
    sudo hostnamectl set-hostname "jfrog.cloudbinary.io"

    # Configure Hostname unto hosts file
    echo "`hostname -I | awk '{ print $1}'` `hostname`" >> /etc/hosts

    # Update the Repository 
    # sudo apt-get update.  ---> Ubuntu Linux Distributions
    sudo yum update

    # Install required utility softwares
    # sudo apt-get install vim curl elinks unzip wget tree git -y ---> Ubuntu Linux Distributions
    sudo yum install vim curl elinks unzip wget tree git -y

    # Download, Install Java 17
    # sudo apt-get install openjdk-17-jdk -y ---> Ubuntu Linux Distributions
    sudo yum install java-17-amazon-corretto-devel -y 

    # Backup the Environment File
    sudo cp -pvr /etc/environment "/etc/environment_$(date +%F_%R)"

    # Create Environment Variables
    echo "JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64/" >> /etc/environment
    # echo "JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64/" >> /etc/environment ---> Ubuntu Linux Distributions

    # Compile the Configuration
    source /etc/environment

    # Download JFROG Software 
    sudo wget  https://releases.jfrog.io/artifactory/bintray-artifactory/org/artifactory/oss/jfrog-artifactory-oss/[RELEASE]/jfrog-artifactory-oss-[RELEASE]-linux.tar.gz  

    # Extract the Tar File
    tar xvzf jfrog-artifactory-oss-\[RELEASE\]-linux.tar.gz 

    # Rename the Directory 
    mv artifactory-oss-7.55.6/ jfrog
    
    # Backup the Environment File
    sudo cp -pvr /etc/environment "/etc/environment_$(date +%F_%R)"

    # Create Environment Variables
    echo "JFROG_HOME=/opt/jfrog" >> /etc/environment

    # To start JFROG Artifactory, Go to Bin 
    cd /opt/jfrog/app/bin/ 

    # Start JFROG Artifactory
    ./artifactory.sh status
    ./artifactory.sh start

    # Take the PublicIP of EC2 Instance and Login
    http://<public_ip_of_ec2_instance>:8081

    # Default UserName & Passwords are:
        # UserName : admin ; Password : password

    # Configure INIT Scripts for JFrog Artifactory

    # sudo vi /etc/systemd/system/artifactory.service

    ```
    [Unit]
    Description=JFrog artifactory service
    After=syslog.target network.target

    [Service]
    Type=forking

    ExecStart=/opt/jfrog/app/bin/artifactory.sh start
    ExecStop=/opt/jfrog/app/bin/artifactory.sh stop

    User=root
    Group=root 
    Restart=always

    [Install]
    WantedBy=multi-user.target

    ```

    # Start JFrog Service
    # sudo systemctl status artifactory.service

    # sudo systemctl stop artifactory.service
    
    # sudo systemctl start artifactory.service
    
    # sudo systemctl restart artifactory.service
    
    # sudo systemctl enable artifactory.service    

```
