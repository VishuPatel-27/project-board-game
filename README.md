# BoardgameListingWebApp - A Full-Stack DevOps Project

## Project Overview
BoardgameListingWebApp is a full-stack web application designed to showcase board games and their reviews. The application follows authentication and authorization best practices, and it has been deployed using a complete DevOps pipeline with Jenkins, Kubernetes, and AWS.

I've also written a comprehensive [blog](https://imv27-blogs.hashnode.dev/how-to-build-a-cicd-pipeline-for-java-spring-boot-with-jenkins-kubernetes-and-aws-ec2) about this project.

## Tech Stack
### Application Technologies
- **Backend:** Java, Spring Boot, Spring MVC, JDBC
- **Frontend:** Thymeleaf, HTML5, CSS, JavaScript, Bootstrap
- **Database:** H2 (In-memory)
- **Security:** Spring Security (Role-Based Authentication & Authorization)
- **Testing:** JUnit
- **Build & Dependency Management:** Maven

### DevOps Technologies
- **CI/CD:** Jenkins
- **Code Quality & Security:** SonarQube, Trivy
- **Containerization & Orchestration:** Docker, Kubernetes (kubeadm)
- **Artifact Repository:** Nexus
- **Infrastructure:** AWS EC2 (Jenkins, SonarQube, Nexus, Kubernetes cluster)
- **Monitoring:** Prometheus, Grafana
- **RBAC & Security:** Kubernetes RBAC, Kubeaudit

---

## Features
### Application Features
- **Full-Stack Web App** with CRUD functionalities
- **Role-Based Authentication & Authorization** (Users & Managers)
- **Game & Review Management** (Users can add, managers can edit/delete)
- **Deployed on AWS EC2** for real-world testing
- **JUnit Tests** for unit testing

### DevOps Features
- **CI/CD Pipeline:** Automated build, test, security scan, and deployment
- **Code Analysis:** Integrated with SonarQube for code quality checks
- **Security Scanning:** Trivy for container vulnerabilities, Kubeaudit for cluster security
- **Kubernetes Deployment:** Cluster set up using `kubeadm` following best practices
- **Monitoring & Logging:** Prometheus & Grafana dashboards for system and application metrics

---

## Project Architecture
```
+---------------------------------------+
|           GitHub Repository           |
+---------------------------------------+
              |  (Webhook)  
              v  
+---------------------------------------+
|               Jenkins                 |
|  CI/CD Pipeline (Build, Test, Deploy) |
+---------------------------------------+
              |
              v
+---------------------------------------+
|               Docker Hub              |
|  Container Registry for App Images    |
+---------------------------------------+
              |
              v
+---------------------------------------+
|        Kubernetes Cluster (AWS)       |
|  Deployed using kubeadm on EC2        |
+---------------------------------------+
```

---

## How to Run Locally
```bash
# Clone the repository
git clone <repo-url>
cd BoardgameListingWebApp

# Build & Run the application
mvn clean package
java -jar target/*.jar
```

### Default Credentials
- **User:** `bugs` | **Password:** `bunny`
- **Manager:** `daffy` | **Password:** `duck`

---

## CI/CD Pipeline Flow
**Code Commit** → Trigger Jenkins pipeline via webhook  
**Build & Test** → Maven build & JUnit tests  
**Static Analysis** → SonarQube code quality check  
**Security Scan** → Trivy scan for vulnerabilities  
**Docker Image Build & Push** → Nexus artifact storage  
**Deployment on Kubernetes** → Using Helm charts  
**Monitoring Setup** → Prometheus & Grafana dashboards  

---

## Challenges & Lessons Learned
- **Kubeadm Setup Issues:** Resolved networking conflicts and ensured best practices for cluster security.
- **RBAC Misconfigurations:** Fixed role bindings for Jenkins to access Kubernetes securely.
- **Pipeline Failures:** Debugged issues related to missing dependencies and container image pushes.
- **Monitoring Optimization:** Configured Prometheus exporters for better observability.

---

## Credits & References
This project implementation was inspired by [Aditya](https://www.youtube.com/@DevOpsShack) from **DevOps Shack**. Watch the original tutorial here: [DevOps Shack YouTube Video](https://youtu.be/NnkUGzaqqOc?si=0CAMZW0DG0mlQQWP)

---

## Future Enhancements
- Automate infrastructure provisioning using Terraform.
- Implement logging with ELK Stack (Elasticsearch, Logstash, Kibana).
- Use ArgoCD for GitOps-driven deployments.

---

## Contributing
Pull requests are welcome! Feel free to raise issues for feature requests or bugs.

---

## Contact
Email: [Email Me](vishupatel575@gmail.com)
LinkedIn: [LinkedIn Profile](https://www.linkedin.com/in/vishu-patel/)  
GitHub: [GitHub Profile](https://github.com/VishuPatel-27)  

🔥 **Give this repo a ⭐ if you found it useful!**
