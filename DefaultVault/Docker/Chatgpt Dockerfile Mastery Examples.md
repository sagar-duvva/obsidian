Great — I’ll add a complete, Obsidian-ready Markdown section for each of the topics you listed. Each topic includes a concise explanation plus **10 real‑world examples**, and every example includes the relevant Dockerfile (or build) code so you can copy/paste and run them. I’ll keep examples practical and compact so they’re easy to test.

I’ll produce the sections in the same style as your existing doc so you can paste them directly into **“Dockerfile Mastery — Beginner To Advanced (obsidian Markdown)”**.

---

## 4. Multi-stage builds — explanation + 10 real-world examples

**Concept (short):** Multi‑stage builds let you perform expensive compile/build steps in intermediate stages, then copy only the minimal runtime artifacts into the final image. This reduces image size and keeps build tools out of production images.

**When to use:** compile languages (Go, Rust, C++), build frontend assets, compile JAR/WAR, produce single binary images, create artifacts for multi-arch, remove dev-only dependencies.

### Examples

1. **Go — build then small runtime (Alpine)**
    

```Dockerfile
# Dockerfile
FROM golang:1.21 AS builder
WORKDIR /src
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o app ./cmd/app

FROM alpine:3.19
COPY --from=builder /src/app /usr/local/bin/app
ENTRYPOINT ["/usr/local/bin/app"]
```

2. **Node (webpack) — build assets then nginx**
    

```Dockerfile
# Dockerfile
FROM node:20 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
```

3. **Java — Maven build -> JRE runtime**
    

```Dockerfile
# Dockerfile
FROM maven:3.9 AS builder
WORKDIR /build
COPY pom.xml .
COPY src ./src
RUN mvn -DskipTests package

FROM eclipse-temurin:17-jre
COPY --from=builder /build/target/myapp.jar /app/myapp.jar
ENTRYPOINT ["java","-jar","/app/myapp.jar"]
```

4. **Python — wheel build -> slim image with only wheels installed**
    

```Dockerfile
# Dockerfile
FROM python:3.11-slim AS builder
WORKDIR /build
COPY pyproject.toml setup.cfg ./
RUN pip wheel --no-deps --wheel-dir /wheels .

FROM python:3.11-slim
COPY --from=builder /wheels /wheels
RUN pip install --no-index --find-links=/wheels mypackage
COPY app /app
CMD ["python","/app/main.py"]
```

5. **Rust — release binary -> minimal scratch image**
    

```Dockerfile
# Dockerfile
FROM rust:1.71 as builder
WORKDIR /src
COPY . .
RUN cargo build --release

FROM debian:stable-slim
COPY --from=builder /src/target/release/mybin /usr/local/bin/mybin
ENTRYPOINT ["/usr/local/bin/mybin"]
```

6. **C/C++ — build with GCC, runtime packaged to Alpine**
    

```Dockerfile
# Dockerfile
FROM gcc:12 AS builder
WORKDIR /src
COPY . .
RUN g++ -O2 -static main.cpp -o app

FROM scratch
COPY --from=builder /src/app /app
ENTRYPOINT ["/app"]
```

7. **Frontend monorepo — build only selected package**
    

```Dockerfile
# Dockerfile
FROM node:20 AS builder
WORKDIR /repo
COPY package.json pnpm-lock.yaml ./
COPY packages/app ./packages/app
RUN cd packages/app && npm ci && npm run build

FROM nginx:alpine
COPY --from=builder /repo/packages/app/dist /usr/share/nginx/html
```

8. **Dotnet — SDK build -> runtime image**
    

```Dockerfile
# Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY . .
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:7.0
COPY --from=build /app/publish /app
ENTRYPOINT ["dotnet","/app/MyApp.dll"]
```

9. **Machine learning model packaging — build artifact then small runtime**
    

```Dockerfile
# Dockerfile
FROM python:3.10 AS builder
WORKDIR /build
COPY requirements.txt .
RUN pip wheel --wheel-dir /wheels -r requirements.txt
COPY model.pt /build/

FROM python:3.10-slim
COPY --from=builder /wheels /wheels
RUN pip install --no-index --find-links=/wheels -r /wheels/requirements.txt
COPY --from=builder /build/model.pt /app/model.pt
COPY serve.py /app/serve.py
CMD ["python","/app/serve.py"]
```

