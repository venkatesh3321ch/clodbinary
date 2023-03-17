
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