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
