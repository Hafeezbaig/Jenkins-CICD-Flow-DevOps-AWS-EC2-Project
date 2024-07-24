# Jenkins CI Pipeline Setup with EC2, SonarQube, Docker, Maven, and TRIVY

## Project Overview

This project demonstrates a complete CI pipeline setup using Jenkins, deployed on an AWS EC2 instance. The pipeline integrates with SonarQube for code quality analysis, Docker for containerization, Maven for build automation, and TRIVY for Docker image scanning.

## Table of Contents

- [Project Overview](#project-overview)
- [Setup Instructions](#setup-instructions)
  - [Step 1: Create EC2 Instance](#step-1-create-ec2-instance)
  - [Step 2: Connect and Install Tools](#step-2-connect-and-install-tools)
  - [Step 3: Install Jenkins](#step-3-install-jenkins)
  - [Step 4: Change Security Group](#step-4-change-security-group)
  - [Step 5: Access Jenkins Console](#step-5-access-jenkins-console)
  - [Step 6: Retrieve Administrator Password](#step-6-retrieve-administrator-password)
  - [Step 7: Install Suggested Plugins](#step-7-install-suggested-plugins)
  - [Step 8: Create First User](#step-8-create-first-user)
  - [Step 9: Create Pipeline Job](#step-9-create-pipeline-job)
  - [Step 10: Add Pipeline Script as SCM](#step-10-add-pipeline-script-as-scm)
  - [Step 11: Add Required Plugins](#step-11-add-required-plugins)
  - [Step 12: Setup Docker](#step-12-setup-docker)
  - [Step 13: Install and Configure SonarQube](#step-13-install-and-configure-sonarqube)
  - [Step 14: Install Maven](#step-14-install-maven)
  - [Step 15: Install TRIVY](#step-15-install-trivy)
  - [Step 16: Integrate SonarQube with Jenkins](#step-16-integrate-sonarqube-with-jenkins)
  - [Step 17: Add Docker Hub Credentials](#step-17-add-docker-hub-credentials)
  - [Step 18: Add Jenkins Shared Library](#step-18-add-jenkins-shared-library)
  - [Step 19: Verify Pipeline Execution](#step-19-verify-pipeline-execution)

## Setup Instructions

### Step 1: Create EC2 Instance

1. **Launch an EC2 Instance**
   - **Instance Type**: t2.medium
   - **EBS Volume**: 30 GB
   - **Region**: US-EAST-1
   - **Operating System**: Ubuntu

### Step 2: Connect and Install Tools

1. **Connect to EC2**
   - SSH into your EC2 instance.
   - Switch to the root user:
     ```bash
     sudo su
     ```

2. **Install Required Tools**
   - Follow the installation commands from the provided script:
     ```bash
     wget https://github.com/Hafeezbaig/tools_installation_scripts/raw/main/jenkins.sh
     chmod +x jenkins.sh
     ./jenkins.sh
     ```

### Step 3: Install Jenkins

1. **Install Jenkins**
   - Run the installation script for Jenkins:
     ```bash
     wget https://github.com/Hafeezbaig/tools_installation_scripts/raw/main/jenkins.sh
     chmod +x jenkins.sh
     ./jenkins.sh
     ```

### Step 4: Change Security Group

1. **Update Security Group**
   - Allow necessary ports (e.g., HTTP, Jenkins port 8080) in the EC2 security group settings.

### Step 5: Access Jenkins Console

1. **Sign Into Jenkins**
   - Access Jenkins via `http://<EC2_PUBLIC_IP>:8080/`.

### Step 6: Retrieve Administrator Password

1. **Get Admin Password**
   - Run the following command in the EC2 instance:
     ```bash
     cat /var/lib/jenkins/secrets/initialAdminPassword
     ```

### Step 7: Install Suggested Plugins

1. **Install Plugins**
   - Navigate to **Manage Jenkins** -> **Manage Plugins** and install all suggested plugins.

### Step 8: Create First User

1. **Set Up User**
   - Follow the Jenkins setup wizard to create your first user.

### Step 9: Create Pipeline Job

1. **Create Pipeline Job**
   - Create a new Pipeline job in Jenkins.

### Step 10: Add Pipeline Script as SCM

1. **Configure SCM**
   - Use the pipeline script from:
     ```text
     https://github.com/Hafeezbaig/Java_app_3.0.git
     ```

### Step 11: Add Required Plugins

1. **Install Additional Plugins**
   - Navigate to **Manage Jenkins** -> **Manage Plugins** -> **Available Plugins** and install:
     - SonarQube Scanner
     - SonarQube Generic Coverage
     - Sonar Quality Gates
     - Artifactory
     - Jfrog

### Step 12: Setup Docker

1. **Install Docker**
   - Follow the installation script for Docker:
     ```bash
     wget https://github.com/Hafeezbaig/tools_installation_scripts/raw/main/docker.sh
     chmod +x docker.sh
     ./docker.sh
     ```

2. **Verify Docker Installation**
   - Check Docker version:
     ```bash
     docker -v
     ```

### Step 13: Install and Configure SonarQube

1. **Install SonarQube**
   - Follow the installation script for SonarQube:
     ```bash
     wget https://github.com/Hafeezbaig/tools_installation_scripts/raw/main/sonarqube.sh
     chmod +x sonarqube.sh
     ./sonarqube.sh
     ```

2. **Start SonarQube Container**
   - Start the container if it's not running:
     ```bash
     docker ps -a
     docker start <containerID>
     ```

3. **Login to SonarQube Dashboard**
   - **Username**: admin
   - **Password**: admin

4. **Create SonarQube Token**
   - Navigate to **Administration** -> **My Account** -> **Security** -> **Create token**.

5. **Integrate SonarQube with Jenkins**
   - Go to **SonarQube Dashboard** -> **Administration** -> **Configuration** -> **Webhooks**.
   - Add the following URL:
     ```text
     http://<EC2_IP>:8080/sonarqube-webhook/
     ```

### Step 14: Install Maven

1. **Install Maven**
   - Follow the installation script for Maven:
     ```bash
     wget https://github.com/Hafeezbaig/tools_installation_scripts/raw/main/Maven.sh
     chmod +x Maven.sh
     ./Maven.sh
     ```

### Step 15: Install TRIVY

1. **Install TRIVY**
   - Follow the installation script for TRIVY:
     ```bash
     wget https://github.com/Hafeezbaig/tools_installation_scripts/raw/main/trivy.sh
     chmod +x trivy.sh
     ./trivy.sh
     ```

### Step 16: Integrate SonarQube with Jenkins

1. **Add SonarQube Server**
   - Navigate to **Manage Jenkins** -> **Configure System** -> **SonarQube Servers**.
   - Add SonarQube URL and token.

### Step 17: Add Docker Hub Credentials

1. **Add Docker Hub Credentials**
   - Navigate to **Manage Jenkins** -> **Credentials** -> **System** -> **Global credentials**.
   - Add Docker Hub credentials with the ID `docker`.

### Step 18: Add Jenkins Shared Library

1. **Configure Shared Library**
   - Go to **Manage Jenkins** -> **Configure System** -> **Global Pipeline Libraries**.
   - Add the following data:
     - **Name**: my-shared-library
     - **Default Version**: main
     - **Git URL**: 
       ```text
       https://github.com/Hafeezbaig/jenkins_shared_lib.git
       ```

### Step 19: Verify Pipeline Execution

1. **Check Pipeline Execution**
   - Monitor Jenkins logs for pipeline execution details.
   - Review TRIVY scan results for Docker image vulnerabilities.
   - Check the SonarQube dashboard for code quality reports.

## Feel Free to Contribute

Feel free to contribute to the project or use it as a reference for your own development needs. For any issues or suggestions, please open an issue in the [GitHub repository](https://github.com/Hafeezbaig/Gemini-Clone-Project/issues).

## Made By

This project is developed by [Hafeez Baig](https://www.hafeezbaig.in).

## Connect

- [Portfolio](https://www.hafeezbaig.in)
- [Connect](https://connect.hafeezbaig.in)

## Socials

- [GitHub](https://github.com/Hafeezbaig)
- [LinkedIn](https://www.linkedin.com/in/mohammed-abdul-hafeez-baig-52b21b209/)
- [Instagram](https://www.instagram.com/mohammed_hafeez_baig/)

## Feedback or Bug Report

- [Talk](https://talk.hafeezbaig.in)

