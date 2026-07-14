1.EC2 SERVER SETUP 

# Deploying a Node.js Application on AWS EC2

## Step 1: Create an EC2 Instance

1. Log in to the AWS Management Console.
2. Open the **EC2 Dashboard**.
3. Click **Launch Instance**.
4. Choose **Ubuntu Server**.
5. Select an instance type (e.g., `t2.micro` for Free Tier).
6. Create or select an existing **Key Pair (.pem)**.
7. Configure the Security Group:

   * SSH (Port 22)
   * HTTP (Port 80) *(Optional)*
   * Custom TCP (Port 3000) *(For Express applications)*
8. Launch the instance.

---

# Step 2: Connect to the EC2 Instance

```bash
ssh -i "hallex-test-1.pem" ubuntu@<EC2-Public-IP>
```

Example:

```bash
ssh -i "hallex-test-1.pem" ubuntu@ec2-13-234-19-20.ap-south-1.compute.amazonaws.com
```

---

# Step 3: Create a Project Directory

```bash
mkdir express-project
```

Move into the project folder:

```bash
cd express-project
```

Check the current directory:

```bash
pwd
```

---

# Step 4: Install Node.js

Update the package list:

```bash
sudo apt update
```

Upgrade installed packages:

```bash
sudo apt upgrade -y
```

Install Node.js:

```bash
sudo apt install nodejs -y
```

Install npm:

```bash
sudo apt install npm -y
```

Verify installation:

```bash
node -v
npm -v
```

---

# Step 5: Copy Project Files from Local Machine to EC2

Copy `package.json`:

```bash
scp -i "hallex-test-1.pem" ./package.json ubuntu@ec2-13-234-19-20.ap-south-1.compute.amazonaws.com:/home/ubuntu/express-project/package.json
```

Copy `package-lock.json` (if available):

```bash
scp -i "hallex-test-1.pem" ./package-lock.json ubuntu@ec2-13-234-19-20.ap-south-1.compute.amazonaws.com:/home/ubuntu/express-project/package-lock.json
```

Copy `server.js`:

```bash
scp -i "hallex-test-1.pem" ./server.js ubuntu@ec2-13-234-19-20.ap-south-1.compute.amazonaws.com:/home/ubuntu/express-project/server.js
```

---

# Step 6: Verify Files on EC2

List files:

```bash
ls
```

View the contents of `server.js`:

```bash
cat server.js
```

View the contents of `package.json`:

```bash
cat package.json
```

---

# Step 7: Install Project Dependencies

```bash
npm install
```

---

# Step 8: Run the Application

```bash
node server.js
```

If everything is correct, you should see:

```text
Server is running on port 3000
```

---

# Useful Ubuntu Commands

Check current directory:

```bash
pwd
```

Create a new folder:

```bash
mkdir folder-name
```

Create a new file:

```bash
touch filename
```

List files and folders:

```bash
ls
```

Change directory:

```bash
cd folder-name
```

Go back one directory:

```bash
cd ..
```

Remove a file:

```bash
rm filename
```

Remove a folder:

```bash
rm -r folder-name
```

---

# Useful Node.js Commands

Check Node.js version:

```bash
node -v
```

Check npm version:

```bash
npm -v
```

Install project dependencies:

```bash
npm install
```

Start the application:

```bash
node server.js
```

---

# Project Structure

```text
express-project/
│── package.json
│── package-lock.json
│── server.js
```

---

# Deployment Flow

```text
Local Machine
      │
      │  scp
      ▼
AWS EC2 (Ubuntu)
      │
      │  npm install
      ▼
Node.js Application
      │
      ▼
Server Running on Port 3000
```



2.DOCKER 
# Docker - Basic Node.js Application

## Step 1: Install Docker Desktop
- Download and install Docker Desktop.
- Complete the setup and make sure Docker Desktop is running.

---

## Step 2: Create a Dockerfile

Create a file named `Dockerfile` (without any extension) and add the following code:

```dockerfile
FROM node:18
# Base image (Linux OS + Node.js)

WORKDIR /app

COPY package*.json ./
# COPY package-lock.json ./

RUN npm install

COPY server.js ./

EXPOSE 3000

CMD ["node", "server.js"]
```

---

## Explanation

- `FROM node:18` → Uses the official Node.js 18 image.
- `WORKDIR /app` → Sets the working directory inside the container.
- `COPY package*.json ./` → Copies `package.json` and `package-lock.json`.
- `RUN npm install` → Installs all dependencies.
- `COPY server.js ./` → Copies the application file.
- `EXPOSE 3000` → Exposes port 3000.
- `CMD ["node", "server.js"]` → Starts the Node.js application.

---

## Step 3: Build Docker Image

Run the following command to create a Docker image:

```bash
docker build -t <image-name> .
```

Example:

```bash
docker build -t halley .
```

### Command Breakdown

- `docker build` → Builds a Docker image.
- `-t` → Assigns a name (tag) to the image.
- `<image-name>` → Your custom image name (e.g., `halley`).
- `.` → Uses the current directory as the build context.

After the build is successful, a Docker image is created and stored in your local Docker images.

---

## Step 4: Verify the Image
<img width="737" height="491" alt="image" src="https://github.com/user-attachments/assets/95508d49-7b20-4789-8c9b-1b6484d89dd1" />

```bash
docker images
```

This command displays all Docker images stored on your system.
