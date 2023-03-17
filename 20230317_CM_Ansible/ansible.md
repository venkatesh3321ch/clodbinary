#### Session Video:
    https://s3.amazonaws.com/kesavkummari.aws/8am_AWSDevOps20230207/20230317_CM_Ansible-1/video1957611991.mp4

#### Example Playbooks

    Syntax Check:
        $ ansible-playbook --syntax-check web.yml


    Preview a Playbook:
        $ ansible-playbook --check web.yml


    Execute a Playbook:
        $ ansible-playbook web.yml

#### STEP-1 : Launch 2 EC2 Instance i.e. Ubuntu

aws ec2 run-instances \
--image-id "ami-0557a15b87f6559cf" \ 
--count 2 \
--instance-type t2.micro \
--key-name "us_east_1_keys" \
--security-group-ids "sg-09e7a75b97f33d7f1" \
--subnet-id "subnet-00a07bb8fefdfcfec" \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=8amAnsible},{Key=Environment,Value=dev}]'


```
aws ec2 run-instances --image-id "ami-0557a15b87f6559cf" --count 2 --instance-type t2.micro --key-name "us_east_1_keys" --security-group-ids "sg-09e7a75b97f33d7f1" --subnet-id "subnet-00a07bb8fefdfcfec" --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=8amAnsible},{Key=Environment,Value=dev}]'

```