Alright — here’s your **Dockerfile Mastery — Beginner to Advanced Cheat Sheet** in **Obsidian-ready Markdown** with everything in one place.  
I’ve kept it **expanded enough to learn from** while still being quick to scan.

---

# Dockerfile Mastery — Beginner to Advanced (Cheat Sheet)

## 1. Basics

### `FROM` — Base Image

Defines the base image for your container.

```dockerfile
FROM ubuntu:22.04
```

- Can use `scratch` for minimal images.
    
- Pin versions for reproducibility.
    

---

### `LABEL` — Metadata

```dockerfile
LABEL maintainer="you@example.com"
LABEL version="1.0"
LABEL description="Sample App"
```

---

### `RUN` — Execute Commands at Build Time

```dockerfile
RUN apt-get update && apt-get install -y curl
```

- Combine commands to reduce layers.
    
- Clean up temporary files to save space.
    

---

### `COPY` vs `ADD`

```dockerfile
COPY ./app /app
ADD file.tar.gz /app/
```

- `COPY` → simple copy.
    
- `ADD` → copy + auto-extract for archives + remote URLs.
    

---

### `WORKDIR` — Set Working Directory

```dockerfile
WORKDIR /app
```

---

### `CMD` vs `ENTRYPOINT`

```dockerfile
CMD ["node", "server.js"]       # Default command
ENTRYPOINT ["python3", "app.py"] # Fixed executable
```

---

## 2. Intermediate

### `ENV` — Environment Variables

```dockerfile
ENV APP_ENV=production
```

---

### `EXPOSE` — Document Ports

```dockerfile
EXPOSE 8080
```

- Doesn’t actually publish — use `docker run -p`.
    

---

### `VOLUME` — Persistent Storage

```dockerfile
VOLUME ["/data"]
```

---

### `HEALTHCHECK` — Container Health

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s \
  CMD curl -f http://localhost:8080/ || exit 1
```

---

## 3. Advanced

### Multi-Stage Builds

Reduce image size by building in stages.

```dockerfile
# Builder
FROM golang:1.20 AS builder
WORKDIR /src
COPY . .
RUN go build -o app .

# Final
FROM alpine:3.18
COPY --from=builder /src/app /app
CMD ["/app"]
```

---

### Caching & Layer Optimization

```dockerfile
# Install deps first for cache efficiency
COPY package*.json .
RUN npm install
COPY . .
RUN npm run build
```

- Put frequently changing files at the bottom.
    

---

### Build Arguments

```dockerfile
ARG VERSION=latest
RUN echo "Building version $VERSION"
```

Pass at build:

```bash
docker build --build-arg VERSION=1.0 .
```

---

### Secrets (BuildKit)

```dockerfile
# syntax=docker/dockerfile:1.4
RUN --mount=type=secret,id=mysecret cat /run/secrets/mysecret
```

Pass secret:

```bash
docker build --secret id=mysecret,src=./secret.txt .
```

---

### BuildKit Features

```dockerfile
# syntax=docker/dockerfile:1.4
RUN --mount=type=cache,target=/root/.cache go build
```

- Caching, secrets, inline mounts.
    

---

### Image Size Optimization

- Use `alpine` or `distroless`.
    
- Multi-stage builds.
    
- Clean temp files in same `RUN`.
    

---

### Provenance & Security Best Practices

- Pin versions (`FROM node:18.16.0-alpine`).
    
- Use `USER` non-root.
    
- Scan images (`docker scan` / `trivy`).
    

---

### Advanced Patterns

#### Minimal Final Image

```dockerfile
FROM scratch
COPY mybinary /
ENTRYPOINT ["/mybinary"]
```

#### Dynamic Entrypoint Script

```dockerfile
COPY entrypoint.sh /usr/local/bin/
ENTRYPOINT ["entrypoint.sh"]
```

---

### Troubleshooting & Debugging

- Use `docker run -it --entrypoint sh image` to debug.
    
- Add temporary `RUN sleep 9999` in Dockerfile.
    
- Use `docker history image` to inspect layers.
    

---

## 4. Common Best Practices Summary

- **One concern per layer**.
    
- **Cache smartly**: dependencies first, source code later.
    
- **Pin versions** for reproducibility.
    
- **Minimize layers**.
    
- **Use non-root user**.
    
- **Scan images** for vulnerabilities.
    
- **Keep secrets out of the image**.
    

---

Do you want me to now **extend this cheat sheet** with  
✅ **10 real-world examples per advanced topic** (multi-stage, caching, secrets, etc.) — so you’ll have an _applied cookbook_ right inside Obsidian? That would turn this into a full practical guide.