10. **Multi-target binaries — build for multiple platforms in one Dockerfile (with buildx)**
    

```Dockerfile
# Dockerfile (use with docker buildx build --platform linux/amd64,linux/arm64)
FROM --platform=$BUILDPLATFORM golang:1.21 AS builder
WORKDIR /src
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=${TARGETARCH} go build -o /out/app

FROM scratch
COPY --from=builder /out/app /app
ENTRYPOINT ["/app"]
```

---

## 5. Caching, layers & optimization — explanation + 10 real-world examples

**Concept (short):** Docker builds produce image _layers_ — each instruction creates a layer. Docker caches layers and will reuse them when the instruction and its context haven't changed. Optimizing layer order and content drastically speeds up builds and reduces final image size.

**Key techniques:** re-order Dockerfile (install deps first, copy sources later), reduce number of layers (combine RUN lines), use `.dockerignore`, use BuildKit cache mounts, `--cache-from` in CI.

### Examples

1. **Node: cache `npm ci` by copying package files first**
    

```Dockerfile
# Dockerfile
FROM node:20
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --production
COPY . .
CMD ["node","server.js"]
```

2. **Python: cache `pip install` by copying requirements first**
    

```Dockerfile
# Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python","app.py"]
```

3. **Combine apt installs to reduce layers and cleanup apt lists**
    

```Dockerfile
# Dockerfile
FROM ubuntu:24.04
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl ca-certificates build-essential \
 && rm -rf /var/lib/apt/lists/*
```

4. **Use `.dockerignore` to prevent copying `node_modules`, `.git`, big files**
    

```
# .dockerignore
node_modules
.git
dist
Dockerfile
*.log
```

5. **Use BuildKit cache mount for pip cache**
    

```dockerfile
# Dockerfile (requires BuildKit)
# syntax=docker/dockerfile:1.4
FROM python:3.11
WORKDIR /app
COPY requirements.txt .
RUN --mount=type=cache,target=/root/.cache/pip pip install -r requirements.txt
COPY . .
```

6. **Cache Maven repo to speed Java builds**
    

```Dockerfile
# Dockerfile (requires BuildKit)
# syntax=docker/dockerfile:1.4
FROM maven:3.9 AS build
WORKDIR /app
COPY pom.xml .
RUN --mount=type=cache,target=/root/.m2 mvn -q dependency:go-offline
COPY src ./src
RUN mvn -DskipTests package
```

7. **Minimize final image by deleting build-time tools in same RUN**
    

```Dockerfile
# Dockerfile
FROM ubuntu:24.04
RUN apt-get update && apt-get install -y build-essential \
 && make build \
 && apt-get purge -y --auto-remove build-essential \
 && rm -rf /var/lib/apt/lists/*
```

8. **Avoid cache-busting by not doing `ADD https://...` (remote)**
    

```Dockerfile
# BAD: causes rebuilds whenever remote changes
ADD https://example.com/somefile /opt/
# BETTER: vendor file into build context and COPY it
```

9. **Use `--cache-from` in CI to reuse previously pushed image layers**
    

```bash
# CI pipeline snippet
docker pull myrepo/myapp:cache || true
docker build --cache-from myrepo/myapp:cache -t myrepo/myapp:latest .
docker push myrepo/myapp:latest
```

10. **Reduce number of layers by grouping file ops**
    

```Dockerfile
FROM alpine
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --production
COPY src ./src
# instead of multiple COPY or RUN lines across many layers
```

---

## 6. Secrets, build args and BuildKit features — explanation + 10 real-world examples

**Concept (short):**

- `ARG` are build-time variables (not persisted unless promoted to `ENV`).
    
- Secrets should never be baked into images. Use BuildKit’s secret/ssh mounts to access secrets during build without leaving them in image history.
    
- BuildKit adds `--mount=type=cache`, `--mount=type=secret`, `--mount=type=ssh`, `TARGETPLATFORM` and better parallel build performance.
    

### Examples (with code)

1. **Basic build arg for selecting base**
    

```Dockerfile
# Dockerfile
ARG NODE_VERSION=20
FROM node:${NODE_VERSION}
WORKDIR /app
COPY . .
RUN npm ci
```

