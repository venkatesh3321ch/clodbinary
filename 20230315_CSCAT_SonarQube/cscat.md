#### CSCAT SonarQube

    SonarQube is an open-source platform for continuous inspection of code quality. 
    
    It provides a range of tools to analyze and manage code quality, technical debt, and code coverage. 
    
    The platform is used by developers and quality assurance teams to improve the overall quality of software applications.

    SonarQube performs static code analysis and identifies potential bugs, code smells, and security vulnerabilities. 

    It supports a wide range of programming languages, including Java, JavaScript, Python, C#, C/C++, Ruby, and more. 
    
    Additionally, it integrates with popular build tools and IDEs, such as Jenkins, Maven, and Visual Studio.

    The platform provides detailed reports and dashboards, making it easy to track code quality metrics over time. 
    
    These metrics can help teams identify trends, track progress, and prioritize areas for improvement. 
    
    SonarQube can also be integrated with other tools, such as issue trackers, to automate the process of tracking and fixing code quality issues.

    Overall, SonarQube is a powerful tool for improving the quality of software applications, helping teams catch and fix issues early in the development cycle.

#### Provision a Linux Machine - Ubuntu

#### Prerequisites:
    
    - Ubuntu 22.04 LTS with minimum 2GB RAM and 1 CPU.
    
    - PostgreSQL Version 9.3 or higher
    
    - SSH access with sudo privileges
    
    - Firewall Port: 9000
    
# Canonical, Ubuntu, 22.04 LTS, amd64 jammy image build on 2023-02-08 | ami-0557a15b87f6559cf

$ aws ec2 run-instances \
--image-id "ami-0557a15b87f6559cf" \
--count 1 \
--instance-type t2.micro \
--key-name "us_east_1_keys" \
--security-group-ids "sg-09e7a75b97f33d7f1" \
--subnet-id "subnet-00a07bb8fefdfcfec" \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=8am-Sonarqube},{Key=Environment,Value=dev}]'



#### Hands-On :

    - Install Docker on Ubuntu Server

        $ sudo apt-get update 

        # Setup Hostname
        sudo hostnamectl set-hostname "sonarqube.cloudbinary.io"

        # Update the hostname part of Host File
        echo "`hostname -I | awk '{ print $1 }'` `hostname`" >> /etc/hosts

        # Update Ubuntu Repository
        sudo apt-get update

        # Download, & Install Utility Softwares
        sudo apt-get install git wget unzip zip curl tree -y

        $ sudo apt-get install docker.io -y


    - Download a Docker Image of Sonarqube from hub.docker.com

        $ sudo docker pull sonarqube


    - check Downloaded Docker Image of Sonarqube

        $ sudo docker images

    - Create Docker volumes to store the SonarQube persistent data.

        $ docker volume create sonarqube-conf

        $ docker volume create sonarqube-data

        $ docker volume create sonarqube-logs

        $ docker volume create sonarqube-extensions


    - Verify the persistent data directories.

        $ docker volume inspect sonarqube-conf

        $ docker volume inspect sonarqube-data

        $ docker volume inspect sonarqube-logs

        $ docker volume inspect sonarqube-extensions


    - Optionally, create symbolic links to an easier access location.


        $ mkdir /sonarqube


        $ ln -s /var/lib/docker/volumes/sonarqube-conf/_data /sonarqube/conf

        $ ln -s /var/lib/docker/volumes/sonarqube-data/_data /sonarqube/data

        $ ln -s /var/lib/docker/volumes/sonarqube-logs/_data /sonarqube/logs

        $ ln -s /var/lib/docker/volumes/sonarqube-extensions/_data /sonarqube/extensions


    - Start a SonarQube container with persistent data storage.

        $ docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 -v sonarqube-conf:/sonarqube/conf -v sonarqube-data:/sonarqube/data -v sonarqube-logs:/sonarqube/logs -v sonarqube-extensions:/sonarqube/extensions sonarqube

        
#### To Allow Normal User to Access Docker Images
    
    STEP-1: 
        $  sudo groupadd docker
    
    STEP-2:
        $ sudo usermod -aG docker $USER
    
    STEP-3:
        $ sudo chown root:docker /var/run/docker.sock

    Note: OPTIONAL = $ sudo setfacl -m user:$USER:rw /var/run/docker.sock