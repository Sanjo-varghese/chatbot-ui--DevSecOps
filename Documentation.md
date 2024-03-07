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

 ``sh
 vi jenkins.sh
 ``

 ``sh
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
``
