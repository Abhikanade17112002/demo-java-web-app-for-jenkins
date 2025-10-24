<div align="center">

# 🚀 Java Web Application CI/CD Pipeline using Jenkins & Tomcat

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Java Version](https://img.shields.io/badge/Java-17-orange)
![Maven](https://img.shields.io/badge/Maven-3.8.7-blue)
![Tomcat](https://img.shields.io/badge/Tomcat-9.0.111-yellow)
![Jenkins](https://img.shields.io/badge/Jenkins-Latest-red)

A production-ready CI/CD pipeline setup for Java web application deployment using Jenkins and Apache Tomcat on AWS EC2.

</div>

---

## ✅ Overview

This project automates Java application deployment using **Jenkins CI/CD Pipeline**, **Maven** for build, and **Tomcat** as the deployment server hosted on **AWS EC2 (Ubuntu 24.04)**. The goal is to demonstrate **real-time DevOps automation** with minimal manual steps.

---

## 🏗️ Architecture Diagram

<div align="center">

```
Developer → GitHub → Jenkins CI → Maven Build → Tomcat Deployment → User
```

</div>

---

## 🔧 Tech Stack

| Tool          | Version  | Purpose                |
| ------------- | -------- | ---------------------- |
| Java          | 17       | Programming Language   |
| Maven         | 3.8.7    | Build & Dependencies   |
| Jenkins       | Latest   | Continuous Integration |
| Apache Tomcat | 9.0.111  | Application Server     |
| AWS EC2       | t3.micro | Deployment Server      |
| Git           | 2.x      | Version Control        |

---

## 📸 Project Screenshots

### ✅ AWS EC2 Elastic IP

![Elastic IP](https://raw.githubusercontent.com/Abhikanade17112002/demo-java-web-app-for-jenkins/main/Static%20Files/Elastic%20Ip%20SS.png)

### ✅ Tomcat Server Running

![Tomcat Server](https://raw.githubusercontent.com/Abhikanade17112002/demo-java-web-app-for-jenkins/main/Static%20Files/Tomcat%20Server%20SS.png)

### ✅ Web App Deployment

![Web App](https://raw.githubusercontent.com/Abhikanade17112002/demo-java-web-app-for-jenkins/main/Static%20Files/Web%20App%20Deployment%20SS.png)

### ✅ Jenkins CI/CD Pipeline

![Jenkins Pipeline](https://raw.githubusercontent.com/Abhikanade17112002/demo-java-web-app-for-jenkins/main/Static%20Files/Screenshot%202025-10-16%20180415.png)
![Jenkins Deploy](https://raw.githubusercontent.com/Abhikanade17112002/demo-java-web-app-for-jenkins/main/Static%20Files/Screenshot%202025-10-24%20120519.png)

---

## 📦 Prerequisites

* AWS EC2 instance
* Git installed
* Java 17 installed
* Jenkins installed
* Apache Tomcat 9 installed
* Maven installed

---

## ⚙️ CI/CD Pipeline Workflow

1. Developer pushes code to GitHub
2. Jenkins pulls latest code
3. Maven builds the WAR file
4. Jenkins deploys to Tomcat
5. Application goes live ⚡

---

## 🛠️ Deployment Commands (Sample)

```bash
mvn clean install
scp target/demo.war ubuntu@EC2-IP:/opt/tomcat9/webapps/
```

---

## 🔗 Access URLs

| Service           | URL                                                                          |
| ----------------- | ---------------------------------------------------------------------------- |
| Jenkins Dashboard | [http://EC2-IP:8080](http://EC2-IP:8080)                                     |
| Tomcat Server     | [http://EC2-IP:8081](http://EC2-IP:8081)                                     |
| Deployed App      | [http://EC2-IP:8081/Demo-Java-Web-App](http://EC2-IP:8081/Demo-Java-Web-App) |

---

## 🧩 Project Structure

```
project/
├── src/main/java
├── src/main/webapp
├── pom.xml
└── README.md
```

---

## 🚀 Future Enhancements

* Add GitHub Webhooks
* Dockerize the application
* Deploy using Kubernetes
* Integrate SonarQube code quality

---

## 👨‍💻 Author

**Abhishek Kanade**

* GitHub: [https://github.com/Abhikanade17112002](https://github.com/Abhikanade17112002)

---

> ⭐ *If you like this project, don’t forget to star the repo!* ⭐

<div align="center">

# 🚀 Java Web Application CI/CD Pipeline with Jenkins & Tomcat

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]()
[![Java Version](https://img.shields.io/badge/Java-17-orange)]()
[![Maven](https://img.shields.io/badge/Maven-3.8.7-blue)]()
[![Tomcat](https://img.shields.io/badge/Tomcat-9.0.111-yellow)]()
[![Jenkins](https://img.shields.io/badge/Jenkins-Latest-red)]()

### End-to-end CI/CD automation for Java web applications using Jenkins, Maven, and Apache Tomcat on AWS EC2

---

### 📌 Project Screenshots

| Elastic IP Configuration                                                                                                                    | Tomcat Server Running                                                                                                                          | Web App Deployment                                                                                                                                    |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| <img src="https://github.com/Abhikanade17112002/demo-java-web-app-for-jenkins/blob/main/Static%20Files/Elastic%20Ip%20SS.png" width="300"/> | <img src="https://github.com/Abhikanade17112002/demo-java-web-app-for-jenkins/blob/main/Static%20Files/Tomcat%20Server%20SS.png" width="300"/> | <img src="https://github.com/Abhikanade17112002/demo-java-web-app-for-jenkins/blob/main/Static%20Files/Web%20App%20Deployment%20SS.png" width="300"/> |

| Jenkins Dashboard                                                                                                                                          | CI/CD Job Console Output                                                                                                                                   | Maven Build Success                                                                                                                                        |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <img src="https://github.com/Abhikanade17112002/demo-java-web-app-for-jenkins/blob/main/Static%20Files/Screenshot%202025-10-16%20180415.png" width="300"/> | <img src="https://github.com/Abhikanade17112002/demo-java-web-app-for-jenkins/blob/main/Static%20Files/Screenshot%202025-10-24%20120519.png" width="300"/> | <img src="https://github.com/Abhikanade17112002/demo-java-web-app-for-jenkins/blob/main/Static%20Files/Screenshot%202025-10-24%20120559.png" width="300"/> |

</div>

---
