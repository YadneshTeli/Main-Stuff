
# 🚀 Docker Static Web App Setup

This document outlines how to build and run a Docker container that serves a static HTML page using Nginx.

---

## 🐳 Dockerfile

```Dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

### 🔍 Description:

- **`FROM nginx:latest`**  
  Uses the official latest version of the Nginx image as the base.  
  Nginx is a lightweight web server used to serve the static website.

- **`COPY index.html /usr/share/nginx/html/index.html`**  
  Copies the local `index.html` file to the directory Nginx uses to serve content.

---

## 💻 Docker Commands

### 🔧 1. Build the Docker Image

```bash
docker build -t my-docker-webapp .
```

- Builds a Docker image from the current directory.
- Tags the image as `my-docker-webapp`.

---

### 📋 2. List Running Containers

```bash
docker ps
```

- Displays all currently running containers with their details:
  container ID, image, ports, and status.

---

### 🚀 3. Run the Docker Container

```bash
docker run -d -p 8081:80 --name webapp-container my-docker-webapp
```

- `-d`: Runs the container in detached (background) mode.
- `-p 8081:80`: Maps **host port 8081** to **container port 80**.
- `--name webapp-container`: Names the container for easier management.
- `my-docker-webapp`: The name of the Docker image you built.

🌐 After running this command, open your browser and go to:

**http://localhost:8081**  
to view your static web page.

---

## 🧼 Useful Docker Commands

- **Stop the container**:
  ```bash
  docker stop webapp-container
  ```

- **Remove the container**:
  ```bash
  docker rm webapp-container
  ```

- **View container logs**:
  ```bash
  docker logs webapp-container
  ```

---

## 📂 Recommended Project Structure

```
my-docker-webapp/
├── Dockerfile
├── index.html
└── docker-webapp.md
```

---

## ✅ Summary

You’ve successfully:
1. Created a Dockerfile using Nginx.
2. Built a Docker image for a static web app.
3. Launched a container to serve your HTML file using Nginx.
4. Accessed it at `http://localhost:8081`.

Happy Dockering! 🐳
