# Project Architecture

![intelligence](https://github.com/user-attachments/assets/303df76b-f36d-4517-a2fa-06b14395dae1)

---

## Objective

Deploy a Netflix clone using a DevSecOps approach on AWS Cloud, integrating Jenkins for CI/CD with automated security and quality checks, Docker for containerization, and monitoring Jenkins and application metrics using Grafana, Prometheus, and Node Exporter to ensure a secure, scalable, and resilient deployment. This project aims to provide a comprehensive guide for secure and automated application deployment.

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
   - **Stage 8: Deploymet**
     - Deploys the application as a container.
   - **Stage 9: Email Notifications**
     - Sends notifications on the build and deployment status (success/failure).

### 3. Monitoring
   - Monitors Jenkins and Kubernetes metrics using Grafana, Prometheus, and Node Exporter, ensuring system health and performance.

### 4. API Key Requirement
   - The project requires an API key for full functionality. Detailed instructions on running the project with or without the API key are provided.

---

## Step-by-Step Implementation Guide
To successfully deploy the Netflix clone application using a DevSecOps approach, follow these steps to set up and configure the necessary tools and services. This guide provides a structured approach to achieve a secure, scalable, and automated deployment.


### Step 1: Launch an Ubuntu (22.04) T2 Large Instance
   - Start an EC2 instance with Ubuntu 22.04 and the T2 Large instance type.

### Step 2: Install Jenkins, Docker, and Trivy
   - Install Jenkins on the instance.
   - Install Docker and Trivy.
   - Create a SonarQube container using Docker.

### Step 3: Create a TMDB API Key
   - Register and generate an API key from The Movie Database (TMDB).

### Step 4: Install Prometheus and Grafana
   - Install Prometheus and Grafana on the server.

### Step 5: Install the Prometheus Plugin
   - Add the Prometheus plugin to Jenkins.
   - Integrate Jenkins with the Prometheus server.

### Step 6: Email Integration with Jenkins
   - Set up email integration in Jenkins and configure the necessary plugins.

### Step 7: Install Jenkins Plugins
   - Install essential plugins: JDK, SonarQube Scanner, Node.js, and OWASP Dependency Check.

### Step 8: Create a Pipeline Project in Jenkins
   - Set up a new pipeline project in Jenkins using a declarative pipeline script.

### Step 9: Install OWASP Dependency Check Plugin
   - Add and configure the OWASP Dependency Check plugin in Jenkins.

### Step 10: Dockerfile, Build, push, and run a Docker image as a container.
   - Build the Docker image for your application.
   - Push the Docker image to DockerHub.
   - Deploy the Docker image on the instance using Docker.
 
### Step 11: Access the Netflix App on the Browser
   - Open a web browser and navigate to the deployed Netflix clone application.


## Step 1: Launch and Access an Ubuntu 22.04 T2 Large Instance on AWS

### 1. Sign In to AWS Management Console
- Go to the [AWS Management Console](https://aws.amazon.com/console/) and sign in with your credentials.
![Screenshot (167)](https://github.com/user-attachments/assets/36c55110-6511-4f4b-8fa8-8be694a5dd4e)

### 2. Navigate to EC2 Dashboard
- In the AWS Management Console, select **EC2** from the Services menu.

### 3. Launch an Instance
- Click the **Launch Instance** button.

### 4. Choose an Amazon Machine Image (AMI)
- In the **Choose an Amazon Machine Image (AMI)** section, search for **Ubuntu 22.04**.
- Select the Ubuntu 22.04 AMI from the list.

### 5. Choose an Instance Type
- In the **Choose an Instance Type** section, select **t2.large**.
- Click **Next: Configure Instance Details**.

### 6. Configure Instance Details
- Scroll down to the **Advanced Details** section.
- Click **Next: Add Storage**.

### 7. Add Storage
- The default storage might be set to a different size. Click **Add New Volume** if needed.
- Set the **Size** to **30 GiB**.
- Ensure the **Volume Type** is set to **General Purpose SSD (gp3)**.
- Click **Next: Add Tags**.

### 8. Add Tags (Optional)
- Add tags to help identify the instance, then click **Next: Configure Security Group**.

### 9. Configure Security Group
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

### 10. Review and Launch
- Review your settings and click **Launch**.
- You will be prompted to **Select a key pair**:
  - Choose **Create a new key pair** or **Select an existing key pair**.
  - If creating a new key pair, download the key pair file (.pem) and keep it safe.
- Click **Launch Instances**.

### 11. View Your Instance
- Click **View Instances** to go to the EC2 dashboard and see your newly launched instance.

### 12. Download and Install MobaXterm
- If you haven't already, download and install MobaXterm from the [official website](https://mobaxterm.mobatek.net/download.html).

### 13. Obtain Your EC2 Instance Public IP Address
- In the AWS Management Console, go to the **EC2 Dashboard**.
- Select your instance from the list.
- Find the **Public IPv4 address** in the instance details and copy it.

### 14. Open MobaXterm
- Launch MobaXterm on your computer.

### 15. Create a New SSH Session
- Click on the **Session** button in the top-left corner of MobaXterm.

### 16. Configure SSH Session
- In the **Session settings** window, select **SSH**.
- In the **Remote host** field, paste the **Public IPv4 address** of your EC2 instance.
- Ensure the **Specify username** field is set to `ubuntu` (for Ubuntu instances).

### 17. Add Your Key Pair
- Click on the **Advanced SSH settings** tab.
- Check the box for **Use private key**.
- Click the **...** button next to the field and navigate to the location of your **.pem** key file.
- Select your key file and click **Open**.

### 18. Start the Session
- Click **OK** to start the SSH session.
- MobaXterm will use the provided key file to authenticate and log you in to the EC2 instance.

### 19. Verify Connection
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
## Step 3: Create a TMDB API Key

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

## Step 4: Install Prometheus and Grafana on a New Server

## Set Up the Server

### a. Server Specifications

- **Operating System:** Ubuntu 22.04
- **Instance Type:** t2.medium
- **Memory:** 12 GB (optional, depending on your needs)
- **Purpose:**
  - Running applications
  - Monitoring Jenkins
  - Monitoring a Kubernetes cluster

### b. Security Group Configuration

- **Security Group:** 
  - Select the security group that was used when creating the previous machine (the one where Jenkins is installed). 
  - This ensures consistent access and security rules across your setup.
 
### C. Login to Server using Mobaxterm by providing details.

```
sudo apt-get update -y
```

 **Installing Prometheus:**

   First, create a dedicated Linux user for Prometheus and download Prometheus:

   ```bash
   sudo useradd --system --no-create-home --shell /bin/false prometheus
   wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz
   ```

   Extract Prometheus files, move them, and create directories:

   ```bash
   tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
   cd prometheus-2.47.1.linux-amd64/
   sudo mkdir -p /data /etc/prometheus
   sudo mv prometheus promtool /usr/local/bin/
   sudo mv consoles/ console_libraries/ /etc/prometheus/
   sudo mv prometheus.yml /etc/prometheus/prometheus.yml
   ```

   Set ownership for directories:

   ```bash
   sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
   ```

   Create a systemd unit configuration file for Prometheus:

   ```bash
   sudo nano /etc/systemd/system/prometheus.service
   ```

   Add the following content to the `prometheus.service` file:

   ```plaintext
   [Unit]
   Description=Prometheus
   Wants=network-online.target
   After=network-online.target

   StartLimitIntervalSec=500
   StartLimitBurst=5

   [Service]
   User=prometheus
   Group=prometheus
   Type=simple
   Restart=on-failure
   RestartSec=5s
   ExecStart=/usr/local/bin/prometheus \
     --config.file=/etc/prometheus/prometheus.yml \
     --storage.tsdb.path=/data \
     --web.console.templates=/etc/prometheus/consoles \
     --web.console.libraries=/etc/prometheus/console_libraries \
     --web.listen-address=0.0.0.0:9090 \
     --web.enable-lifecycle

   [Install]
   WantedBy=multi-user.target
   ```

   Here's a brief explanation of the key parts in this `prometheus.service` file:

   - `User` and `Group` specify the Linux user and group under which Prometheus will run.

   - `ExecStart` is where you specify the Prometheus binary path, the location of the configuration file (`prometheus.yml`), the storage directory, and other settings.

   - `web.listen-address` configures Prometheus to listen on all network interfaces on port 9090.

   - `web.enable-lifecycle` allows for management of Prometheus through API calls.

   Enable and start Prometheus:

   ```bash
   sudo systemctl enable prometheus
   sudo systemctl start prometheus
   ```

   Verify Prometheus's status:

   ```bash
   sudo systemctl status prometheus
   ```

   You can access Prometheus in a web browser using your server's IP and port 9090:

   `http://<your-server-ip>:9090`

![Screenshot (38)](https://github.com/user-attachments/assets/3efdbe60-7cf8-4d08-8257-3371eeb71b37)
![Screenshot (39)](https://github.com/user-attachments/assets/82190639-a82f-44eb-9b74-5c69a8e63bcb)
![Screenshot (40)](https://github.com/user-attachments/assets/5c685be8-bf5a-467f-8da0-db5bfa11c7fc)





   **Installing Node Exporter:**

   Create a system user for Node Exporter and download Node Exporter:

   ```bash
   sudo useradd --system --no-create-home --shell /bin/false node_exporter
   wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
   ```

   Extract Node Exporter files, move the binary, and clean up:

   ```bash
   tar -xvf node_exporter-1.6.1.linux-amd64.tar.gz
   sudo mv node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/
   rm -rf node_exporter*
   ```

   Create a systemd unit configuration file for Node Exporter:

   ```bash
   sudo nano /etc/systemd/system/node_exporter.service
   ```

   Add the following content to the `node_exporter.service` file:

   ```plaintext
   [Unit]
   Description=Node Exporter
   Wants=network-online.target
   After=network-online.target

   StartLimitIntervalSec=500
   StartLimitBurst=5

   [Service]
   User=node_exporter
   Group=node_exporter
   Type=simple
   Restart=on-failure
   RestartSec=5s
   ExecStart=/usr/local/bin/node_exporter --collector.logind

   [Install]
   WantedBy=multi-user.target
   ```

   Replace `--collector.logind` with any additional flags as needed.

   Enable and start Node Exporter:

   ```bash
   sudo systemctl enable node_exporter
   sudo systemctl start node_exporter
   ```

   Verify the Node Exporter's status:

   ```bash
   sudo systemctl status node_exporter
   ```

   You can access Node Exporter metrics in Prometheus.

   
![Screenshot (46)](https://github.com/user-attachments/assets/f031d97d-6e82-4c6b-a560-bce6d7818b26)
   
   **Prometheus Configuration:**

   To configure Prometheus to scrape metrics from Node Exporter, you need to modify the `prometheus.yml` file. Here is an example `prometheus.yml` configuration for your setup:

   ```yaml
   global:
     scrape_interval: 15s

   scrape_configs:
     - job_name: 'node_exporter'
       static_configs:
         - targets: ['localhost:9100']
   ```

   Make sure to replace `<your-jenkins-ip>` and `<your-jenkins-port>` with the appropriate values for your Jenkins setup.

   Check the validity of the configuration file:

   ```bash
   promtool check config /etc/prometheus/prometheus.yml
   ```

   Reload the Prometheus configuration without restarting:

   ```bash
   curl -X POST http://localhost:9090/-/reload
   ```

   You can access Prometheus targets at:

   `http://<your-prometheus-ip>:9090/targets`

![Screenshot (47)](https://github.com/user-attachments/assets/b123eb6c-1540-4e57-875e-1da8c5aa11f1)


####Grafana

**Install Grafana on Ubuntu 22.04 and Set it up to Work with Prometheus**

**Step 1: Install Dependencies:**

First, ensure that all necessary dependencies are installed:

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https software-properties-common
```

**Step 2: Add the GPG Key:**

Add the GPG key for Grafana:

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

**Step 3: Add Grafana Repository:**

Add the repository for Grafana stable releases:

```bash
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

**Step 4: Update and Install Grafana:**

Update the package list and install Grafana:

```bash
sudo apt-get update
sudo apt-get -y install grafana
```

**Step 5: Enable and Start Grafana Service:**

To automatically start Grafana after a reboot, enable the service:

```bash
sudo systemctl enable grafana-server
```

Then, start Grafana:

```bash
sudo systemctl start grafana-server
```

**Step 6: Check Grafana Status:**

Verify the status of the Grafana service to ensure it's running correctly:

```bash
sudo systemctl status grafana-server
```

**Step 7: Access Grafana Web Interface:**

Open a web browser and navigate to Grafana using your server's IP address. The default port for Grafana is 3000. For example:

`http://<your-server-ip>:3000`


![Screenshot (50)](https://github.com/user-attachments/assets/47797803-0bf2-4c3b-aada-7fe07fd90b58)

You'll be prompted to log in to Grafana. The default username is "admin," and the default password is also "admin."

**Step 8: Change the Default Password:**

When you log in for the first time, Grafana will prompt you to change the default password for security reasons. Follow the prompts to set a new password.

**Step 9: Add Prometheus Data Source:**

To visualize metrics, you need to add a data source. Follow these steps:

- Click on the gear icon (⚙️) in the left sidebar to open the "Configuration" menu.

- Select "Data Sources."

- Click on the "Add data source" button.
![Screenshot (51)](https://github.com/user-attachments/assets/f8b4885e-f15d-4eba-a7f4-cda7e321915f)
- Choose "Prometheus" as the data source type.
- In the "HTTP" section:
  - Set the "URL" to `http://localhost:9090` (assuming Prometheus is running on the same server).
  - Click the "Save & Test" button to ensure the data source is working.
![Screenshot (54)](https://github.com/user-attachments/assets/f48ed22d-6774-4ca4-8ae6-d94aa230ea21)
![Screenshot (55)](https://github.com/user-attachments/assets/e5f6e2b5-959f-44e6-877b-9baa08fc7b87)
![Screenshot (56)](https://github.com/user-attachments/assets/84899f05-b178-4bb4-b11c-0c8ba69e9963)



**Step 10: Import a Dashboard:**

To make it easier to view metrics, you can import a pre-configured dashboard. Follow these steps:

- Click on the "+" (plus) icon in the left sidebar to open the "Create" menu.

- Select "Dashboard."

- Click on the "Import" dashboard option.

- Enter the dashboard code you want to import (e.g., code 1860).

- Click the "Load" button.

- Select the data source you added (Prometheus) from the dropdown.

- Click on the "Import" button.
once you import you can see grafana dashboard like this
![Screenshot (57)](https://github.com/user-attachments/assets/a4b85076-d9a6-43a4-be6e-40b766829649)
![Screenshot (58)](https://github.com/user-attachments/assets/290a2136-2d2f-46e7-b252-5008f73ea8a6)


![Screenshot (52)](https://github.com/user-attachments/assets/cff01d39-a9e5-478b-843e-687e158e5779)


You should now have a Grafana dashboard set up to visualize metrics from Prometheus.


## Step 5: Install the Prometheus Plugin & Integrate Jenkins with the Prometheus server.

## Monitoring Jenkins with Prometheus

1. **Access Jenkins:**
   - Open your web browser and navigate to your Jenkins instance (e.g., `http://<jenkins-ip>:8080`).

2. **Go to Manage Jenkins:**
   - On the Jenkins dashboard, click on **Manage Jenkins**.

3. **Navigate to Plugin Manager:**
   - Click on **Manage Plugins**.

4. **Install the Prometheus Plugin:**
   - Go to the **Available** tab.
   - In the search box, type `Prometheus`.
   - Check the box next to the **Prometheus Metrics Plugin**.
   - Click **Install without restart** (or **Download now and install after restart** if necessary).

5. **Handle Restart Prompt:**
   - If prompted to restart Jenkins after the plugin installation, check the box to **Restart Jenkins when installation is complete and no jobs are running**.
   - Jenkins will automatically restart. Once restarted, you will be prompted to log in.
   - Enter your Jenkins credentials to log in.

6. **Apply and Save:**
   - After Jenkins has restarted and you've logged in, go back to **Manage Jenkins** and click on **Configure System**.
   - Scroll down to the **Prometheus** section.
   - Ensure the default settings are configured, then click **Apply** and **Save**.

## Configure Prometheus to Monitor Jenkins


1. **Access the Prometheus Configuration File in prometheus server:**
   - SSH into your Prometheus server.

   ```bash
   sudo vim /etc/prometheus/prometheus.yml
   ```
2. **Add Jenkins as a Target:**

- Paste the following configuration into the prometheus.yml file:

```
- job_name: 'jenkins'
  metrics_path: '/prometheus'
  static_configs:
    - targets: ['<jenkins-ip>:8080']
```
![Screenshot (60)](https://github.com/user-attachments/assets/2f3c05ad-bc4b-42cf-b234-de0e811e9883)

3. **Save and Exit:**

- Save the file and exit the text editor.

4. **Check the Configuration for Validity:**

- Before restarting Prometheus, check if the configuration is valid:
  ```
  promtool check config /etc/prometheus/prometheus.yml
  ```

5. **Reload the Prometheus Configuration:**
   
- Use a POST request to reload the configuration:
  ```
  curl -X POST http://localhost:9090/-/reload
  ```
  
6.** Verify the Configuration:**

- You will see Jenkins is added to it
- Check the targets section to ensure Jenkins is being monitored:

![Screenshot (61)](https://github.com/user-attachments/assets/21d852f8-eb2c-48f8-bb25-e559a80202fe)


## Add Dashboard in Grafana for Jenkins Monitoring

1. **Access Grafana:**
   - Open your web browser and navigate to your Grafana instance (e.g., `http://<grafana-ip>:3000`).

2. **Log In to Grafana:**
   - Enter your Grafana credentials to log in.

3. **Import a Dashboard:**
   - Click on **Dashboard** in the left-hand menu.
   - Click on the **+** symbol to add a new dashboard.
   - Select **Import**.

4. **Load the Jenkins Dashboard:**
   - In the **Import via grafana.com** section, enter the dashboard ID **9964**.
   - Click **Load**.
![Screenshot (62)](https://github.com/user-attachments/assets/abc729c9-c2b2-4cd0-8337-4da08de83c31)


5. **Select a Prometheus Data Source:**
   - In the **Import Dashboard** screen, select the Prometheus data source you configured earlier.
![Screenshot (64)](https://github.com/user-attachments/assets/fac52990-de3a-4712-9b12-ea7ba0ab6160)

6. **Click Import:**
   - Click **Import** to add the dashboard to Grafana.
   - You can now view a comprehensive overview of Jenkins.
![Screenshot (65)](https://github.com/user-attachments/assets/f0dc81e7-4a4f-4f2c-816b-bfc65c4989b5)


## Step 6: Email Integration With Jenkins and Plugin Setup

### Install Email Extension Plugin in Jenkins

1. **Install the Plugin:**
   - Go to Jenkins dashboard and navigate to **Manage Jenkins**.
   - Click on **Manage Plugins**.
   - Go to the **Available** tab.
   - Search for **Email Extension Plugin**.
   - Check the box next to **Email Extension Plugin** and click **Install**.
     
2. **Configure Gmail for Jenkins:**

   a. **Access Gmail Settings:**
      - Go to your Gmail account and click on your profile picture.
      - Click on **Manage Your Google Account**.

   b. **Enable 2-Step Verification:**
      - Click on the **Security** tab on the left-hand side.
      - Ensure that 2-Step Verification is enabled. If not, follow the prompts to enable it.

   c. **Generate App Password:**
      - In the **Security** section, find **App passwords**.
      - Click on **Select app** dropdown and choose **Other**.
      - Provide a name (e.g., "Jenkins") and click **Generate**.
      - Copy the generated app password. 

3. **Configure Credentials in Jenkins:**

   a. **Add New Credentials:**
      - Go to Jenkins dashboard and navigate to **Manage Jenkins**.
      - Click on **Manage Credentials**.
      - Click on **(global)** or the relevant domain if applicable.
      - Click on **Add Credentials**.

      - **Kind:** Select **Username with password**.
      - **Username:** Enter your Gmail address.
      - **Password:** Enter the app password you generated earlier.
      - **ID:** Provide a unique ID for the credential (e.g., `gmail-credentials`).
      - **Description:** Enter a description (e.g., `Gmail for Jenkins notifications`).
      -  Click **create** to save the credentials.


![Screenshot (66)](https://github.com/user-attachments/assets/a50534eb-39cb-401e-97cf-2e6d0c6e8b8f)

      
   b. **Configure E-mail Notification:**
      - Go to **Manage Jenkins** and click on **Configure System**.
      - Scroll down to the **E-mail Notification** section and configure the details as shown below:

![Screenshot (68)](https://github.com/user-attachments/assets/0f4859a0-f878-469b-a2f0-e380d8c70efa)
![Screenshot (69)](https://github.com/user-attachments/assets/b2a81022-b0db-4206-9210-258770156112)
![Screenshot (70)](https://github.com/user-attachments/assets/22a0b879-ee4a-4d5d-8d1e-7e653d10822f)
      - Click **Test Connectivity**
      - Check your **Email** 
![Screenshot (71)](https://github.com/user-attachments/assets/1aa5cff4-d089-4752-b71a-087ca5100c51)

   - This step is to validate the email configuration.

    
   c. Next, **configure the settings in the Extended E-mail Notification** section according to the details shown in the images below:

![Screenshot (77)](https://github.com/user-attachments/assets/662f61d0-e75d-4afa-80d3-06815424ba40)
![Screenshot (73)](https://github.com/user-attachments/assets/f0f7f87d-553d-4b70-a367-e97dbb7f8960)
![Screenshot (74)](https://github.com/user-attachments/assets/c1acbb1c-9ecb-4d27-a32a-520a6d464aa2)
![Screenshot (76)](https://github.com/user-attachments/assets/47821817-2f97-4a6e-96cd-6e572536d027)

  - Click **Apply** and **Save**.


## Install and Configure Plugins

### A: Install Required Plugins

1. **Access Plugin Manager:**
   - Navigate to **Manage Jenkins**.
   - Click on **Manage Plugins**.
   - Go to the **Available** tab.

2. **Install Plugins:**
   - Search for and install the following plugins:
     - **Eclipse Temurin Installer**
     - **SonarQube Scanner**
     - **NodeJS Plugin**
 ![Screenshot (78)](https://github.com/user-attachments/assets/8f8c28b7-65af-4433-a503-926403adbf63)

### B: Configure Tools in Global Tool Configuration

1. **Access Global Tool Configuration:**
   - Go to **Manage Jenkins**.
   - Click on **Tools**.

2. **Configure JDK:**
   - Find the **JDK** section.
   - Add the JDK version (e.g., JDK 17).

![Screenshot (80)](https://github.com/user-attachments/assets/fd584769-ff75-4a4f-8675-907556bb187e)


3. **Configure Node.js:**
   - Find the **NodeJS** section.
   - Add the Node.js version (e.g., Node.js 16).

![Screenshot (81)](https://github.com/user-attachments/assets/e170252a-6886-47f0-8963-ec8e45c04df8)


4. **Apply Changes:**
   - Click **Apply** and **Save**.
  
## Step 7: Configure SonarQube Server in Manage Jenkins

1. **Obtain SonarQube Server Details:**
   - Grab the **Public IP Address** of your EC2 instance.
   - Note that SonarQube operates on **Port 9000**. The URL will be in the format: `<Public IP>:9000`.

![Screenshot (82)](https://github.com/user-attachments/assets/83f28fe1-5b49-4545-a579-b0660789df7c)


2. **Generate a SonarQube Authentication Token:**
   - Go to your SonarQube server by navigating to the URL: `<Public IP>:9000`.
   - Click on **Administration**.
   - Go to **Security** and then **Users**.
   - Click on **Tokens**.
   - Click on **Generate** to create a new token:
     - Provide a name for the token.
     - Click **Generate Token** to obtain your token.

![Screenshot (84)](https://github.com/user-attachments/assets/b346caa8-56b4-42b0-a4ce-eb763b7240cd)
![Screenshot (85)](https://github.com/user-attachments/assets/7d360e43-c497-43b2-95dd-e0dadfb4a610)

**Copy Token**

3. Goto **Jenkins Dashboard → Manage Jenkins → Credentials → Add Secret Text**. It should look like this.

![Screenshot (87)](https://github.com/user-attachments/assets/74995d20-ec21-4890-8972-646d2fab0327)

4. Next, navigate to **Dashboard → Manage Jenkins → System** and add the configuration as shown in the image below.

![Screenshot (88)](https://github.com/user-attachments/assets/41c8ab9d-8107-4bd6-9ba5-4c1117a22c8d)
![Screenshot (90)](https://github.com/user-attachments/assets/42389727-6423-447f-93ee-16db49bd7d05)

Click on **Apply** and **Save**.

The **Configure System** option in Jenkins is used for setting up various server configurations.

5. We will add a **SonarQube Scanner** to this tools configuration.

   - Go to **Manage Jenkins**.
   - Click on **Tools**.

![Screenshot (92)](https://github.com/user-attachments/assets/654c47e3-5f2b-44dc-a617-69301e72d5c5)


6. In the **SonarQube Dashboard, add a quality gate** by navigating to:

**Administration → Configuration → Webhooks**

Click **Create** to add a new webhook. In the **URL** section, enter shown in below:

`http://jenkins-public-ip:8080/sonarqube-webhook/`


![Screenshot (94)](https://github.com/user-attachments/assets/096fe7b6-37fc-4e11-bd73-2201443fa27f)
![Screenshot (95)](https://github.com/user-attachments/assets/7a845a5a-2e6c-4257-af2e-d754c105b68b)


## Step 8: Create a Pipeline Job in Jenkins

1. **Open Jenkins Dashboard:**
   - Navigate to your Jenkins instance in a web browser.

2. **Create a New Item:**
   - Click on **New Item** or **Create New Jobs** from the left-hand menu.

3. **Enter Job Name:**
   - In the **Enter an item name** field, type the name of your pipeline job (e.g., `NetflixClone`).

4. **Select Pipeline Option:**
   - Choose **Pipeline** from the list of available options.

5. **Click OK:**
   - Click **OK** to proceed to the job configuration page.

6. **Configure Pipeline:**
   - On the configuration page, scroll down to the **Pipeline** section.
   - **Definition:** Choose **Pipeline script** to enter your pipeline code directly in Jenkins.
   - **Script:** Enter your pipeline script in the **Script** text box.

```
pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/Bathalapalli-SaiRangaPavan/Netflix-Clone-Assignment.git'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
    }
    post {
     always {
        emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME}<br/>" +
                "Build Number: ${env.BUILD_NUMBER}<br/>" +
                "URL: ${env.BUILD_URL}<br/>",
            to: 'bathalapalli.pavan@gmail.com',
            attachmentsPattern: 'trivyfs.txt'
        }
    }
}
```
7. **Save the Configuration:**
   - Click **Apply** and then **Save** to create the pipeline job.

8. **Run the Pipeline Job:**
   - Go back to the Jenkins dashboard and select your newly created pipeline job.
   - Click **Build Now** to run the pipeline.

- The build completed successfully, with all stages passing without issues.
![Screenshot (103)](https://github.com/user-attachments/assets/8e9fd6b2-4009-4a1c-89c7-d6007be168b6)
![Screenshot (101)](https://github.com/user-attachments/assets/c52ca637-b7d6-4239-9a14-51d6da3a2335)

- The quality gate report was successfully generated.
![Screenshot (100)](https://github.com/user-attachments/assets/fc89cfd3-1e21-405a-b1ce-4741f75e9c24)
![Screenshot (99)](https://github.com/user-attachments/assets/44305c38-4f3f-466c-8f1c-15a900412e8c)

- The Grafana dashboard displays that the Jenkins job was successful.
![Screenshot (108)](https://github.com/user-attachments/assets/d033bbbd-513f-4765-9532-ccfe6571e975)


- A notification email was received confirming the successful completion of the build.
![Screenshot (102)](https://github.com/user-attachments/assets/ea1cdd24-3da6-4c88-afa4-c14f88fd76cb)

## Step 9: Install OWASP Dependency Check Plugin

Go to the **Jenkins Dashboard → Manage Jenkins → Plugins**.

Search for **OWASP Dependency-Check**, click on it, and install the plugin.

First, we configured the plugin, and next, we need to configure the tool.

Go to **Dashboard → Manage Jenkins → Tools**-> Configure like below.

![Screenshot (109)](https://github.com/user-attachments/assets/1b9042df-f438-4854-8036-2acc83ea0b98)

 **Configure Pipeline:**
   - On the configuration page, scroll down to the **Pipeline** section.
   - **Script:** Enter your pipeline script in the **Script** text box.
```
pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Bathalapalli-SaiRangaPavan/Netflix-Clone-Assignment.git'
            }
        }
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix '''
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('OWASP FS Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('Trivy FS Scan') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
    }
    post {
        always {
            emailext attachLog: true,
                subject: "'${currentBuild.result}'",
                body: "Project: ${env.JOB_NAME}<br/>" +
                    "Build Number: ${env.BUILD_NUMBER}<br/>" +
                    "URL: ${env.BUILD_URL}<br/>",
                to: 'bathalapalli.pavan@gmail.com',
                attachmentsPattern: 'trivyfs.txt'
        }
    }
}
```
**Run the Pipeline Job:**
   - Go back to the Jenkins dashboard and select your newly created pipeline job.
   - Click **Build Now** to run the pipeline.

- The build completed successfully, with all stages passing without issues.
![Screenshot (114)](https://github.com/user-attachments/assets/ceb8267b-7247-4f5a-bef4-6f6716f80441)
- The quality gate report was successfully generated.
![Screenshot (117)](https://github.com/user-attachments/assets/ddcd97bd-b993-4fd0-bbd2-912033c7a3f5)

- A graph displaying the status, along with identified vulnerabilities, will be generated for better visualization. 
![Screenshot (113)](https://github.com/user-attachments/assets/3d6049d7-27c1-409d-af9b-0dddb5702ec8)

- The Grafana dashboard displays that the Jenkins job was successful.
![Screenshot (115)](https://github.com/user-attachments/assets/cef28101-3dbe-4212-99cf-3598c456cd55)

- A notification email was received confirming the successful completion of the build.
![Screenshot (111)](https://github.com/user-attachments/assets/50bb7e49-3f8b-4b93-b840-f8ba9fcdf238)


## Step 10 — Dockerfile, Build, push, and run a Docker image as a container.


**Note:** If you already have a DockerHub account, you can skip the DockerHub account creation steps.

To build and push Docker images, follow these steps:

1. **Create a DockerHub Account:**
   - Visit the [DockerHub website](https://hub.docker.com/).
   - Click on **Sign Up** to create a new account.
   - Fill in the required details, including your username, email, and password.
   - Complete the verification process by following the instructions sent to your email.
   - Log in to DockerHub using your credentials.

2. **Install Docker Plugins in Jenkins:**
   - Go to the Jenkins Dashboard.
   - Navigate to **Manage Jenkins** → **Manage Plugins** → **Available Plugins**.
   - Search for and install the following plugins:
     - Docker
     - Docker Commons
     - Docker Pipeline
     - Docker API
     - docker-build-step
   - Click **Install** without restarting Jenkins.

![Screenshot (118)](https://github.com/user-attachments/assets/a98cb7af-6bb6-4a2e-bb6a-2192838d1043)

3. **Add DockerHub Credentials in Jenkins:**
   - Go to the Jenkins Dashboard.
   - Navigate to **Manage Jenkins** → **Manage Credentials**.
   - Select the appropriate domain or create a new one if needed.
   - Click on **Add Credentials**.
   - Choose **Username with password** as the kind.
   - Enter your DockerHub username and password.
   - Provide an **ID** and **Description** for easy identification.
   - Click **create** to save the credentials.
  

![Screenshot (119)](https://github.com/user-attachments/assets/5438b9ce-f99c-4470-8859-a643841f1169)

4. **Configure Docker Tool:**
   - Go back to the Jenkins Dashboard.
   - Navigate to **Manage Jenkins** → **Global Tool Configuration**.
   - Set up the Docker tool by specifying the Docker installations and settings as required shown below.
  
  ![Screenshot (131)](https://github.com/user-attachments/assets/2052ad50-d311-4284-ad10-77439553c39d)

**Dockerfile**
Note: This file is already committed to the GitHub repository.
```
# Use the official Node.js 16.17.0 image based on Alpine Linux as the base image for the build stage
FROM node:16.17.0-alpine as builder

# Set the working directory inside the container to /app
WORKDIR /app

# Copy the package.json file from the host machine to the container's /app directory
COPY ./package.json .

# Copy the yarn.lock file from the host machine to the container's /app directory
COPY ./yarn.lock .

# Install the dependencies defined in package.json using yarn
RUN yarn install

# Copy the entire project directory from the host machine to the container's /app directory
COPY . .

# Define an argument to pass the TMDB API key into the container during build time
ARG TMDB_V3_API_KEY

# Set the environment variable VITE_APP_TMDB_V3_API_KEY inside the container with the provided TMDB API key
ENV VITE_APP_TMDB_V3_API_KEY=${TMDB_V3_API_KEY}

# Set another environment variable for the API endpoint URL
ENV VITE_APP_API_ENDPOINT_URL="https://api.themoviedb.org/3"

# Run the build command using yarn, which compiles the application for production
RUN yarn build

# Use the official Nginx image based on Alpine Linux for the final stage to serve the built application
FROM nginx:stable-alpine

# Set the working directory inside the container to the Nginx default content directory
WORKDIR /usr/share/nginx/html

# Remove all existing content in the Nginx content directory to prepare for the new build
RUN rm -rf ./*

# Copy the compiled files from the previous build stage's /app/dist directory to the Nginx content directory
COPY --from=builder /app/dist .

# Expose port 80, which Nginx will use to serve the application
EXPOSE 80

# Set the Nginx entry point command to start the server and keep the container running in the foreground
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```


**Configure Pipeline in Jenkins:**
   - On the configuration page, scroll down to the **Pipeline** section.
   - **Script:** Enter your pipeline script in the **Script** text box.

```
pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Bathalapalli-SaiRangaPavan/Netflix-Clone-Assignment.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
                        -Dsonar.projectKey=Netflix'''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('OWASP FS Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('Trivy FS Scan') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build --build-arg TMDB_V3_API_KEY=9e7e0d7ffdd5bda739af5bea6f8aa7e1 -t netflix ."
                        sh "docker tag netflix btppavan/netflix:1.0"
                        sh "docker push btppavan/netflix:1.0"
                    }
                }
            }
        }
        stage('TRIVY') {
            steps {
                sh "trivy image btppavan/netflix:latest > trivyimage.txt"
            }
        }
        stage('Deploy to Container') {
            steps {
                sh 'docker run -d --name netflix -p 8081:80 btppavan/netflix:1.0'
            }
        }
    }
    post {
        always {
            emailext attachLog: true,
                subject: "'${currentBuild.result}'",
                body: "Project: ${env.JOB_NAME}<br/>" +
                      "Build Number: ${env.BUILD_NUMBER}<br/>" +
                      "URL: ${env.BUILD_URL}<br/>",
                to: 'bathalapalli.pavan@gmail.com',
                attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
        }
    }
}

```

![Screenshot (136)](https://github.com/user-attachments/assets/45176329-41b0-497c-8d9d-0e4578091161)

![Screenshot (138)](https://github.com/user-attachments/assets/3430b1d9-476c-4f6f-b3b3-297b4f1934e0)

- The quality gate report was successfully generated.
![Screenshot (117)](https://github.com/user-attachments/assets/ddcd97bd-b993-4fd0-bbd2-912033c7a3f5)

- A graph displaying the status, along with identified vulnerabilities, will be generated for better visualization. 
![Screenshot (113)](https://github.com/user-attachments/assets/3d6049d7-27c1-409d-af9b-0dddb5702ec8)

- The Trivy scan report has been sent via email.
![Screenshot (141)](https://github.com/user-attachments/assets/364bf5b2-89ec-431a-8938-17bd72e3ec73)

- The Docker image has been pushed to DockerHub after the pipeline execution.
![Screenshot (161)](https://github.com/user-attachments/assets/c6b7d89f-be76-4aae-9cd9-cf5c2cabbb72)

- Trivy image report sent to mail
![Screenshot (142)](https://github.com/user-attachments/assets/c26b0ecd-68aa-4a81-81e3-e081f1f09f03)

- You can view and monitor all configured targets directly in the Prometheus dashboard
![Screenshot (145)](https://github.com/user-attachments/assets/1f2b30b5-802c-4312-9c03-eabeb57dd7bb)

- The Grafana dashboard displays that the Jenkins job was successful.
![Screenshot (146)](https://github.com/user-attachments/assets/7c25d2a4-622b-4e3d-bf05-3760fc60ff99)

- Ensure the necessary ports are open in the AWS security group to successfully access the output at the Jenkins URL: http://<jenkins-url>:port
![Screenshot (155)](https://github.com/user-attachments/assets/fa8c4231-a681-4046-a67b-0781218d7ac3)

## Step 11: Access the Netflix App on the Browser
- You can see that it has been successfully deployed and you are able to access the Netflix clone.
![Screenshot (152)](https://github.com/user-attachments/assets/9b7e5f51-e4e9-43a2-ab2c-46731b9f0988)

-  A notification email was received confirming the successful completion of the build and deploy.
![Screenshot (147)](https://github.com/user-attachments/assets/3e0cdb4a-61be-4583-8d1b-0440aed68efc)


## Tools and Technologies Used

To successfully achieve this assignment, the following tools and technologies were used:

1. **GitHub**: Version control and source code management.
2. **AWS Cloud**: Hosting and infrastructure management.
3. **Jenkins**: CI/CD pipeline automation with integrated security, quality checks, and testing.
4. **SonarQube**: Code quality and security analysis.
5. **npm**: Package management for Node.js, including testing.
6. **OWASP Dependency-Check**: Identifying and reporting vulnerable dependencies.
7. **Trivy**: Vulnerability scanning for Docker images.
8. **Docker**: Containerization of the application.
9. **Prometheus**: Metrics collection and monitoring.
10. **Node Exporter**: Exporting server metrics for Prometheus.
11. **Grafana**: Visualization and monitoring of metrics.
