# 🚀 CI/CD Pipeline with Jenkins, Ansible, Docker & GitHub Webhooks on AWS

## 📌 Overview
Designed and implemented a fully automated Continuous Integration and Continuous Deployment (CI/CD) pipeline to deploy a static web application using:

- **Jenkins** for automation and orchestration
- **GitHub** for version control and webhook triggers
- **Ansible** for provisioning and remote Docker operations
- **Docker** for containerization
- **AWS EC2** instances for cloud-based infrastructure

This project automatically deploys an updated Dockerized app whenever code is pushed to GitHub.
- ![Whole Pipeline](images/bfa69396-a498-428e-aec6-ae66ee87bdad.jfif)
---

## 🔁 Pipeline Workflow

### 1. **📥 GitHub Integration**
- Source code is hosted on a GitHub repository.
- Configured **webhooks** to automatically trigger Jenkins jobs on every `push` or `pull` request.
- ![GitHub Webhook Setup](images/github-webhook.JPG)

---

### 2. **🛠 Jenkins Automation**
- Jenkins is installed on an EC2 Ubuntu instance.
- Freestyle job created:
  - Pulls the latest code
  - Executes shell commands to copy files to Docker host and run Ansible playbook
- Jenkins handles pipeline automation from source to deployment.
- ![Jenkins Job](images/1.JPG)
- ![Jenkins Console Output](images/buildingimage.JPG)

---

### 3. **📦 Ansible Remote Deployment**
- Ansible runs on the Jenkins host.
- Playbook automates:
  - File transfer to Docker host
  - Docker image removal (if any)
  - Image build from Dockerfile
  - Container creation and exposure on port 80
- Inventory configured to use remote interpreter via virtualenv due to Python environment restrictions (PEP 668).
- ![Ansible Playbook](images/deploymentimg.JPG)
- ![Ansible Hosts](images/yamlfiledeployment.JPG)

---

### 4. **🐳 Docker Containerization**
- Docker is installed on a separate EC2 instance.
- The application is served inside a container using an NGINX base image.
- Docker image built via Ansible and deployed on port `80`.
- ![Docker Running](images/dockerserver.JPG)
- ![Web App](images/website.JPG)

---

### 5. **☁️ Deployment on AWS EC2**
- Two EC2 instances were used:
  - One for Jenkins + Ansible
  - One for Docker hosting
- Security groups configured to allow:
  - Port `8080` (Jenkins)
  - Port `80` (Web App)
- SSH key-based access set up between Jenkins and Docker hosts.
- ![AWS Instances](images/2.JPG)
- ![AWS Instances](images/pipelineworking.JPG)

---

## 🛠️ Tools & Technologies

- **Jenkins** - CI/CD Orchestration
- **Ansible** - Remote provisioning
- **Docker** - Containerization
- **GitHub** - Source control & webhook
- **AWS EC2** - Cloud hosting
- **Ubuntu 22.04** - Operating System
- **Python venv** - Environment isolation for Docker SDK

---

## 🐞 Challenges & Fixes

| Issue | Solution |
|-------|----------|
| `state is present but source is missing` | Added `source: build` to Ansible Docker task |
| `pip install docker` fails on Ubuntu 22+ | Used `python3-venv` to isolate Docker SDK install (PEP 668 fix) |
| Jenkins to Docker host SSH issue | Set up key-based SSH and tested connectivity |
| Docker build not working via Ansible | Ensured proper directory structure and Dockerfile placement |

---

## 🎯 Future Improvements

- Add automated unit tests before deployment
- Use Docker Compose or Kubernetes for better scaling
- Implement Slack/email notifications on successful deploy
- Add SSL/HTTPS with Let's Encrypt and NGINX

---

## 💼 Result

- A fully automated CI/CD pipeline triggered by GitHub pushes
- Jenkins orchestrates Ansible, which remotely deploys to Docker
- Containerized app live on AWS in under a minute

> 🔗 Want to collaborate or provide feedback? Feel free to connect on [LinkedIn](https://www.linkedin.com/) or fork this project!

---

## 📸 Screenshots

> Replace these placeholder paths with your actual image file names under a `./images/` folder.

- GitHub Webhook Setup  
  ![](images/github-webhook.png)

- Jenkins Job Console Output  
  ![](images/jenkins-console.png)

- Ansible Inventory Setup  
  ![](images/ansible-hosts.png)

- Docker Container Running  
  ![](images/docker-running.png)

- Deployed Website  
  ![](images/deployed-site.png)

- AWS EC2 Instances  
  ![](images/aws-ec2.png)
