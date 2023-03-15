#### Prerequisites:
    
    - Ubuntu 22.04 LTS with minimum 2GB RAM and 1 CPU.
    
    - PostgreSQL Version 9.3 or higher
    
    - SSH access with sudo privileges
    
    - Firewall Port: 9000


#### Hands-On :
    - Provision Linux i.e. Ubuntu:
    - Canonical, Ubuntu, 22.04 LTS, amd64 jammy image build on 2023-02-08 | ami-0557a15b87f6559cf

```
$ aws ec2 run-instances \
--image-id "ami-0557a15b87f6559cf" \
--count 1 \
--instance-type t2.medium \
--key-name "us_east_1_keys" \
--security-group-ids "sg-09e7a75b97f33d7f1" \
--subnet-id "subnet-00a07bb8fefdfcfec" \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=SonarQube},{Key=Environment,Value=dev}]'
```

    # Debian/Ubutu
    #!/bin/bash

    # Setup Hostname
    sudo hostnamectl set-hostname "sonarqube.cloudbinary.io"

    # Update the hostname part of Host File
    echo "`hostname -I | awk '{ print $1 }'` `hostname`" >> /etc/hosts

    # Update Ubuntu Repository
    sudo apt-get update

    # Download, & Install Utility Softwares
    sudo apt-get install git wget unzip zip curl tree -y

    # Download, Install Java 11
    sudo apt-get install openjdk-17-jdk -y

    # Backup the Environment File
    sudo cp -pvr /etc/environment "/etc/environment_$(date +%F_%R)"

    # Create Environment Variables
    echo "JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64/" >> /etc/environment

    # Install and Configure PostgreSQL
    # Add the PostgreSQL repository.
    sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

    # Add the PostgreSQL signing key.
    wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

    # Install PostgreSQL.
    sudo apt install postgresql postgresql-contrib -y

    # Enable the database server to start automatically on reboot.
    sudo systemctl enable postgresql

    # Start the database server.
    sudo systemctl start postgresql

    # Change the default PostgreSQL password.
    sudo passwd postgres

    Note: PostgreSQL

    Switch to the postgres user.
    su - postgres

    # Create a user named sonar.
    createuser sonar

    Log in to PostgreSQL.
    $ psql

    # Set a password for the sonar user. Use a strong password in place of my_strong_password.

    ALTER USER sonar WITH ENCRYPTED password 'PostgreSQL123';

    # Create a sonarqube database and set the owner to sonar.

    CREATE DATABASE sonarqube OWNER sonar;

    # Grant all the privileges on the sonarqube database to the sonar user.

    GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;

    # Exit PostgreSQL.
    \q

    # Return to your non-root sudo user account.

    $ exit

    # Download and Install SonarQube
    
    # Install the zip utility, which is needed to unzip the SonarQube files.

    $ sudo apt-get install zip -y

    Locate the latest download URL from the SonarQube official download page.
        https://www.sonarqube.org/downloads/
    
    # Download the SonarQube distribution files.

    $ sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip

    # Unzip the downloaded file.
    sudo unzip sonarqube-9.9.0.65466.zip

    # Move the unzipped files to /opt/sonarqube directory
    sudo mv sonarqube-9.9.0.65466.zip /opt/sonarqube
    
    # Add SonarQube Group and User
    
    # Create a dedicated user and group for SonarQube, which can not run as the root user.

    # Create a sonar group.

    $ sudo groupadd sonar
    
    # Create a sonar user and set /opt/sonarqube as the home directory.

    $ sudo useradd -d /opt/sonarqube -g sonar sonar

    # Grant the sonar user access to the /opt/sonarqube directory.
    
    $ sudo chown sonar:sonar /opt/sonarqube -R


    # Configure SonarQube
    
    # Edit the SonarQube configuration file.
    
    $ sudo vi /opt/sonarqube/conf/sonar.properties

    Find the following lines:

    #sonar.jdbc.username=
    #sonar.jdbc.password=

    # Uncomment the lines, and add the database user and password you created in Step 2.

    sonar.jdbc.username=sonar

    sonar.jdbc.password=PostgreSQL123

    # Below those two lines, add the sonar.jdbc.url.

    sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube

    Save and exit the file.

    # Edit the sonar script file.
    
    $ sudo vi /opt/sonarqube/bin/linux-x86-64/sonar.sh

    About 50 lines down, locate this line:

    #RUN_AS_USER=
    
    Uncomment the line and change it to:
    
    RUN_AS_USER=sonar

    Save and exit the file.

    # Setup Systemd service
    
    # Create a systemd service file to start SonarQube at system boot.

    $ sudo vi /etc/systemd/system/sonar.service

```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target

```
    - Save and exit the file.

    # Enable the SonarQube service to run at system startup.

    $ sudo systemctl enable sonar

    # Start the SonarQube service.

    $ sudo systemctl start sonar

    # Check the service status.

    $ sudo systemctl status sonar
