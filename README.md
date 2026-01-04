# **Jenkins and SonarQube End-to-End Setup Guide **

This document provides step-by-step instructions for installing Jenkins on Ubuntu, setting up SonarQube using Docker, and adding Trivy installation via the official apt repository.

# **Jenkins Installation on Ubuntu**

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

    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

4. Install Jenkins

    sudo apt update
    sudo apt install jenkins -y

5. Start and enable Jenkins service

   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   sudo systemctl status jenkins

6. Open firewall port 8080

    sudo ufw allow 8080

**Access Jenkins Web UI**

URL: **http://<Jenkins-Public-IP>:8080**

Retrieve initial admin password:

   sudo cat /var/lib/jenkins/secrets/**initialAdminPassword**


**Reference**

Jenkins Official Installation Guide
-----------------------------------------------------------------------------------------------------------------------------------------------------

 # **Install Trivy via Official apt Repository**

**Steps**

1. Install prerequisites:

   sudo apt install wget apt-transport-https gnupg lsb-release -y

2. Add Trivy GPG key and repository

   wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
   echo "deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list

3. Update and install Trivy

   sudo apt update
   sudo apt install trivy -y

4. Verify installation

   trivy --version

**Reference**

Trivy Official Installation Guide

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

 
 # **SonarQube Setup in Docker**

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

**Default credentials:

Username: admin

Password: admin**

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

**Reference**

SonarQube Docker Hub

SonarQube Documentation


I
