# AWS-Ansible-Wordpress
This project will create and deploy the necessary AWS architecture for an Ansible and Wordpress server. 

## Description
Creates a VPC to to host a Wordpress website, launches a subnet with 2 servers, Ansible and a Wordpress server. Deploys an Internet Gateway, a security group with Inbound & Outbound rules and a route table. Installs the latest version of Wordpress with an Ansible playbook. Assigns an Elastic IP to the Wordpress EC2 instance.

## Auto-deployment-Steps (AWS-CloudFormation):

Prerequisite:
- Create Keys through the AWS Console.
 
1. Launch template with Keys.
2. ssh into ansible host instance and add keys to home directory.
3. run the following commands: 
	
        chmod 400 ~/.ssh/your_keys
        cd ansible-ec2-wordpress
  
4. Run the playbook:

       ansible-playbook --private-key="~/.ssh/your_keys” site.yml

5. Visit wordpress site:

       ec2-“your-IP”.compute-1.amazonaws.com

## Manual-deployment-Steps:

Prerequisite:
- Create Keys through the AWS Console.

1. Create VPC.
2. Create Public Subnet & Attach Internet Gateway.
3. Create an Ansible Instance - Amazon Linux (ami-97785bed). 
4. Create Security Group with access to Port 80 & Port 22.
5. Configure Ansible instance with the following commands:

                yum-config-manager --enable epel,
                yum -y install ansible,
                yum -y install git,
                chmod go+w /etc/ansible/hosts,
                git clone https://github.com/hamza15/AWS-Ansible-Wordpress.git /home/ec2-user/ansible-ec2-wordpress,
                echo 'localhost ansible_python_interpreter=\"/usr/bin/env python\"' >> /etc/ansible/hosts
                echo '[wordpresshost]' >> /etc/ansible/hosts
                echo $WORDPRESSHOST >> /etc/ansible/hosts
                
6. Launch a Wordpress Instance - Ubuntu (ami-0ce3bb76).
7. Attach an Elastic IP to Wordpress Instance.
8. Confirm python is installed on Wordpress Instance.
9. Add the keys used to deploy Wordpress Instance, to the Ansible Instance and change permissions using the following command:
				
        chmod 400 ~/your_keys
        
10. Inside Ansible Instance add the hostname for wordpress instance using the following commands:
				
        sudo nano /etc/ansible/host
        Under [wordpresshost] add hostname.
        
11. To deploy wordpress using the playbook, change directory to the ansible directory and run the following commands:
				
        ansible-playbook --private-key="~/your_keys" site.yml
        
12. Visit wordpress site:

        ec2-“your-IP”.compute-1.amazonaws.com        
