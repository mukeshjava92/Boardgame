# Jenkins and SonarQube End-to-End Setup Guide 

This document provides step-by-step instructions for installing Jenkins on Ubuntu, setting up SonarQube using Docker, and adding Trivy installation via the official apt repository.

# Jenkins Installation on Ubuntu

**Prerequisites**

* Ubuntu server (20.04 or later recommended)

* Sudo privileges

**Steps**

1. Update system packages

        sudo apt update && sudo apt upgrade -y

2. Install Java (required for Jenkins)

        sudo apt install openjdk-11-jdk -y
        java -version

3. Add Jenkins repository and key

        sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
           
        echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]"   https://pkg.jenkins.io/debian-stable binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null


5. Install Jenkins

        sudo apt update
        sudo apt install jenkins -y

6. Start and enable Jenkins service

       sudo systemctl start jenkins
       sudo systemctl enable jenkins
       sudo systemctl status jenkins

7. Open firewall port 8080

        sudo ufw allow 8080

**Access Jenkins Web UI**

URL: http://Jenkins-Public-IP:8080

Retrieve initial admin password:

       sudo cat /var/lib/jenkins/secrets/**initialAdminPassword**


**Reference**
https://www.jenkins.io/doc/book/installing/linux/

-----------------------------------------------------------------------------------------------------------------------------------------------------

 # Install Trivy via Official apt Repository

**Steps**

1. Install prerequisites:

       sudo apt install wget apt-transport-https gnupg lsb-release -y

2. Add Trivy GPG key and repository

       wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
       
       echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list

4. Update and install Trivy

       sudo apt update
       sudo apt install trivy -y

5. Verify installation

       trivy --version

**Reference**

https://trivy.dev/docs/latest/getting-started/installation/

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

 
 # SonarQube Setup in Docker

**Prerequisites**

* Docker installed on Ubuntu

* At least 2GB RAM available

**Steps**

1. Install Docker

      sudo apt update
      sudo apt install docker.io -y
      sudo systemctl enable docker
      sudo systemctl start docker

2. Start the SonarQube application in Sonar container 

          docker run -d --name sonarqube \
            -p 9000:9000 \
            --restart unless-stopped \
            sonarqube:lts

**Access SonarQube Web UI**

URL: http://<Sonar-Public-IP>:9000

**Default credentials:**

Username: admin

Password: admin

**(Optional) Run with PostgreSQL for production:**
    
      docker run -d --name postgres \
        -e POSTGRES_USER=sonar \
        -e POSTGRES_PASSWORD=sonar \
        -e POSTGRES_DB=sonar \
        postgres:13

      docker run -d --name sonarqube \
        -p 9000:9000 \
        -e SONARQUBE_JDBC_URL=jdbc:postgresql://postgres:5432/sonar \
        -e SONARQUBE_JDBC_USERNAME=sonar \
        -e SONARQUBE_JDBC_PASSWORD=sonar \
        --link postgres \
        sonarqube:lts
----------------------------------------------------------------------------------------------------------------------------------------------
# End to End Jenkins Pipeline setup 

**Tools**:
Marven: marven3
JDK: jdk-17
docker: latest
SonarQube: 7.3.0

Below are the tools setup in Jenkins:
<img width="574" height="325" alt="image" src="https://github.com/user-attachments/assets/b02ffa5f-c4f2-42f4-a6f7-bd7e342b995b" />




I
