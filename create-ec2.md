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
            1. provider "aws" {
            2.   region = "us-east-1"
            3. }
            4. 
            5. resource "aws_instance" "example" {
            6.   ami                    = "ami-091138d0f0d41ff90"
            7.   instance_type          = "t2.micro"
            8.   user_data              = <<-EOF
            9.               #!/bin/bash
            10.               apt-get update
            11.               apt-get install -y apache2
            12.               sed -i -e 's/80/8080/' /etc/apache2/ports.conf
            13.               echo "Hello World" > /var/www/html/index.html
            14.               systemctl restart apache2
            15.               EOF
            16.   tags = {
            17.     Name = "terraform-learn-state-ec2"
            18.   }
            19. }
            20. 
            21. output "public_ip"{
            22. value = aws_instance.example.public_ip
            23. }
    4. Save the file
    5. Terraform init
    6. Terraform validate
    7. Terraform apply
    8. Check resource is created or not 
    9. Terraform destroy
