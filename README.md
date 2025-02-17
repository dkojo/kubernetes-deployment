# itgenius-app-kubernetes

#User data to install pkgs on ec2 instance

#!/bin/bash
   # Update packages and install required software

   # Install Docker and configure permissions
   sudo yum update -y
   sudo yum install docker -y
   docker --version
   sudo systemctl start docker
   sudo systemctl enable docker
   sudo chmod 770 /var/run/docker.sock

   # Install Terraform
   sudo wget https://releases.hashicorp.com/terraform/1.3.7/terraform_1.3.7_linux_amd64.zip
   sudo unzip terraform_1.3.7_linux_amd64.zip
   sudo mv terraform /usr/local/bin
   terraform -v

   # Install Git
   sudo yum install git -y
   git --version

#Install mysql client
sudo dnf install mariadb105

   # Install AWS CLI
   sudo curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   sudo unzip awscliv2.zip
   sudo ./aws/install
   aws --version

   # Install Jenkins
   # Check documentation: https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/
   sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
   sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
   sudo yum upgrade
   sudo yum install java-17-amazon-corretto -y
   sudo yum install jenkins -y
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   sudo usermod -aG docker jenkins

   # Install eksctl
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin

   # Install kubectl
   curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.30.2/2024-07-12/bin/linux/amd64/kubectl
   chmod +x ./kubectl
   mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
   echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
   sudo mv ./kubectl /usr/local/bin/kubectl
