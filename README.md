Jenkins & Docker & Terraform  Installation:

#  Jenkins Installation

# Update and install Java 17 or 21 (Required for Jenkins)

sudo dnf update -y

sudo dnf install java-17-amazon-corretto -y

# Add Jenkins Repository

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# Install and Start Jenkins

sudo dnf install jenkins -y

sudo systemctl enable jenkins

sudo systemctl start jenkins

#  Docker & Terraform Installation

sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo

sudo yum install yum-utils awscli unzip maven git tree docker terraform -y

sudo systemctl start docker

sudo systemctl enable docker

sudo systemctl enable jenkins

# pipeline docker sock issue fix

sudo chmod 666 /var/run/docker.sock