Build:

```bash
docker build --build-arg NODE_VERSION=18 -t mynode .
```

2. **Use ARG to pass a Git commit SHA into LABEL**
    

```Dockerfile
ARG VCS_REF
FROM alpine
LABEL org.opencontainers.image.revision=$VCS_REF
```

Build:

```bash
docker build --build-arg VCS_REF=$(git rev-parse --short HEAD) -t myapp:sha .
```

3. **BuildKit secret to access private package registry token**
    

```dockerfile
# Dockerfile (top: use BuildKit syntax)
# syntax=docker/dockerfile:1.4
FROM node:20
WORKDIR /app
COPY package.json ./
RUN --mount=type=secret,id=npm_token \
  sh -c 'export NPM_TOKEN=$(cat /run/secrets/npm_token) && npm ci'
COPY . .
```

Build:

```bash
DOCKER_BUILDKIT=1 docker build --secret id=npm_token,src=token.txt -t myapp .
```

4. **Use BuildKit SSH mount to clone private repo**
    

```dockerfile
# syntax=docker/dockerfile:1.4
FROM alpine/git AS fetch
WORKDIR /src
RUN --mount=type=ssh git clone git@github.com:org/private-repo.git
```

Build:

```bash
DOCKER_BUILDKIT=1 docker build --ssh default .
```

5. **Use cache mount for pip to accelerate installs**
    

```dockerfile
# syntax=docker/dockerfile:1.4
FROM python:3.11
WORKDIR /app
COPY requirements.txt .
RUN --mount=type=cache,target=/root/.cache/pip pip install -r requirements.txt
COPY . .
```

6. **Pass secret at build but not leave it in image (read only during RUN)**
    

```dockerfile
# syntax=docker/dockerfile:1.4
FROM alpine
RUN --mount=type=secret,id=dbpass \
  sh -c 'dbpass=$(cat /run/secrets/dbpass) && ./fetch-from-db --password "$dbpass"'
```

Build:

```bash
DOCKER_BUILDKIT=1 docker build --secret id=dbpass,src=secret.txt .
```

7. **ARG for toggling debug flags (not persisted)**
    

```Dockerfile
ARG ENABLE_DEBUG=false
FROM node:20
WORKDIR /app
COPY . .
RUN if [ "$ENABLE_DEBUG" = "true" ]; then npm run build:debug; else npm run build; fi
```

8. **Use `--mount=type=cache` for npm cache**
    

```dockerfile
# syntax=docker/dockerfile:1.4
FROM node:20
WORKDIR /app
COPY package.json package-lock.json ./
RUN --mount=type=cache,target=/root/.npm npm ci
COPY . .
```

9. **Multi-stage: BuildKit export of intermediate artifact without pushing an image**
    

```bash
# example CLI with BuildKit: build and export artifact
DOCKER_BUILDKIT=1 docker build --target builder --output type=local,dest=out .
```

(Dockerfile should have a `builder` stage that outputs built files.)

10. **Use TARGETPLATFORM/TARGETARCH with ARGs for cross-compiling**
    

```dockerfile
# Dockerfile
ARG TARGETOS
ARG TARGETARCH
FROM --platform=${TARGETOS}/${TARGETARCH} golang:1.21 AS builder
WORKDIR /src
COPY . .
RUN GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -o /out/app
```

Build with buildx for specific arch/platform.

---

## 7. Image size, provenance & security best practices — explanation + 10 real-world examples

**Concept (short):** Reduce attack surface and image size, ensure provenance (who built what and when), and apply security hygiene (scanning, signing, least privilege).

**Key practices:** minimal base images, multi-stage builds, remove package managers and caches, pin versions, scan images (Trivy/clair), sign images (cosign), add provenance labels (`org.opencontainers.image.*`), run as nonroot, use immutable base digests.

### Examples

1. **Use minimal base (distroless)**
    

```Dockerfile
FROM golang:1.21 AS builder
WORKDIR /src
COPY . .
RUN CGO_ENABLED=0 go build -o /app

FROM gcr.io/distroless/static
COPY --from=builder /app /app
ENTRYPOINT ["/app"]
```

2. **Add provenance labels (build date, git sha)**
    

