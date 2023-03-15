#!/bin/bash


# Update RHEL/CENTOS/AmazonLinux/Oracle Operating System Repository

sudo yum update -y


# Upgrade RHEL Server

sudo yum upgrade -y


# Install required utility softwares

sudo yum install git curl wget unzip elinks tree -y


wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm


sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm


sudo yum update -y


# Install MySQL server.

sudo yum install mysql-server


# Start MySQL service.

sudo systemctl start mysqld

sudo systemctl enable mysqld

sudo systemctl status mysqld


# All the other prompts are self-explanatory.

sudo mysql_secure_installation


mysql -u root -p


SHOW GLOBAL VARIABLES LIKE 'storage_engine';


Create a sonarqube user and a database from MySQL CLI.


CREATE USER 'sonarqube'@'localhost' IDENTIFIED BY 'password';


CREATE DATABASE sonarqube;


Grant all privileges on sonarqube database to the sonarqube user.


GRANT ALL PRIVILEGES ON sonarqube.* TO 'sonarqube'@'localhost';


Open /etc/my.cnf file and under [mysqld] section add the query cache parameter.

The minimum size should be 15 MB. You can increase the size based on your server type.


vi /etc/my.cnf

query_cache_size = 15M


Setup Sonarcube Web Server


Download the latest sonarqube installation file to /opt folder. You can get the latest download link from here. http://www.sonarqube.org/downloads/


# cd /opt


# wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.5.zip

# sudo wget https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-6.0.zip


Install unzip and java.


# sudo yum install unzip -y

# sudo yum install java-1.8.0-openjdk -y


Unzip sonarqube source files and rename the folder.


# sudo unzip sonarqube-6.0.zip

# mv sonarqube-6.0 sonarqube


Open /opt/sonarqube/conf/sonar.properties, uncomment and edit the parameters as shown below. Change the password accordingly.


# vi /opt/sonarqube/conf/sonar.properties

sonar.jdbc.username=sonarqube

sonar.jdbc.password=password

sonar.jdbc.url=jdbc:mysql://localhost:3306/sonarqube?useUnicode=true&amp;characterEncoding=utf8&amp;rewriteBatchedStatements=true&amp;useConfigs=maxPerformance


By default, sonar will run on 9000.


If you want on port 80 or any other port,change the following parameters for accessing the web console on that specific port.


sonar.web.host=0.0.0.0

sonar.web.port=80



If you want to access sonarqube some path like http://url:/sonar, change the following parameter.


# sonar.web.context=/sonar


Start Sonarqube Service

To start sonar service, you need to use the script in sonarqube bin directory.

1. Navigate to the start script directory.



# cd /opt/sonarqube/bin/linux-x86-64


2. Start the sonarqube service.


# sudo ./sonar.sh start


Check the application status. If it is in running state, you can access the sonarqube dashboard using the DNS name or Ip address of your server.


# sudo ./sonar.sh status


Setting up Sonarcube as a service:


Create a file /etc/init.d/sonar and copy the following content on to the file.


#!/bin/sh

#

# rc file for SonarQube

#

# chkconfig: 345 96 10

# description: SonarQube system (www.sonarsource.org)

#

### BEGIN INIT INFO

# Provides: sonar

# Required-Start: $network

# Required-Stop: $network

# Default-Start: 3 4 5

# Default-Stop: 0 1 2 6

# Short-Description: SonarQube system (www.sonarsource.org)

# Description: SonarQube system (www.sonarsource.org)

### END INIT INFO

/usr/bin/sonar $*



Now, create a symbolic link for /usr/bin/sonar with out sonarqube start scripts in the source file directory. i.e /opt/sonarqube/bin/linux-x86-64


# sudo ln -s /opt/sonarqube/bin/linux-x86-64/sonar.sh /usr/bin/sonar


Change the file permissions and add sonar to the boot.


# sudo chmod 755 /etc/init.d/sonar

# sudo chkconfig --add sonar


Once you are done with all the above configurations, you can manage sonar using the following commands.


# sudo service sonar start

# sudo service sonar stop

# sudo service sonar restart