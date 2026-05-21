create ec2-instance through terraform

 1.log in ec2 ubuntu and update
	sudo apt update -y

2. Install terraform
* wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
* echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
* sudo apt update && sudo apt install terraform

3. Install and Configure aws cli
    1. sudo apt install awscli -y
    2. aws configure 
        1. Give access key, sec key, region, format
    3. Aws configure list
4. Create ec2 through terraform
    1. Create a directory , goto directory
    2. Vi main.tf
    3. Task: create ec2 instance with us-east-1 region
       
             provider "aws" {
               region = "us-east-1"
             }
        
             resource "aws_instance" "example" {
               ami                    = "ami-091138d0f0d41ff90"
               instance_type          = "t2.micro"
             
              tags = {
                 Name = "terraform-learn-state-ec2"
               }
             }
       
             output "public_ip"{
             value = aws_instance.example.public_ip
             }

       
    5. Save the file
    6. Terraform init
    7. Terraform validate
    8. Terraform apply
    9. Check resource is created or not  if created
    10. Terraform destroy : will destroy the resource