```Dockerfile
ARG BUILD_DATE
ARG VCS_REF
FROM alpine
LABEL org.opencontainers.image.created=$BUILD_DATE \
      org.opencontainers.image.revision=$VCS_REF \
      org.opencontainers.image.source="https://github.com/you/repo"
```

Build:

```bash
docker build --build-arg BUILD_DATE="$(date -u +%Y-%m-%dT%H:%M:%SZ)" \
             --build-arg VCS_REF=$(git rev-parse --short HEAD) -t myapp .
```

3. **Remove package caches in one RUN and use `--no-install-recommends`**
    

```Dockerfile
FROM ubuntu:24.04
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates \
 && rm -rf /var/lib/apt/lists/*
```

4. **Scan image with Trivy in CI (example CI command)**
    

```bash
# CI snippet
trivy image --severity HIGH,CRITICAL myrepo/myapp:latest
```

5. **Sign image with cosign after push**
    

```bash
# sign
cosign sign --key cosign.key myrepo/myapp:latest
# verify
cosign verify --key cosign.pub myrepo/myapp:latest
```

6. **Run as non-root user in runtime**
    

```Dockerfile
FROM node:20
RUN useradd -m appuser
WORKDIR /home/appuser/app
COPY --chown=appuser:appuser . .
USER appuser
CMD ["node","server.js"]
```

7. **Pin base image using digest for immutable builds**
    

```Dockerfile
FROM ubuntu@sha256:0a1b2c3d4e...   # real digest
```

8. **Use minimal runtime and strip symbols for small binaries**
    

```Dockerfile
# For Go: build with -ldflags "-s -w"
FROM golang:1.21 AS builder
RUN go build -ldflags "-s -w" -o /app main.go
```

9. **Verify downloaded binary checksums before copying into image**
    

```Dockerfile
FROM alpine
RUN wget -O /tmp/binary.tar.gz https://example.com/binary.tar.gz \
 && echo "abc123  /tmp/binary.tar.gz" | sha256sum -c - \
 && tar -xzf /tmp/binary.tar.gz -C /usr/local/bin
```

10. **Use SLSA provenance or in-toto in CI (concept example)**
    

```text
# CI will produce provenance metadata describing build inputs/outputs
# Example: GitHub Actions with in-toto or SLSA builders — see your CI docs for implementation
```

---

## 8. Advanced patterns and real-world examples — explanation + 10 examples (with code)

**Concept (short):** Combine Dockerfile techniques for real-world architectures: sidecar patterns, build pipelines, conditional builds, multi-arch images, layered microservice images and monorepo partial builds.

### Examples

1. **Sidecar pattern (app + logging sidecar) — Docker Compose snippet (runtime pattern)**
    

```yaml
# docker-compose.yml
version: "3.8"
services:
  app:
    image: myapp:latest
    ports: ["8080:8080"]
  fluentd:
    image: fluent/fluentd:latest
    volumes: ["./fluent.conf:/fluentd/etc/fluent.conf"]
```

(Dockerfiles for each service follow standard best practices.)

2. **Mutli-arch build with `buildx` and a single Dockerfile**
    

```Dockerfile
# Dockerfile uses --platform hints
FROM --platform=$BUILDPLATFORM golang:1.21 AS builder
# build steps...
```

Build:

```bash
docker buildx build --platform linux/amd64,linux/arm64 -t myrepo/myapp:multiarch --push .
```

3. **Feature-flag builds using ARG to enable optional modules**
    

```Dockerfile
ARG ENABLE_METRICS=false
FROM node:20
WORKDIR /app
COPY . .
RUN if [ "$ENABLE_METRICS" = "true" ]; then npm run build:metrics; else npm run build; fi
```

4. **Image that can produce different artifacts based on `--target`**
    

```Dockerfile
FROM node:20 as dev
COPY . .
RUN npm ci
FROM dev as test
RUN npm test
FROM dev as prod
RUN npm run build
```

Build specific target:

```bash
docker build --target prod -t myapp:prod .
```

5. **ONBUILD base image for language templates**
    

```Dockerfile
# base.Dockerfile
FROM ubuntu
ONBUILD COPY . /app
ONBUILD RUN make /app
```

Child images inherit these steps.

6. **Entrypoint script that configures runtime using environment variables**
    

