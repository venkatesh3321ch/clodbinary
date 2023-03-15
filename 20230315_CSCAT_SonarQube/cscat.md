#### CSCAT SonarQube


#### Provision a Linux Machine - Ubuntu

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