# Java Web Application CI/CD Pipeline with Jenkins and Tomcat

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]()
[![Java Version](https://img.shields.io/badge/Java-17-orange)]()
[![Maven](https://img.shields.io/badge/Maven-3.8.7-blue)]()
[![Tomcat](https://img.shields.io/badge/Tomcat-9.0.111-yellow)]()
[![Jenkins](https://img.shields.io/badge/Jenkins-Latest-red)]()

## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Technologies Used](#technologies-used)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [CI/CD Pipeline Configuration](#cicd-pipeline-configuration)
- [Deployment](#deployment)
- [Access URLs](#access-urls)
- [Project Structure](#project-structure)
- [Troubleshooting](#troubleshooting)
- [Cost Optimization](#cost-optimization)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Project Overview

This project demonstrates a complete **CI/CD pipeline** implementation for a Java web application using **Jenkins** for continuous integration and **Apache Tomcat** for deployment. The infrastructure is hosted on **AWS EC2**, providing a scalable and production-ready environment.

### Key Features
- ✅ Automated build process using Maven
- ✅ Continuous Integration with Jenkins
- ✅ Automated deployment to Tomcat server
- ✅ Git-based version control integration
- ✅ Cloud-hosted on AWS EC2
- ✅ Elastic IP for consistent access

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        AWS EC2 Instance                      │
│                      (Ubuntu 24.04 LTS)                      │
│                                                               │
│  ┌──────────────┐         ┌──────────────┐                  │
│  │   Jenkins    │────────▶│   Tomcat 9   │                  │
│  │  (Port 8080) │         │  (Port 8081) │                  │
│  └──────────────┘         └──────────────┘                  │
│         │                        │                            │
│         │                        │                            │
│    ┌────▼────┐            ┌─────▼──────┐                    │
│    │  Maven  │            │ Web Apps   │                    │
│    │  Git    │            │ Deployment │                    │
│    └─────────┘            └────────────┘                    │
│                                                               │
└─────────────────────────────────────────────────────────────┘
                          │
                          │ HTTPS/HTTP
                          ▼
                    ┌──────────┐
                    │  GitHub  │
                    │   Repo   │
                    └──────────┘
```

---

## 💻 Technologies Used

| Technology | Version | Purpose |
|------------|---------|---------|
| **Java (OpenJDK)** | 17.0.16 | Runtime Environment |
| **Apache Maven** | 3.8.7 | Build Automation |
| **Git** | 2.43.0 | Version Control |
| **Apache Tomcat** | 9.0.111 | Application Server |
| **Jenkins** | Latest | CI/CD Automation |
| **AWS EC2** | t3.micro | Cloud Infrastructure |
| **Ubuntu** | 24.04 LTS | Operating System |

---

## 📦 Prerequisites

Before you begin, ensure you have:

- AWS Account with EC2 access
- SSH key pair for EC2 instance access
- GitHub account (for repository hosting)
- Basic knowledge of Linux commands
- Understanding of CI/CD concepts

---

## 🚀 Installation & Setup

### 1. EC2 Instance Setup

**Launch EC2 Instance:**
```bash
Instance Type: t3.micro (or higher)
AMI: Ubuntu 24.04 LTS
Storage: 8GB (minimum)
Security Group: Allow ports 22, 8080, 8081
```

**Allocate Elastic IP:**
```bash
EC2 Console → Elastic IPs → Allocate → Associate with instance
```

**Connect to Instance:**
```bash
ssh -i your-key.pem ubuntu@your-elastic-ip
```

### 2. Install Java 17

```bash
# Update package index
sudo apt update

# Install OpenJDK 17
sudo apt install openjdk-17-jdk -y

# Set JAVA_HOME
echo 'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64' >> ~/.bashrc
echo 'export PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc
source ~/.bashrc

# Verify installation
java -version
```

### 3. Install Maven & Git

```bash
# Install Maven and Git
sudo apt install maven git -y

# Verify installations
mvn --version
git --version

# Configure Git (optional)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 4. Install Apache Tomcat 9

```bash
# Download Tomcat 9
cd /tmp
wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.111/bin/apache-tomcat-9.0.111.tar.gz

# Extract and move to /opt
sudo tar xzvf apache-tomcat-9.0.111.tar.gz -C /opt
sudo mv /opt/apache-tomcat-9.0.111 /opt/tomcat9

# Create tomcat user
sudo useradd -r -m -U -d /opt/tomcat9 -s /bin/false tomcat
sudo chown -R tomcat:tomcat /opt/tomcat9

# Configure port 8081
sudo nano /opt/tomcat9/conf/server.xml
# Change <Connector port="8080" to <Connector port="8081"

# Create systemd service
sudo nano /etc/systemd/system/tomcat9.service
```

**Tomcat Service Configuration:**
```ini
[Unit]
Description=Apache Tomcat 9 Web Application Container
After=network.target

[Service]
Type=forking

Environment="JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64"
Environment="CATALINA_PID=/opt/tomcat9/temp/tomcat.pid"
Environment="CATALINA_HOME=/opt/tomcat9"
Environment="CATALINA_BASE=/opt/tomcat9"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat9/bin/startup.sh
ExecStop=/opt/tomcat9/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

**Configure Tomcat Users:**
```bash
sudo nano /opt/tomcat9/conf/tomcat-users.xml
```

Add before `</tomcat-users>`:
```xml
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<role rolename="manager-script"/>
<user username="admin" password="admin123" roles="manager-gui,admin-gui,manager-script"/>
```

**Allow Remote Access:**
```bash
sudo nano /opt/tomcat9/webapps/manager/META-INF/context.xml
```

Comment out or modify the Valve:
```xml
<!-- <Valve className="org.apache.catalina.valves.RemoteCIDRValve"
       allow="0.0.0.0/0" /> -->
```

**Start Tomcat:**
```bash
sudo chmod +x /opt/tomcat9/bin/*.sh
sudo systemctl daemon-reload
sudo systemctl start tomcat9
sudo systemctl enable tomcat9
sudo systemctl status tomcat9
```

### 5. Install Jenkins

```bash
# Add Jenkins repository key
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add Jenkins repository
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Update and install Jenkins
sudo apt update
sudo apt install jenkins -y

# Start Jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins

# Get initial admin password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### 6. Configure AWS Security Group

Add inbound rules:
- **Port 22** (SSH) - Your IP
- **Port 8080** (Jenkins) - 0.0.0.0/0 or Your IP
- **Port 8081** (Tomcat) - 0.0.0.0/0 or Your IP

---

## ⚙️ CI/CD Pipeline Configuration

### 1. Access Jenkins

Navigate to: `http://your-elastic-ip:8080`

1. Enter initial admin password
2. Install suggested plugins
3. Create first admin user

### 2. Configure Jenkins Global Tools

**Navigate to:** Manage Jenkins → Tools

**JDK Configuration:**
- Name: `Java-17`
- JAVA_HOME: `/usr/lib/jvm/java-17-openjdk-amd64`
- Uncheck "Install automatically"

**Maven Configuration:**
- Name: `Maven-3.8.7`
- MAVEN_HOME: `/usr/share/maven`
- Uncheck "Install automatically"

**Git Configuration:**
- Name: `Default`
- Path: `/usr/bin/git`
- Uncheck "Install automatically"

### 3. Install Required Plugins

**Manage Jenkins → Plugins → Available**

Install:
- Deploy to container Plugin
- Git Plugin (if not already installed)
- Maven Integration Plugin

### 4. Add Tomcat Credentials

**Manage Jenkins → Credentials → System → Global credentials → Add Credentials**

```
Kind: Username with password
Username: admin
Password: admin123
ID: tomcat-credentials
Description: Tomcat 9 Admin Credentials
```

### 5. Create Jenkins Job

**New Item → Freestyle Project**

**General:**
- Project name: `Demo-java-web-app`
- Description: Java Web Application CI/CD Pipeline

**Source Code Management:**
- Git
- Repository URL: `https://github.com/Abhikanade17112002/demo-java-web-app-for-jenkins`
- Branch: `*/main`

**Build Environment:**
- Delete workspace before build starts (optional)

**Build Steps:**
- Invoke top-level Maven targets
- Maven Version: `Maven-3.8.7`
- Goals: `clean install`

**Post-build Actions:**
- Deploy war/ear to a container
  - WAR/EAR files: `target/*.war`
  - Context path: `Demo-Java-Web-App`
  - Containers: Tomcat 9.x Remote
  - Credentials: `tomcat-credentials`
  - Tomcat URL: `http://localhost:8081`

**Save and Build Now**

---

## 🚀 Deployment

### Manual Deployment (if needed)

```bash
# Build the project
cd /var/lib/jenkins/workspace/Demo-java-web-app
mvn clean install

# Deploy to Tomcat
sudo cp target/Demo-Java-Web-App.war /opt/tomcat9/webapps/

# Verify deployment
ls -lh /opt/tomcat9/webapps/
```

### Automated Deployment

Simply push code to GitHub and trigger Jenkins build:
```bash
git add .
git commit -m "Update feature"
git push origin main
```

Then in Jenkins, click **Build Now**

---

## 🌐 Access URLs

| Service | URL | Credentials |
|---------|-----|-------------|
| **Jenkins** | `http://your-elastic-ip:8080` | Admin user created during setup |
| **Tomcat Manager** | `http://your-elastic-ip:8081/manager/html` | admin / admin123 |
| **Web Application** | `http://your-elastic-ip:8081/Demo-Java-Web-App` | N/A |
| **Tomcat Home** | `http://your-elastic-ip:8081` | N/A |

---

## 📁 Project Structure

```
demo-java-web-app/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── [Java source files]
│   │   ├── resources/
│   │   │   └── [Configuration files]
│   │   └── webapp/
│   │       ├── WEB-INF/
│   │       │   └── web.xml
│   │       └── index.jsp
│   └── test/
│       └── [Test files]
├── target/
│   └── Demo-Java-Web-App.war
├── pom.xml
└── README.md
```

---

## 🔧 Troubleshooting

### Common Issues and Solutions

#### Jenkins Build Fails
```bash
# Check Jenkins logs
sudo journalctl -u jenkins -f

# Check workspace permissions
sudo chown -R jenkins:jenkins /var/lib/jenkins/workspace/
```

#### Tomcat Deployment Fails
```bash
# Check Tomcat logs
sudo tail -f /opt/tomcat9/logs/catalina.out

# Verify Tomcat is running
sudo systemctl status tomcat9

# Restart Tomcat
sudo systemctl restart tomcat9
```

#### 403 Forbidden on Tomcat Manager
```bash
# Verify tomcat-users.xml configuration
sudo cat /opt/tomcat9/conf/tomcat-users.xml

# Check context.xml for IP restrictions
sudo nano /opt/tomcat9/webapps/manager/META-INF/context.xml
```

#### Out of Memory Errors
```bash
# Add swap space
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Reduce Java heap size
# Edit Jenkins: /etc/default/jenkins
# Edit Tomcat: /etc/systemd/system/tomcat9.service
```

---

## 💰 Cost Optimization

### Stop EC2 Instance When Not In Use

```bash
# Charges when running (t3.micro): ~$7.50/month
# Charges when stopped: ~$0.80/month (EBS only)
# Savings: ~90%
```

**To Stop:**
```
AWS Console → EC2 → Instance State → Stop
```

**To Start:**
```
AWS Console → EC2 → Instance State → Start
```

### Add Swap Space (Free Performance Boost)

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

### Resource Monitoring

```bash
# Check memory usage
free -h

# Check disk usage
df -h

# Monitor processes
top
htop
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Abhishek Kanade**

- GitHub: [@Abhikanade17112002](https://github.com/Abhikanade17112002)
- LinkedIn: [Your LinkedIn Profile]
- Email: your.email@example.com

---

## 🙏 Acknowledgments

- Apache Software Foundation for Tomcat and Maven
- Jenkins Community for the excellent CI/CD tool
- AWS for providing cloud infrastructure
- Ubuntu for the stable operating system

---

## 📊 Project Status

- ✅ **Setup Complete:** EC2, Java, Maven, Git
- ✅ **Tomcat Configured:** Running on port 8081
- ✅ **Jenkins Configured:** Running on port 8080
- ✅ **CI/CD Pipeline:** Automated build and deployment
- ✅ **Production Ready:** Deployed and accessible

---

## 🔮 Future Enhancements

- [ ] Implement GitHub webhooks for automatic builds
- [ ] Add Docker containerization
- [ ] Set up Kubernetes orchestration
- [ ] Implement automated testing
- [ ] Add SonarQube for code quality analysis
- [ ] Configure HTTPS with SSL certificates
- [ ] Set up Nginx reverse proxy
- [ ] Implement blue-green deployment strategy
- [ ] Add monitoring with Prometheus and Grafana
- [ ] Configure automated backups

---

## 📞 Support

For issues, questions, or contributions, please:
- Open an issue on GitHub
- Contact via email
- Check the troubleshooting section above

---

**Made with ❤️ by Abhishek Kanade**

*Last Updated: October 24, 2025*