```Dockerfile
FROM python:3.11-slim
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["gunicorn","app:app"]
```

`entrypoint.sh` reads env vars to generate config before launching.

7. **Per-service Dockerfile in monorepo using `--output` to avoid images**
    

```Dockerfile
# In monorepo: build stage that outputs artifact
FROM node:20 AS builder
WORKDIR /repo/service
COPY service/package*.json ./
RUN npm ci
COPY service ./
RUN npm run build
```

CI: `docker build --target builder --output type=local,dest=out/service .`

8. **Layer squashing (when needed) — use `docker build --squash` (legacy; experimental)**
    

```bash
docker build --squash -t myapp:squashed .
```

(Prefer multi-stage + reduced layers instead; squash is less used.)

9. **Build image per environment with small differences using ARG**
    

```Dockerfile
ARG ENV=production
FROM node:20
WORKDIR /app
COPY . .
RUN if [ "$ENV" = "production" ]; then npm ci --production; else npm ci; fi
```

Build:

```bash
docker build --build-arg ENV=staging -t myapp:staging .
```

10. **Composable images: base security-hardened base used by many apps**
    

```Dockerfile
# secure-base.Dockerfile
FROM ubuntu:24.04
RUN apt-get update && apt-get install -y ca-certificates \
 && rm -rf /var/lib/apt/lists/*
USER nobody
# Use this in other Dockerfiles:
# FROM myrepo/secure-base:latest
```

---

## 9. Troubleshooting & debugging — explanation + 10 real‑world examples (with code/commands)

**Concept (short):** When builds fail or containers misbehave, use Docker/BuildKit tooling and Dockerfile practices to inspect layers, logs, and intermediate state. Use temporary debug steps to inspect file system and environment during build.

### Examples / Techniques

1. **Use verbose build output to see build steps**
    

```bash
# plain output
docker build --progress=plain -t myapp .
# or with BuildKit
DOCKER_BUILDKIT=1 docker build --progress=plain .
```

2. **Use `--target` to build to an intermediate stage for quick debug**
    

```bash
docker build --target builder -t myapp:builder .
docker run --rm -it myapp:builder /bin/sh
```

3. **Add temporary debug RUN steps to list files**
    

```Dockerfile
# add during debug
RUN echo "DEBUG: listing /app" && ls -la /app
```

4. **Run container with an interactive shell to inspect runtime**
    

```bash
docker run --rm -it --entrypoint /bin/sh myimage:latest
# inspect env, files, processes
env; ls -la /; ps aux
```

5. **Use `dive` to inspect image layers and what each layer contains**
    

```bash
# install dive and run
dive myrepo/myapp:latest
```

(dive is a local tool that visualizes layers and filesystem contents.)

6. **Check why cache is busted — copy order or file timestamps**
    

- If `npm ci` keeps rerunning, ensure you copied `package-lock.json` separately before `COPY . .`.
    
- Use `docker history myimage:latest` to inspect layer sizes and commands.
    

7. **Inspect intermediate container filesystem with BuildKit `--output`**
    

```bash
# output artifacts from a build stage for inspection
DOCKER_BUILDKIT=1 docker build --target builder --output type=local,dest=tmp_out .
ls -la tmp_out
```

8. **When `COPY` fails due to missing files — confirm build context and .dockerignore**
    

```bash
# make sure you run build from correct context
docker build -t myapp .
# and verify .dockerignore doesn't exclude required files
cat .dockerignore
```

9. **Debug failing package installs by capturing logs in RUN**
    

```Dockerfile
RUN apt-get update && apt-get install -y pkg || (cat /var/log/apt/term.log && exit 1)
```

10. **Reproduce exact environment using the same base digest**
    

```Dockerfile
FROM ubuntu@sha256:<digest>
# ensures the base layer is identical across builds
```

---

### Notes about applying these snippets in your Obsidian doc

- Paste each block directly into your Obsidian note under the matching section.
    
- If you want, I can:
    
    - Merge these sections directly into your open Canvas document (`Dockerfile Mastery — Beginner To Advanced (obsidian Markdown)`), or
        
    - Produce a single downloadable Markdown file (one complete `.md`) with all sections combined so you can import it into Obsidian.
        

Which would you prefer? Merge the content into the existing doc, or give you a single `.md` file for download?