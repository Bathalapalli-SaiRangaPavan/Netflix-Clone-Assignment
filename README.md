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
