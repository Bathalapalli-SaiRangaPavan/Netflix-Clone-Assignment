# Project Architecture

![intelligence](https://github.com/user-attachments/assets/303df76b-f36d-4517-a2fa-06b14395dae1)

---

## Objective

Deploy a Netflix clone using a DevSecOps approach, integrating Jenkins for CI/CD with automated security and quality checks, Docker for containerization, and Kubernetes for orchestration. Monitor Jenkins and Kubernetes metrics using Grafana, Prometheus, and Node Exporter to ensure a secure, scalable, and resilient deployment. This project aims to provide a comprehensive guide for secure and automated application deployment.

---

## Project Flow

### 1. Code Push
   - Developers push the latest code to the GitHub repository.

### 2. Jenkins CI/CD Pipeline
   - **Stage 1: Workspace Cleanup**
     - Ensures a clean environment by clearing the Jenkins workspace before each build.
   - **Stage 2: Code Checkout**
     - Retrieves the latest code from the GitHub repository.
   - **Stage 3: Security and Code Quality Scans**
     - **SonarQube Analysis:** Scans the code for vulnerabilities and code quality issues.
     - **Trivy Code Scan:** Conducts a thorough security scan of the codebase.
     - **OWASP Dependency Check:** Checks dependencies for known security vulnerabilities.
   - **Stage 4: Dependency Installation**
     - Installs necessary dependencies for the Node.js application using npm.
   - **Stage 5: Docker Image Build**
     - Builds a Docker image of the application.
   - **Stage 6: Docker Image Push**
     - Pushes the Docker image to DockerHub.
   - **Stage 7: Docker Image Security Scan**
     - Scans the Docker image for vulnerabilities using Aqua Trivy.
   - **Stage 8: Kubernetes Deployment**
     - Deploys the application as a container in a Kubernetes cluster.
   - **Stage 9: Email Notifications**
     - Sends notifications on the build and deployment status (success/failure).

### 3. Monitoring
   - Monitors Jenkins and Kubernetes metrics using Grafana, Prometheus, and Node Exporter, ensuring system health and performance.

### 4. API Key Requirement
   - The project requires an API key for full functionality. Detailed instructions on running the project with or without the API key are provided.

---

# Step-by-Step Implementation Guide
To successfully deploy the Netflix clone application using a DevSecOps approach, follow these steps to set up and configure the necessary tools and services. This guide provides a structured approach to achieve a secure, scalable, and automated deployment.


## Step 1: Launch an Ubuntu (22.04) T2 Large Instance
   - Start an EC2 instance with Ubuntu 22.04 and the T2 Large instance type.

## Step 2: Install Jenkins, Docker, and Trivy
   - Install Jenkins on the instance.
   - Install Docker and Trivy.
   - Create a SonarQube container using Docker.

## Step 3: Create a TMDB API Key
   - Register and generate an API key from The Movie Database (TMDB).

## Step 4: Install Prometheus and Grafana
   - Install Prometheus and Grafana on the server.

## Step 5: Install the Prometheus Plugin
   - Add the Prometheus plugin to Jenkins.
   - Integrate Jenkins with the Prometheus server.

## Step 6: Email Integration with Jenkins
   - Set up email integration in Jenkins and configure the necessary plugins.

## Step 7: Install Jenkins Plugins
   - Install essential plugins: JDK, SonarQube Scanner, Node.js, and OWASP Dependency Check.

## Step 8: Create a Pipeline Project in Jenkins
   - Set up a new pipeline project in Jenkins using a declarative pipeline script.

## Step 9: Install OWASP Dependency Check Plugin
   - Add and configure the OWASP Dependency Check plugin in Jenkins.

## Step 10: Docker Image Build and Push
   - Build the Docker image for your application.
   - Push the Docker image to DockerHub.

## Step 11: Deploy the Image Using Docker
   - Deploy the Docker image on the instance using Docker.

## Step 12: Kubernetes Master and Slave Setup
   - Set up a Kubernetes master and slave nodes on Ubuntu (20.04).

## Step 13: Access the Netflix App on the Browser
   - Open a web browser and navigate to the deployed Netflix clone application.

## Step 14: Terminate the AWS EC2 Instances
   - After verifying everything is working, terminate the AWS EC2 instances.


# Step 1: Launch and Access an Ubuntu 22.04 T2 Large Instance on AWS

## 1. Sign In to AWS Management Console
- Go to the [AWS Management Console](https://aws.amazon.com/console/) and sign in with your credentials.

## 2. Navigate to EC2 Dashboard
- In the AWS Management Console, select **EC2** from the Services menu.

## 3. Launch an Instance
- Click the **Launch Instance** button.

## 4. Choose an Amazon Machine Image (AMI)
- In the **Choose an Amazon Machine Image (AMI)** section, search for **Ubuntu 22.04**.
- Select the Ubuntu 22.04 AMI from the list.

## 5. Choose an Instance Type
- In the **Choose an Instance Type** section, select **t2.large**.
- Click **Next: Configure Instance Details**.

## 6. Configure Instance Details
- Scroll down to the **Advanced Details** section.
- Click **Next: Add Storage**.

## 7. Add Storage
- The default storage might be set to a different size. Click **Add New Volume** if needed.
- Set the **Size** to **30 GiB**.
- Ensure the **Volume Type** is set to **General Purpose SSD (gp3)**.
- Click **Next: Add Tags**.

## 8. Add Tags (Optional)
- Add tags to help identify the instance, then click **Next: Configure Security Group**.

## 9. Configure Security Group
- **Create a new security group** or select an existing one.
- **Add the following rules** for educational or project-specific use:
  - **Type:** HTTP
    - **Protocol:** TCP
    - **Port Range:** 80
    - **Source:** Anywhere (0.0.0.0/0)
  - **Type:** HTTPS
    - **Protocol:** TCP
    - **Port Range:** 443
    - **Source:** Anywhere (0.0.0.0/0)
  - **Type:** Custom TCP Rule
    - **Protocol:** TCP
    - **Port Range:** 8080 (or any specific port your application uses)
    - **Source:** Anywhere (0.0.0.0/0) *(For project-specific use only; adjust as needed for security in organizational or production environments.)*
- Click **Review and Launch**.

## 10. Review and Launch
- Review your settings and click **Launch**.
- You will be prompted to **Select a key pair**:
  - Choose **Create a new key pair** or **Select an existing key pair**.
  - If creating a new key pair, download the key pair file (.pem) and keep it safe.
- Click **Launch Instances**.

## 11. View Your Instance
- Click **View Instances** to go to the EC2 dashboard and see your newly launched instance.

## 12. Download and Install MobaXterm
- If you haven't already, download and install MobaXterm from the [official website](https://mobaxterm.mobatek.net/download.html).

## 13. Obtain Your EC2 Instance Public IP Address
- In the AWS Management Console, go to the **EC2 Dashboard**.
- Select your instance from the list.
- Find the **Public IPv4 address** in the instance details and copy it.

## 14. Open MobaXterm
- Launch MobaXterm on your computer.

## 15. Create a New SSH Session
- Click on the **Session** button in the top-left corner of MobaXterm.

## 16. Configure SSH Session
- In the **Session settings** window, select **SSH**.
- In the **Remote host** field, paste the **Public IPv4 address** of your EC2 instance.
- Ensure the **Specify username** field is set to `ubuntu` (for Ubuntu instances).

## 17. Add Your Key Pair
- Click on the **Advanced SSH settings** tab.
- Check the box for **Use private key**.
- Click the **...** button next to the field and navigate to the location of your **.pem** key file.
- Select your key file and click **Open**.

## 18. Start the Session
- Click **OK** to start the SSH session.
- MobaXterm will use the provided key file to authenticate and log you in to the EC2 instance.

## 19. Verify Connection
- You should now see the command-line interface of your EC2 instance in MobaXterm.
- You can run commands and interact with your instance as needed.

## Step 2: Install Jenkins, Docker, SonarQube and Trivy on Your AWS EC2 Instance

### 1. Switch to Superuser
   - Run the following command to switch to superuser:
     ```bash
     sudo su -
     ```

### 2. Create the Jenkins Installation Script
   - Create a new file for the Jenkins installation script:
     ```bash
     vi jenkins.sh
     ```

### 3. Add the Following Script to `jenkins.sh`
   - Copy and paste the following script into the `jenkins.sh` file:

     ```bash
     #!/bin/bash
     sudo apt update -y
     #sudo apt upgrade -y
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
     sudo systemctl status jenkins
     ```

### 4. Save and Exit
   - Press `Esc` to enter command mode.
   - Type `:wq` and press `Enter` to save and exit `vi`.

### 5. Make the Script Executable
   - Change the script permissions to make it executable:
     ```bash
     sudo chmod 777 jenkins.sh
     ```

### 6. Run the Jenkins Installation Script
   - Execute the script to install Jenkins:
     ```bash
     ./jenkins.sh
     ```

### 7. Obtain the Jenkins Initial Admin Password
   - After Jenkins is installed, the initial admin password is required for the first login.
   - Run the following command to retrieve the password:
     ```bash
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```
   - Note down the password displayed.

### 8. Access Jenkins Web Interface
   - Open your web browser and navigate to:
     ```
     http://<EC2 Public IP Address>:8080
     ```
   - Replace `<EC2 Public IP Address>` with the actual public IP address of your EC2 instance.

### 9. Unlock Jenkins
   - On the Jenkins setup page, you will be prompted to enter the initial admin password.
   - Copy the password obtained from step 7 and paste it into the "Administrator password" field.
   - Click the **Continue** button.

### 10. Customize Jenkins
   - After unlocking Jenkins, you will be prompted to install suggested plugins or select specific plugins to install.
   - It is recommended to click **Install suggested plugins** for a standard setup.

### 11. Create the First Admin User
   - Once the plugins are installed, you will be prompted to create the first admin user.
   - Fill in the details for the new admin user, including Username, Password, Full Name, and Email Address.
   - Click **Save and Continue**.

### 12. Instance Configuration
   - You will be asked to configure the Jenkins URL.
   - The default URL should be correct if you are accessing Jenkins via the public IP. Click **Save and Finish**.

### 13. Jenkins Dashboard
   - You are now logged in and can access the Jenkins dashboard.
   - You can start configuring Jenkins and setting up your pipelines.

### 14. Install Docker
   - Update package index and install Docker:
     ```bash
     sudo apt-get update
     sudo apt-get install docker.io -y
     ```
   - Add your user to the Docker group and apply changes:
     ```bash
     sudo usermod -aG docker $USER   # For Ubuntu
     newgrp docker
     sudo chmod 777 /var/run/docker.sock
     ```
   - Verify Docker installation:
     ```bash
     docker --version
     ```

### 15. Create and Run SonarQube Container
   - Create a SonarQube container. Make sure to allow port 9000 in the security group:
     ```bash
     docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
     ```
   - Access SonarQube via your browser:
     ```
     http://<EC2 Public IP Address>:9000
     ```
   - Replace `<EC2 Public IP Address>` with the actual public IP address of your EC2 instance.

### 16. Log In to SonarQube
   - On the SonarQube login page, use the following credentials:
     - **Username:** `admin`
     - **Password:** `admin`
   - After logging in, you will be prompted to change the default password.
   - Enter a new password and update it. You will then be directed to the SonarQube dashboard.

### 17. Create the Trivy Installation Script
   - Create a new file for the Trivy installation script:
     ```bash
     vi trivy.sh
     ```

### 18. Add the Following Script to `trivy.sh`
   - Copy and paste the following script into the `trivy.sh` file:

     ```bash
     #!/bin/bash
     sudo apt-get install wget apt-transport-https gnupg lsb-release -y
     wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
     echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
     sudo apt-get update
     sudo apt-get install trivy -y
     ```

### 19. Save and Exit
   - Press `Esc` to enter command mode.
   - Type `:wq` and press `Enter` to save and exit `vi`.

### 20. Make the Script Executable
   - Change the script permissions to make it executable:
     ```bash
     sudo chmod +x trivy.sh
     ```

### 21. Run the Trivy Installation Script
   - Execute the script to install Trivy:
     ```bash
     ./trivy.sh
     ```

### 22. Verify Trivy Installation
   - Ensure that Trivy is installed correctly:
     ```bash
     trivy --version
     ```
# Step 3: Create a TMDB API Key

Next, we will create a TMDB API key.

1. **Open TMDB Website**
   - Open a new tab in your browser and search for **TMDB** or go directly to [TMDB's website](https://www.themoviedb.org/).

2. **Log In**
   - Click on the **Login** button located at the top right corner of the page.

3. **Create an Account**
   - If you don't have an account, click on **Sign up here** or **Create an Account**.
   - Follow the instructions to create an account. If you already have an account, simply log in with your credentials.

4. **Access API Settings**
   - Once logged in, click on your profile icon located at the top right corner of the page.
   - From the dropdown menu, click on **Settings**.

5. **Create an API Key**
   - In the settings menu on the left, click on **API**.

6. **Generate API Key**
   - Click on the **Create** button.
   - Select **Developer** for the type of API key.
   - You will be asked to accept the terms and conditions. Read and accept them to proceed.

7. **Provide Basic Details**
   - You may be asked to provide additional details such as your address and country. Fill in these fields as required to complete the API key request.

8. **Copy Your API Key**
   - Once created, your API key will be displayed. Copy this key as you will need it to make API requests.
