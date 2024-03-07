![Devops](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/3ebf713b-48d9-4c2d-a890-db37e7e2e798)

**Special thanks to McKay Wrigley and, the owner of Open Source, for his dedication to fostering innovation and collaboration in the open-source community.**

**We are grateful for his contributions to the development of technology and for making projects like ChatBOT UI possible.**

**Special Thanks to Ajay  Ajay yegireddi**

**ChatBOT is an AI language model trained on vast amounts of human conversation data. It’s capable of generating human-like text responses based on the input it receives. From answering questions to engaging in conversation, ChatBOT can simulate natural language interactions with users, making it an invaluable tool for enhancing user engagement.**

# Prerequisite:.
**Github  Repo Link** :https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps.git

**Launch an AWS T2 Large Instance. Use the image as Ubuntu. You can create a new key pair or use an existing one. Enable HTTP and HTTPS settings in the Security Group and open all ports (not best case to open all ports but just for learning purposes it’s okay).**

![image](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/407f99bc-fe80-48fa-bb28-c2db144c0822)

**Create IAM role**

![1 part](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/8b3190b2-48c4-4f4a-988a-1224a864e7c0)

**Select entity type as AWS service Use case as EC2 and click on Next**

![part2](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/25cd49c2-b8a2-4322-843b-5a87c0638c6e)

**For permission policy select Administrator Access (Just for learning purpose), click Next.**
![part 3](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/a07b6ba4-5a93-479b-9082-fb317cce1e76)

**Provide a Name for Role and click on Create role,you can use any name for role.**

![part 4](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/21ed751b-bbe1-406b-8955-2816db3aaeb7)

**Role is created.**

![part 5](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/4fc1d641-af56-4744-8429-90d5fbe7ea86)

**Now Attach this role to Ec2 instance that we created earlier, so we can provision cluster from that instance.**

**Go to EC2 Dashboard and select the instance.**

**Click on Actions –> Security –> Modify IAM role.**

![part 6](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/f8ea6205-bd01-4d12-a6b3-f7b78d8703c2)

**Select the Role that created earlier and click on Update IAM role.**
![part 7](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/0a921d3d-9814-4995-996d-bd42c900e961)

**Connect the instance to Mobaxtreme or Putty**

- Install Jenkins, Docker and Trivy
- To Install Jenkins
- Connect to your console, and enter these commands to Install Jenkins

 ```sh
 vi jenkins.sh
 ```

 ```sh
 #!/bin/bash
sudo apt update -y
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
sudo apt update -y
sudo apt install temurin-17-jdk -y
/usr/bin/java --version
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
                  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
                  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
                              /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
sudo systemctl start jenkins
```
```sh
sudo chmod 777 jenkins.sh
sudo su   #move into root and run
./jenkins.sh    # this will installl jenkins
```

**Once Jenkins is installed, you will need to go to your AWS EC2 Security Group and open Inbound Port 8080, since Jenkins works on Port 8080.**

**Now, grab your Public IP Address**

