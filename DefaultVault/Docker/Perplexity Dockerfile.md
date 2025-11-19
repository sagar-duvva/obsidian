# **Complete Guide to Writing Dockerfiles (Beginner to Advanced)**

A **Dockerfile** is a script that contains instructions to assemble a Docker image. Let’s break down the process from basics to advanced, offering 5 examples for each area so you can learn by practice.

---

## **Beginner Level**

## **1. Key Concepts and Prerequisites**

- **Docker Image:** A snapshot of what will run in a container.
    
- **Docker Container:** A running instance of an image.
    
- **Dockerfile Syntax:** Text file with sequential instructions.
    

## **2. Essential Dockerfile Instructions**

|Instruction|Description|
|---|---|
|`FROM`|Base image for building your image|
|`RUN`|Execute commands during the build|
|`CMD`|Default command when container runs|
|`COPY`|Copies files from local to image|
|`WORKDIR`|Sets working directory for commands|

## **3. Beginner Examples**

```
# Example 1: Hello World
FROM ubuntu:latest
RUN echo "Hello, Docker!"
CMD ["echo", "Hello from container!"]

# Example 2: Python Script
FROM python:3.10
COPY script.py /script.py
CMD ["python", "/script.py"]

# Example 3: Simple Web Server (nginx)
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html

# Example 4: Set Working Directory
FROM node:18
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["node", "index.js"]

# Example 5: Multiple RUN Commands
FROM alpine:3.18
RUN apk update && apk add curl
RUN echo "Setup complete"
CMD ["sh"]

```

---

## **Intermediate Level**

## **1. ENV, EXPOSE, ENTRYPOINT**

|Instruction|Description|
|---|---|
|`ENV`|Set environment variables|
|`EXPOSE`|Inform Docker which port to expose|
|`ENTRYPOINT`|Specify executable for container|

## **2. File Organization and Multi-Stage Builds**

- Multi-stage builds help create small, secure final images by discarding unnecessary build artifacts.
    

## **3. Intermediate Examples**


```
# Example 1: Using ENV
FROM alpine
ENV APP_HOME /app
WORKDIR $APP_HOME
COPY . .
CMD ["ls"]

# Example 2: Expose Port
FROM python:3.11
EXPOSE 5000
COPY app.py /app.py
CMD ["python", "/app.py"]

# Example 3: ENTRYPOINT vs CMD
FROM busybox
ENTRYPOINT ["echo"]
CMD ["Hello World"]

# Example 4: Multi-Stage Build (Node+nginx)
FROM node:18 AS builder
WORKDIR /build
COPY . .
RUN npm install && npm run build
FROM nginx:alpine
COPY --from=builder /build/dist /usr/share/nginx/html

# Example 5: COPY with Directory
FROM ubuntu:latest
WORKDIR /data
COPY src/ /data/
CMD ["ls", "/data"]

```


---

## **Advanced Level**

## **1. Build Caching, Layers, and Optimizations**

- **Layer Caching:** Docker caches layers to speed up builds.
    
- **Optimize Order:** COPY and RUN earlier to leverage cache.
    

## **2. Healthchecks, Labels, Volumes**

|Instruction|Description|
|---|---|
|`LABEL`|Metadata for image (maintainer, version, etc.)|
|`HEALTHCHECK`|Define health command for the container|
|`VOLUME`|Define mount point for container data|

## **3. Advanced Examples**


```
# Example 1: HEALTHCHECK
FROM nginx
HEALTHCHECK --interval=30s --timeout=10s \
  CMD curl -f http://localhost/ || exit 1

# Example 2: LABEL
FROM python:3.9
LABEL maintainer="your@email.com"
LABEL version="1.0"
COPY app.py /app.py
CMD ["python", "/app.py"]

# Example 3: VOLUME for Persistent Data
FROM mysql:8.0
VOLUME /var/lib/mysql
ENV MYSQL_ROOT_PASSWORD=rootpass

# Example 4: Optimized Build Order
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "server.js"]

# Example 5: ARG for Build Vars
FROM alpine
ARG VERSION=latest
RUN echo ${VERSION}

```


---

## ## **Expert Level**

## **1. Multi-Stage Builds, Security, Entrypoint Scripts, Permissions**

- Multi-Stage: Build, then copy final artifact into minimal image.
    
- Entrypoint Scripts: Custom startup logic.
    
- User: Set container user for security.
    
- Scratch: Minimal base for deploys.
    
- Secrets: Pass sensitive data securely.

```
# Example 1: Build from Source (Golang)
FROM golang:1.20 AS builder
WORKDIR /src
COPY . .
RUN go build -o app
FROM alpine
COPY --from=builder /src/app /app
CMD ["/app"]

# Example 2: Entrypoint Script
FROM ubuntu
COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# Example 3: User & Permission
FROM nginx
RUN adduser -D myuser
USER myuser

# Example 4: Minimal Image (scratch)
FROM scratch
COPY app /app
CMD ["/app"]

# Example 5: Build Args & Secrets
FROM ubuntu
ARG GIT_TOKEN
RUN git clone https://github.com/user/repo.git --branch main --single-branch

```


---

## **Key Tips for Mastery**

- **Start simple**: Master `FROM`, `RUN`, `CMD`, and `COPY` first.
    
- **Read official docs**: Docker’s documentation is detailed and up to date.
    
- **Practice multi-stage builds**: Essential for production.
    
- **Understand containerization best practices**: Minimal images, use environment variables, avoid secrets in Dockerfile.
    
- **Test frequently**: Use `docker build` and `docker run` for each new Dockerfile.
    

---

## **Useful References**

- See Docker's [official documentation](https://docs.docker.com/engine/reference/builder/) for exhaustive command and best practices.
    
- Try out the examples above to solidify each concept: you can modify them and experiment to understand how Dockerfile instructions affect your containers.
    

---

**Ready to start? Begin by writing your first Dockerfile using the beginner examples, and level-up your skills by implementing multi-stage builds, custom entrypoints, and security measures as your comfort grows!**