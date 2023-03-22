#### Session Video:
    https://s3.amazonaws.com/kesavkummari.aws/8am_AWSDevOps20230207/20230317_CM_Ansible-1/video1957611991.mp4

#### What is Configuration Management?

    - Configuration Management (CM) is a process used to manage, track, and control changes to a system's hardware, software, and documentation throughout its life cycle. 
    
    - It is an important part of a larger system development process and helps to ensure that the system is reliable, consistent, and up to date. 
    
    - It also helps to ensure that all changes made to the system are properly documented, tested, and approved before they are implemented.


#### Different Configuration Management Tools
    
    1. Puppet
    2. Chef
    3. SaltStack
    4. Ansible


#### Difference between PULL and Push based Configuration Management Tools

    - Pull Based Configuration Management Tools:
        - Pull based configuration management tools allow an administrator to define a desired state of a system, then the tool will automatically configure that system to match that desired state. 
        
        - These tools are typically agent-based, meaning they must be installed on each system to be managed. 
        
        - The agent will periodically check in with the management server to check for any desired changes and then apply them.

    - Push Based Configuration Management Tools:
        - Push based configuration management tools allow an administrator to define a desired state of a system, then the tool will automatically push the configurations to the systems in order to match the desired state. 
        
        - These tools don't require agents to be installed on each system, instead they will push the configurations directly to each system. 
        
        - This makes them easier to use for large networks where installing agents on each system might be difficult.

#### What is Ansible?

    - Ansible is an open source automation platform used for configuration management, application deployment, and task automation. 
    
    - It is designed to help users automate routine tasks so that they can spend more time on complex and important activities. 
    
    - Ansible uses an agentless architecture, which means there is no need to install any software on the managed hosts. 
    
    - This allows Ansible to be used for managing both on-premises and cloud-based systems.


#### Here's a table comparing Puppet, Chef, Saltstack, and Ansible based on their table output features:

| Feature | Puppet | Chef | Saltstack | Ansible |
|---------|--------|------|-----------|---------|
| Declarative	| Yes | Yes | Yes | Yes |
| Imperative	| No | Yes | Yes | Yes |
| Agentless	| No	| No | Yes | Yes |
| Agent-based	| Yes | Yes | No | No |
| Configuration as Code	| Yes | Yes | Yes | Yes |
| Push-based	| No | No | Yes | Yes |
| Pull-based	| Yes | Yes | No	| No |
| Idempotent	| Yes | Yes | Yes | Yes |
| Easy to Learn	| No | No | No | Yes |
| Community Support	| Yes | Yes | Yes |	Yes |


    - In summary, all four tools have table output features, but there are some differences in their approach and functionality. 
    
    - Puppet, Chef, Saltstack, and Ansible all support declarative configuration, with Chef and Saltstack also supporting imperative configuration. 
    
    - Saltstack and Ansible are agentless, while Puppet and Chef require an agent to be installed on the target hosts. 
    
    - All four tools support configuration as code and push-based deployment, but only Saltstack supports pull-based deployment. 
    
    - All four tools are idempotent and have strong community support, but Ansible is considered easier to learn compared to Puppet, Chef, and Saltstack.

#### Key Things Of Ansible
    1. Ansible is an open-source automation platform that enables users to configure systems, deploy applications, and orchestrate more advanced IT tasks such as continuous deployments or zero downtime rolling updates.

    2. Ansible uses an easy-to-learn language called YAML to write playbooks, which are declarative files that define the desired state of a system.

    3. Ansible Tower is the commercial version of Ansible, which provides a web-based interface, REST API, and CLIs for managing and monitoring Ansible tasks.

    4. Ansible can be used to configure and manage servers, network devices, and cloud services, as well as to deploy applications and orchestrate complex IT workflows.

    5. Ansible is agentless, meaning it does not require any additional software to be installed on the managed nodes.

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