# Java Docker Hacker Snake Game

A complete hands-on project demonstrating how to run a Java Spring Boot application inside Docker and deploy it on an AWS EC2 instance.

This project showcases a cyberpunk-style Hacker Snake game running inside a Docker container with a modern multi-stage Docker build setup. 

---

# 📄 Documentation

Detailed step-by-step documentation with screenshots is available inside the `docs/` folder.

Document Name:

* `10.Java_Docker_Hacker_Snake_Guide.docx`

Repository Structure:

```text id="5f52eu"
project-folder/
│
├── README.md
│
└── docs/
    └── 10.Java_Docker_Hacker_Snake_Guide.docx
```

---

# 🚀 Project Overview

This project demonstrates:

* Cloning a Java Spring Boot project from GitHub
* Building the application using Docker
* Running the container on AWS EC2
* Accessing the application through a browser
* Understanding multi-stage Docker builds

The application includes:

* Matrix rain animation
* Neon hacker UI
* Snake gameplay
* Score tracking
* Level progression
* Particle effects
* Keyboard and on-screen controls 

---

# 🛠️ Tech Stack

* Java 21
* Spring Boot 3
* Maven
* Docker
* AWS EC2

GitHub Repository:

```text id="k2p1t3"
https://github.com/rohith2002/java-docker-app
```

---

# 📋 Prerequisites

Before starting, ensure you have:

* AWS EC2 Ubuntu instance
* Docker installed
* Git installed
* Port 8080 open in Security Group inbound rules 

---

# 🏗️ Project Workflow

## Step 1 — Clone Repository

SSH into your EC2 instance and clone the project:

```bash id="pjkpnq"
git clone https://github.com/rohith2002/java-docker-app.git
```

Verify folder creation:

```bash id="5x6ym2"
ls
```

---

## Step 2 — Verify Project Structure

Navigate into the project directory:

```bash id="m1bl33"
cd java-docker-app
```

Check the Maven project structure:

```bash id="vf2s4q"
tree .
```

Expected files:

```text id="7jk4sk"
Dockerfile
pom.xml
src/main/java/com/example/App.java
```

---

# 🐳 Understanding the Dockerfile

This project uses a **multi-stage Docker build**.

### Stage 1 — Build

Uses:

```dockerfile id="7g78lg"
maven:3.9 + JDK 21
```

Purpose:

* Download dependencies
* Compile source code
* Package application JAR

### Stage 2 — Runtime

Uses:

```dockerfile id="ahpqeh"
eclipse-temurin:21-jre-alpine
```

Purpose:

* Run the compiled JAR
* Keep image lightweight
* Improve security

---

# ✅ Benefits of Multi-Stage Builds

| Single Stage Build    | Multi-Stage Build |
| --------------------- | ----------------- |
| Large image size      | Smaller image     |
| Includes Maven + JDK  | Only JRE          |
| Source code exposed   | Source hidden     |
| Slower deployment     | Faster deployment |
| Higher attack surface | More secure       |

The final image size is reduced significantly using multi-stage builds. 

---

# 🐳 Build Docker Image

Run the following command inside the project folder:

```bash id="v2e6o0"
docker build -t java-docker-app .
```

Docker will:

* Build the Maven project
* Package the JAR
* Create the final runtime image

---

# ▶️ Run Docker Container

Start the container:

```bash id="x3w5mr"
docker run -itd --name java-cont -p 8080:8080 java-docker-app
```

Verify running containers:

```bash id="jmx7x4"
docker ps
```

---

# 🌐 Access the Application

Open your browser:

```text id="jwn3bs"
http://<your-ec2-public-ip>:8080
```

If configured correctly, the Hacker Snake boot screen will appear. 

---

# 🎮 Game Features

The Hacker Snake game includes:

* Matrix-style background animation
* Neon cyberpunk design
* WASD controls
* Arrow key controls
* On-screen mobile controls
* Score tracking
* Speed increase per level
* Particle burst effects
* Game reboot functionality 

---

# 🐳 Useful Docker Commands

## View Running Containers

```bash id="pbp1c1"
docker ps
```

---

## View Container Logs

```bash id="vz5s3p"
docker logs -f java-cont
```

---

## Stop and Remove Container

```bash id="a1f98g"
docker stop java-cont && docker rm java-cont
```

---

## Rebuild After Code Changes

```bash id="5x29fp"
docker stop java-cont && docker rm java-cont
docker build -t java-docker-app .
docker run -itd --name java-cont -p 8080:8080 java-docker-app
```

---

## List Docker Images

```bash id="pw65u7"
docker images
```

---

# ☁️ AWS Services Used

* Amazon EC2
* Security Groups
* Docker
* Linux (Ubuntu)

---

# 📚 Learning Outcomes

After completing this project, you will understand:

* Java Spring Boot containerization
* Docker image creation
* Multi-stage Docker builds
* Running containers on AWS EC2
* Port mapping in Docker
* Docker container lifecycle
* Browser-based application deployment

---

# 🔐 Security Notes

* Open only required ports
* Use lightweight runtime images
* Avoid exposing source code in production images
* Multi-stage builds reduce attack surface

---

# 👨‍💻 Author

Rohith

---

# ⭐ Summary

This project demonstrates a real-world Java Docker deployment workflow using:

* Spring Boot
* Docker
* AWS EC2
* Multi-stage builds

The application is containerized, lightweight, secure, and accessible through a web browser running on port `8080`. 
