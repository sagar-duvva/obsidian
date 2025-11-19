Got it — I can generate a **Dockerfile Mastery — Beginner to Advanced Cheat Sheet** for you in Markdown so you can drop it straight into Obsidian.

Here’s the compact, structured cheat sheet:

---

# 🐳 Dockerfile Mastery — Beginner to Advanced Cheat Sheet

## 1️⃣ Basics

|Instruction|Purpose|Example|
|---|---|---|
|`FROM`|Base image|`FROM ubuntu:20.04`|
|`RUN`|Execute command in new layer|`RUN apt-get update && apt-get install -y curl`|
|`CMD`|Default command|`CMD ["node", "app.js"]`|
|`ENTRYPOINT`|Always executed|`ENTRYPOINT ["python3", "script.py"]`|
|`COPY`|Copy files from host|`COPY . /app`|
|`ADD`|Copy + URL/tar extraction|`ADD app.tar.gz /app`|
|`WORKDIR`|Set working directory|`WORKDIR /app`|
|`ENV`|Set environment variable|`ENV PORT=8080`|
|`EXPOSE`|Document ports|`EXPOSE 8080`|
|`VOLUME`|Mount volume|`VOLUME ["/data"]`|

---

## 2️⃣ Intermediate

|Feature|Purpose|Example|
|---|---|---|
|`.dockerignore`|Ignore files|`node_modules`, `.git`|
|`ARG`|Build-time variable|`ARG VERSION=1.0`|
|`HEALTHCHECK`|Container health|`HEALTHCHECK CMD curl -f [http://localhost/](http://localhost/)|
|Shell vs Exec form|CMD/ENTRYPOINT syntax|`CMD ["echo", "hi"]` (exec)|
|Multiple `RUN` commands|Combine for fewer layers|`RUN apt-get update && apt-get install -y git`|

---

## 3️⃣ Advanced

|Feature|Purpose|Example|
|---|---|---|
|**Multi-stage builds**|Smaller images|`FROM golang AS build` ... `FROM alpine` ...|
|**Caching & layers**|Speed builds|Order instructions by frequency of change|
|**Secrets (BuildKit)**|Hide creds|`RUN --mount=type=secret,id=mysecret`|
|**Build args**|Dynamic builds|`docker build --build-arg VERSION=2.0 .`|
|**BuildKit features**|Parallel builds, mounts|`RUN --mount=type=cache,target=/root/.cache`|
|**Image size optimization**|Alpine base, strip deps|`FROM python:3.11-slim`|
|**Provenance & security**|SBOM, `docker scan`|`docker buildx build --sbom`|
|**Advanced patterns**|Onbuild, conditional COPY|`ONBUILD COPY . /src`|
|**Troubleshooting**|Debug layers|`docker run -it --entrypoint sh image`|

---

## 4️⃣ Best Practices

- Use smallest base image possible (`alpine`, `slim`).
    
- Combine `RUN` commands to reduce layers.
    
- Order instructions to maximize cache hits.
    
- Use `.dockerignore` to avoid copying unnecessary files.
    
- Pin versions for reproducibility.
    
- Avoid secrets in Dockerfile (use BuildKit `--secret`).
    
- Use healthchecks for production containers.
    
- Regularly scan images for vulnerabilities.
    
- Prefer multi-stage builds for compiled languages.
    
- Keep ENTRYPOINT for main process, CMD for defaults.
    

---

## 5️⃣ Quick Commands

```bash
# Build image
docker build -t myimage .

# Build with args
docker build --build-arg VERSION=1.2 -t myimage .

# Enable BuildKit
DOCKER_BUILDKIT=1 docker build .

# Run with mount
docker run -it --mount type=bind,source=$(pwd),target=/app myimage

# Debug
docker run -it --entrypoint sh myimage
```

---

If you want, I can **expand this cheat sheet with code-rich, real-world examples** for each advanced topic like _Multi-stage builds_, _Secrets_, _Optimization_, etc., so you’ll have a single **ready-to-run reference** instead of just quick syntax. That would make it much more than just a memory aid.

Do you want me to prepare that **extended real-world Dockerfile cheat sheet** next?