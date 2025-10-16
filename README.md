# # Fully Automated CI/CD Pipeline for Web Hosting on AWS

![Pipeline Status](https://img.shields.io/badge/Pipeline-Passing-brightgreen?style=for-the-badge&logo=jenkins)
![Platform](https://img.shields.io/badge/Platform-AWS-orange?style=for-the-badge&logo=amazon-aws)
![Technology](https://img.shields.io/badge/Tech-Docker%20%7C%20Apache-blue?style=for-the-badge&logo=docker)

This repository showcases a complete, end-to-end **Continuous Integration/Continuous Deployment (CI/CD)** pipeline. The system automates the process of deploying a static website, taking code from a `git push` and making it live on a web server in minutes, with zero manual intervention.

This project is a practical implementation of modern DevOps principles, demonstrating how to integrate best-in-class tools to create an efficient, reliable, and secure deployment workflow.

---

## 📋 Table of Contents

1.  [**Project Goal**](#-project-goal)
2.  [**Core Concepts Explained**](#-core-concepts-explained)
3.  [**Architectural Workflow**](#-architectural-workflow)
4.  [**Technology Stack & Roles**](#-technology-stack--roles)
5.  [**Visual Showcase**](#-visual-showcase)
    * [Jenkins Pipeline Dashboard](#jenkins-pipeline-dashboard)
    * [Cloud Infrastructure (AWS)](#cloud-infrastructure-aws)
    * [Containerization (Docker)](#containerization-docker)
    * [The Pipeline as Code (Jenkinsfile)](#the-pipeline-as-code-jenkinsfile)
6.  [**Setup & Configuration**](#-setup--configuration)
7.  [**How to Trigger the Pipeline**](#-how-to-trigger-the-pipeline)

---

## 🎯 Project Goal

The primary objective was to build a "zero-touch" deployment system. This means that a developer should only need to focus on writing code and pushing it to GitHub. The pipeline handles everything else automatically: fetching the code, deploying it to a server, and making it live. This minimizes human error, speeds up delivery, and creates a repeatable deployment process.

---

## 🧠 Core Concepts Explained

This project is built on several key DevOps concepts:

* **CI/CD (Continuous Integration/Continuous Deployment):** This is a practice where code changes from developers are frequently integrated, tested, and automatically deployed to production. Our pipeline automates this entire flow.
* **Infrastructure as Code (IaC):** The `Jenkinsfile` is a perfect example of IaC. Instead of configuring the deployment steps manually through a UI, we define them in a code file. This makes our pipeline version-controlled, reproducible, and easy to audit.
* **Master-Agent Architecture:** Jenkins is set up in this model. The **Jenkins Master** (in a Docker container) orchestrates the jobs but doesn't perform the actual work. It delegates the tasks to **Agent Nodes** (our EC2 instance). This is a scalable model because we can add many agents to handle different jobs without overloading the master.
* **Containerization:** We run Jenkins inside a **Docker container**. This isolates Jenkins from the host system, making it easy to set up, upgrade, and move between different machines without worrying about dependencies.

---

## 🔄 Architectural Workflow

The pipeline operates in a clear, sequential flow triggered by a single action:

1.  **Code Commit:** A developer pushes code changes (`index.html`, `styles.css`) to the `main` branch of the **GitHub Repository**.
2.  **Trigger:** A GitHub Webhook (or scheduled poll) notifies the **Jenkins Master** of the new commit.
3.  **Orchestration:** The Jenkins Master, running in a **Docker Container**, reads the `Jenkinsfile` and starts the pipeline.
4.  **Delegation:** Jenkins securely connects to the **EC2 Agent Node** via SSH.
5.  **Execution:** The agent runs the steps defined in the `Jenkinsfile`:
    * Pulls the latest source code from GitHub.
    * Copies the website files to the Apache webroot (`/var/www/html`).
    * Restarts the Apache service to apply the changes.
6.  **Live:** The updated website is immediately available to users.

---

## 🛠️ Technology Stack & Roles

| Technology | Role in the Project |
| :--- | :--- |
| **AWS EC2** | The cloud-based virtual server (Ubuntu 22.04) that hosts our Apache web server and serves as the Jenkins agent. |
| **Jenkins** | The core CI/CD automation server. It orchestrates the entire pipeline from code-fetch to deployment. |
| **Docker** | Used to containerize the Jenkins master, ensuring a clean, isolated, and portable environment. |
| **Git & GitHub** | The version control system and cloud repository. It's the single source of truth for our website code and the trigger point for the pipeline. |
| **Apache2** | The robust and widely-used web server software responsible for serving our static website to the internet. |
| **Groovy** | The language used to write the declarative `Jenkinsfile` script. |

---

## 📸 Visual Showcase

### Jenkins Pipeline Dashboard
The Stage View provides a clear, real-time overview of the pipeline's progress and history.

![Jenkins Stage View](assets/Screenshot%202025-10-16%20at%206.25.41%E2%80%AFPM.jpg)

### Cloud Infrastructure (AWS)
Our infrastructure is built on AWS, using an EC2 instance for compute and Security Groups for network security.

* **EC2 Instance Summary:**
    ![EC2 Instance Summary](assets/Screenshot%202025-10-16%20at%206.26.33%E2%80%AFPM.png)

* **Security Group Rules:**
    ![AWS Security Group Rules](assets/Screenshot%202025-10-16%20at%206.26.47%E2%80%AFPM.jpg)

### Containerization (Docker)
The Jenkins master runs cleanly inside a Docker container, with SSH keys generated from within the container to securely connect to the agent.

![Docker Desktop with Jenkins Container](assets/Screenshot%202025-10-16%20at%206.26.26%E2%80%AFPM.jpg)

### The Pipeline as Code (Jenkinsfile)
This Groovy script is the heart of our automation. It declaratively defines every stage and step of the deployment process.

![Jenkinsfile Pipeline Code](assets/Screenshot%202025-10-16%20at%206.34.25%E2%80%AFPM.jpg)

---

## ⚙️ Setup & Configuration

This project requires setting up an AWS EC2 instance, configuring it as a Jenkins agent, and running the Jenkins master in a Docker container. The master connects to the agent via SSH keys for secure command execution. For a full guide, refer to standard tutorials on setting up a Jenkins Master-Agent architecture with Docker and AWS.

---

## ▶️ How to Trigger the Pipeline

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/Kunal061/apache.git](https://github.com/Kunal061/apache.git)
    ```
2.  **Make a Change:** Modify the `index.html` or `styles.css` file.
3.  **Commit and Push:** Add your changes and push them to the `main` branch.
    ```bash
    git add .
    git commit -m "Updated website content"
    git push origin main
    ```
4.  **Observe:** The pipeline will automatically be triggered in Jenkins, and your changes will be live within moments.