```sh
<EC2 Public IP Address:8080>
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![part 8](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/ea7c536d-5980-43c0-b663-122d92037e1f)

**Unlock Jenkins using an administrative password and install the suggested plugins.**

![part 9](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/91ce3c02-187f-494f-a215-2ac2cee705b4)

**Jenkins will now get installed and install all the libraries.**

![part 10](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/780c7089-f0c9-4ebc-b2d0-1c7b11dd7bdd)

**Create a user click on save and continue.**

**Jenkins Getting Started Screen.**

![part 11](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/58cd9b91-e4ce-498e-89ff-4ef394e2fbb0)

**Install Docker**
```sh
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER   #my case is ubuntu
newgrp docker
sudo chmod 777 /var/run/docker.sock
```

**After the docker installation, we create a sonarqube container (Remember to add 9000 ports in the security group).**

```sh
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```

![part 12](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/0b76102a-dc53-489a-8efd-a2bc336260a9)

**Now our Sonarqube is up and running**


![part 13](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/b05e6e81-0bfd-416d-817e-47f6ab1f30cf)

**Enter username and password, click on login and change password**
```sh
username admin
password admin
```

![part 14](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/4aa10a13-ea0e-48e5-8ebc-13736e95f327)

**Update New password, This is Sonar Dashboard.**
![part 15](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/100cc850-ea1d-4283-8d22-ac6e06da1749)

**Install Trivy, Kubectl,Terraform**
```sh
vi script.sh
```

```sh
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
# Install Terraform
sudo apt install wget -y
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
# Install kubectl
sudo apt update
sudo apt install curl -y
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt-get install unzip -y
unzip awscliv2.zip
sudo ./aws/install
```

**Give permissions and run script**

```sh
sudo chmod 777 script.sh
./script.sh
```

**Next, we will log in to Jenkins and start to configure our Pipeline in Jenkins**

# Install Plugins like JDK, Sonarqube Scanner, NodeJs, OWASP Dependency Check

# Install Plugin
**Goto Manage Jenkins →Plugins → Available Plugins →**

**Install below plugins**

**Blue ocean**

**1 → Eclipse Temurin Installer**

**2 → SonarQube Scanner**

**3 → NodeJs Plugin**

**4 → Docker**

**5 → Docker commons**

**6 → Docker pipeline**

**7 → Docker API**

**8 → Docker Build step**

**9 → Owasp Dependency Check**

**10 → Kubernetes**

**11 → Kubernetes CLI**

**12 → Kubernetes Client API**

**13 → Kubernetes Pipeline DevOps steps**

# Configure Java and Nodejs in Global Tool Configuration
**Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(19)→ Click on Apply and Save**

![part 17](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/488f23c6-aa2a-48dc-b356-68d0843ae472)

**Grab the Public IP Address of your EC2 Instance, Sonarqube works on Port 9000, so <Public IP>:9000. Goto your Sonarqube Server.**

**Click on Administration → Security → Users → Click on Tokens and Update Token → Give it a name → and click on Generate Token**

![part 18](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/01ed0c69-ed91-409e-a14c-d8155d9c5f65)

**click on update Token**
![part 19](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/38a3e363-2955-42c2-a07c-6f32a8fdee98)

**Create a token with a name and generate**

![part 20](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/f335e308-2a34-43db-9724-093e3c156727)

**copy Token**

**Go to Jenkins Dashboard → Manage Jenkins → Credentials → Add Secret Text. It should look like this**
![part 21](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/108a9ca8-b8fe-495e-bb53-db242ce9ca7e)

**You will this page once you click on create**
![part 22](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/77f3df7e-40b1-4dcb-a9ca-3143a7f3cd22)

**Now, go to Dashboard → Manage Jenkins → System and Add like the below image.**
![part 23](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/2c1e891a-d20c-4d5e-99c1-8d46e3647968)

**Click on Apply and Save**

**The Configure System option is used in Jenkins to configure different server**

**Global Tool Configuration is used to configure different tools that we install using Plugins**

**We will install a sonar scanner in the tools.**

**Manage Jenkins –> Tools –> SonarQube Scanner**
![part 24](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/02d6e487-39b5-4065-8cb6-55d03e00c08a)

**In the Sonarqube Dashboard add a quality gate also**

**Administration–> Configuration–>Webhooks**

![part 25](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/ffcb1877-2602-4471-8b6a-4654ef1ec7c2)
**Click on Create**
![part 26](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/c971a9a6-024b-4a57-957c-495af6450691)

**Add details**
```sh
#in url section of quality gate
<http://jenkins-public-ip:8080>/sonarqube-webhook/
```
![part 27](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/774a7515-a058-470a-9471-6d2fb935fa25)

**To see the report, you can go to Sonarqube Server and go to Projects.**

**First, we configured the Plugin and next, we had to configure the Tool**

**Goto Dashboard → Manage Jenkins → Tools →***
![part 28](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/88fdd811-a8a2-4ced-a346-4432a51463a6)

**Click on Apply and Save here.**

**Now, goto Dashboard → Manage Jenkins → Tools →**

![part 29](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/7ab2a02c-409e-459f-8dd7-169d911ca224)

**Tools –> Terraform add this**

**In Jenkins**

```sh
terraform
```
![part 30](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/f8eb06dd-6d31-43c2-a8de-680ad991983d)

**Go to manage Jenkins –> Credentials**

**Add DockerHub Username and Password under Global Credentials**
![part 31](https://github.com/Sanjo-varghese/chatbot-ui--DevSecOps/assets/116708794/9009232f-6597-4915-94bd-84fdb32bb2b5)

**Create EKS Cluster from Jenkins**

<div><span style="color: #ff0000">C</span><span style="color: #ff0e00">H</span><span style="color: #ff1c00">A</span><span style="color: #ff2a00">N</span><span style="color: #ff3800">G</span><span style="color: #ff4700">E</span><span style="color: #ff5500"> </span><span style="color: #ff6300">Y</span><span style="color: #ff7100">O</span><span style="color: #ff7f00">U</span><span style="color: #ff8f00">R</span><span style="color: #ff9f00"> </span><span style="color: #ffaf00">S</span><span style="color: #ffbf00">3</span><span style="color: #ffcf00"> </span><span style="color: #ffdf00">B</span><span style="color: #ffef00">U</span><span style="color: #ffff00">C</span><span style="color: #e3ff00">K</span><span style="color: #c6ff00">E</span><span style="color: #aaff00">T</span><span style="color: #8eff00"> </span><span style="color: #71ff00">N</span><span style="color: #55ff00">A</span><span style="color: #39ff00">M</span><span style="color: #1cff00">E</span><span style="color: #00ff00"> </span><span style="color: #00ff20">I</span><span style="color: #00ff40">N</span><span style="color: #00ff60"> </span><span style="color: #00ff80">T</span><span style="color: #00ff9f">H</span><span style="color: #00ffbf">E</span><span style="color: #00ffdf"> </span><span style="color: #00ffff">B</span><span style="color: #00e3ff">A</span><span style="color: #00c6ff">C</span><span style="color: #00aaff">K</span><span style="color: #008eff">E</span><span style="color: #0071ff">N</span><span style="color: #0055ff">D</span><span style="color: #0039ff">.</span><span style="color: #001cff">T</span><span style="color: #0000ff">F</span></div>
 
**Now create a new job for the Eks provision**
