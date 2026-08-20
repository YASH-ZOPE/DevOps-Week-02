# 🐳 Docker Fundamentals & Containerization Notes

Detailed reference notes on Docker concepts, container lifecycle, Dockerfile syntax, volume persistence, networking, and Docker Compose.

---

## 🌟 1. Key Concepts Architecture

- **Image:** A read-only executable template containing application code, runtime, libraries, environment variables, and configuration.
- **Container:** A lightweight, isolated runtime instance of a Docker image.
- **Dockerfile:** A text document containing instructions to build a Docker image.
- **Docker Registry:** A storage and distribution system for Docker images (e.g., Docker Hub, AWS ECR, GitHub Container Registry).
- **Volume:** Persistent data storage backed by the host filesystem outside the container union filesystem.

---

## ⚡ 2. Core Docker CLI Commands

### Container Lifecycle
```bash
# Run a container in detached mode (-d) with port mapping (-p) and container name (--name)
docker run -d -p 8080:80 --name my-web-app nginx:latest

# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a running container
docker stop my-web-app

# Start a stopped container
docker start my-web-app

# Restart a container
docker restart my-web-app

# Remove a container
docker rm my-web-app

# Force remove a running container
docker rm -f my-web-app
```

### Logs & Executing Commands
```bash
# View container logs
docker logs my-web-app

# Tail container logs in real time
docker logs -f --tail 100 my-web-app

# Execute an interactive bash/sh session inside a running container
docker exec -it my-web-app /bin/bash

# View resource consumption (CPU, Memory, I/O) of containers
docker stats
```

---

## 🏗️ 3. Image Management & Building

```bash
# List local Docker images
docker images

# Pull image from registry
docker pull node:18-alpine

# Build image from local Dockerfile with tag
docker build -t my-custom-app:1.0 .

# Tag an image for remote registry
docker tag my-custom-app:1.0 username/my-custom-app:1.0

# Push image to registry
docker push username/my-custom-app:1.0

# Remove an image
docker rmi my-custom-app:1.0

# Remove unused/dangling images and build cache
docker image prune -f
docker system prune -a --volumes
```

---

## 📄 4. Writing Dockerfiles

### Best Practices Example (Multi-Stage Build Node.js App)

```dockerfile
# --- Stage 1: Build Stage ---
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package manifests first to leverage Docker layer caching
COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

# --- Stage 2: Production Stage ---
FROM node:18-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# Copy built assets and production dependencies from builder stage
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist

# Create non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

EXPOSE 3000

CMD ["node", "dist/index.js"]
```

---

## 💾 5. Data Persistence & Volumes

```bash
# Create a named Docker volume
docker volume create app-data

# List all volumes
docker volume ls

# Inspect volume details
docker volume inspect app-data

# Run container with named volume mount
docker run -d --name db-container -v app-data:/var/lib/mysql mysql:8.0

# Run container with bind mount (host directory mapping)
docker run -d --name dev-app -v $(pwd):/app node:18-alpine

# Delete unused volumes
docker volume prune
```

---

## 🌐 6. Container Networking

```bash
# List docker networks
docker network ls

# Create a custom bridge network
docker network create my-app-network

# Inspect network details
docker network inspect my-app-network

# Connect running container to network
docker network connect my-app-network my-web-app

# Run container inside specific network
docker run -d --name backend --network my-app-network my-api-image
```

---

## 🐙 7. Docker Compose

`docker-compose.yml` allows defining multi-container applications in a single declarative file.

### Example `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=database
    depends_on:
      - database
    networks:
      - app-net
    restart: always

  database:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: devuser
      POSTGRES_PASSWORD: devsecretpassword
      POSTGRES_DB: appdb
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - app-net

volumes:
  pgdata:

networks:
  app-net:
    driver: bridge
```

### Docker Compose CLI Commands
```bash
# Start all services in detached mode
docker compose up -d

# Stop and remove containers, networks, and volumes created by up.
docker compose down -v

# View status of containers managed by compose.
docker compose ps

# View logs for all services in compose file.
docker compose logs -f

# Rebuild images and restart services.
docker compose up -d --build
```
