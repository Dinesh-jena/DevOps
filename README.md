# 🚀 DevOps Basics - EC2 & Docker Deployment Guide

This guide explains how to:

1. Deploy a simple **Node.js** application on an **AWS EC2 (Ubuntu)** server.
2. Create a **Docker Image** for the same application.

---

# 📚 Prerequisites

Before starting, make sure you have:

* AWS Account
* Docker Desktop Installed
* Node.js Application (`server.js`)
* `package.json`
* SSH Key Pair (`.pem`)

---

# Part 1 - Deploy Node.js Application on AWS EC2

## Step 1: Create an EC2 Instance

1. Login to the **AWS Management Console**.
2. Open the **EC2 Dashboard**.
3. Click **Launch Instance**.
4. Select **Ubuntu Server**.
5. Choose an instance type (Recommended: `t2.micro` - Free Tier).
6. Create or select an existing **Key Pair (.pem)**.
7. Configure the Security Group.

Allow the following ports:

| Port | Protocol   | Purpose                |
| ---- | ---------- | ---------------------- |
| 22   | SSH        | Remote Login           |
| 80   | HTTP       | Web Traffic (Optional) |
| 3000 | Custom TCP | Express.js Application |

Finally, launch the instance.

---

# Step 2: Connect to EC2

```bash
ssh -i "hallex-test-1.pem" ubuntu@<EC2-Public-IP>
```

Example

```bash
ssh -i "hallex-test-1.pem" ubuntu@ec2-13-234-19-20.ap-south-1.compute.amazonaws.com
```

---

# Step 3: Create Project Directory

```bash
mkdir express-project
```

Move inside the folder

```bash
cd express-project
```

Check current path

```bash
pwd
```

---

# Step 4: Install Node.js

Update package list

```bash
sudo apt update
```

Upgrade packages

```bash
sudo apt upgrade -y
```

Install Node.js

```bash
sudo apt install nodejs -y
```

Install npm

```bash
sudo apt install npm -y
```

Verify installation

```bash
node -v
npm -v
```

---

# Step 5: Copy Project Files to EC2

Copy **package.json**

```bash
scp -i "hallex-test-1.pem" ./package.json ubuntu@ec2-13-234-19-20.ap-south-1.compute.amazonaws.com:/home/ubuntu/express-project/package.json
```

Copy **package-lock.json**

```bash
scp -i "hallex-test-1.pem" ./package-lock.json ubuntu@ec2-13-234-19-20.ap-south-1.compute.amazonaws.com:/home/ubuntu/express-project/package-lock.json
```

Copy **server.js**

```bash
scp -i "hallex-test-1.pem" ./server.js ubuntu@ec2-13-234-19-20.ap-south-1.compute.amazonaws.com:/home/ubuntu/express-project/server.js
```

---

# Step 6: Verify Project Files

List files

```bash
ls
```

Check `server.js`

```bash
cat server.js
```

Check `package.json`

```bash
cat package.json
```

---

# Step 7: Install Dependencies

```bash
npm install
```

---

# Step 8: Start the Application

```bash
node server.js
```

Expected Output

```text
Server is running on port 3000
```

---

# Useful Ubuntu Commands

| Command             | Description            |
| ------------------- | ---------------------- |
| `pwd`               | Show current directory |
| `ls`                | List files and folders |
| `mkdir folder-name` | Create a directory     |
| `touch filename`    | Create a file          |
| `cd folder-name`    | Enter directory        |
| `cd ..`             | Go back one directory  |
| `rm filename`       | Delete a file          |
| `rm -r folder-name` | Delete a directory     |

---

# Useful Node.js Commands

| Command          | Description           |
| ---------------- | --------------------- |
| `node -v`        | Check Node.js version |
| `npm -v`         | Check npm version     |
| `npm install`    | Install dependencies  |
| `node server.js` | Start application     |

---

# Project Structure

```text
express-project/
│── package.json
│── package-lock.json
│── server.js
```

---

# EC2 Deployment Flow

```text
Local Machine
      │
      │  SCP
      ▼
AWS EC2 (Ubuntu)
      │
      │ npm install
      ▼
Node.js Application
      │
      ▼
Running on Port 3000
```

---

# Part 2 - Docker Basics

## Step 1: Install Docker Desktop

Download and install Docker Desktop.

Verify installation

```bash
docker --version
```

---

# Step 2: Create a Dockerfile

Create a file named **Dockerfile**

```dockerfile
FROM node:18

# Linux + Node.js Base Image

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY server.js ./

EXPOSE 3000

CMD ["node","server.js"]
```

---

# Dockerfile Explanation

| Instruction                | Description                    |
| -------------------------- | ------------------------------ |
| `FROM node:18`             | Uses Node.js 18 official image |
| `WORKDIR /app`             | Creates the working directory  |
| `COPY package*.json ./`    | Copies package files           |
| `RUN npm install`          | Installs dependencies          |
| `COPY server.js ./`        | Copies application source      |
| `EXPOSE 3000`              | Opens port 3000                |
| `CMD ["node","server.js"]` | Starts the application         |

---

# Step 3: Build Docker Image

```bash
docker build -t <image-name> .
```

Example

```bash
docker build -t halley .
```

### Command Explanation

| Option         | Description                        |
| -------------- | ---------------------------------- |
| `docker build` | Builds a Docker image              |
| `-t`           | Assigns a tag/name                 |
| `halley`       | Image name                         |
| `.`            | Current directory as build context |

After the build completes successfully, Docker creates an image and stores it locally.

---

# Step 4: Verify Docker Images

```bash
docker images
```

This command lists all Docker images available on your local machine.

*(Add your screenshot below if required.)*

```text
docker images
```

📷 Screenshot:

![Docker Images](https://github.com/user-attachments/assets/95508d49-7b20-4789-8c9b-1b6484d89dd1)

---

# Docker Workflow

```text
Project Files
      │
      ▼
Dockerfile
      │
docker build
      │
      ▼
Docker Image
      │
docker run
      │
      ▼
Docker Container
      │
      ▼
Node.js Application Running
```

---

# Next Steps

After creating the Docker image, you can:

* Run the image as a Docker Container.
* Push the image to Docker Hub.
* Pull the image on another machine.
* Deploy the container on an AWS EC2 server.
* Use Docker Compose for multi-container applications.
