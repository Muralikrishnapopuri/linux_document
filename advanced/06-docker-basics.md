# 🐳 Docker Basics

Containerize applications and manage containers.

---

## 1. Installation

```bash
# Install Docker
sudo apt update
sudo apt install docker.io

# Start and enable
sudo systemctl start docker
sudo systemctl enable docker

# Add your user to docker group (avoid sudo)
sudo usermod -aG docker $USER
# Log out and back in for this to take effect

# Verify installation
docker --version
docker run hello-world
```

---

## 2. Docker Images

### Pull & List Images

```bash
docker pull ubuntu                   # Pull latest Ubuntu image
docker pull ubuntu:22.04             # Pull specific version
docker pull nginx:alpine             # Pull lightweight nginx
docker images                        # List all images
docker images -a                     # Include intermediate images
```

### Remove Images

```bash
docker rmi ubuntu                    # Remove an image
docker rmi $(docker images -q)      # Remove all images
docker image prune                   # Remove unused images
docker image prune -a                # Remove ALL unused images
```

### Search Images

```bash
docker search nginx                  # Search Docker Hub
```

---

## 3. Running Containers

### Basic Run

```bash
docker run ubuntu echo "Hello"       # Run and exit
docker run -it ubuntu bash           # Interactive terminal
docker run -d nginx                  # Run in background (detached)
docker run --name myapp -d nginx     # Run with a custom name
docker run --rm ubuntu echo "temp"   # Auto-remove after exit
```

### Port Mapping

```bash
docker run -d -p 8080:80 nginx       # Host:Container port
docker run -d -p 3000:3000 myapp     # Same port mapping
docker run -d -P nginx               # Random host ports
```

### Volume Mounting

```bash
# Bind mount (host directory → container)
docker run -d -v /host/path:/container/path nginx

# Named volume
docker volume create mydata
docker run -d -v mydata:/app/data nginx

# Read-only mount
docker run -d -v /host/data:/data:ro nginx
```

### Environment Variables

```bash
docker run -d -e DB_HOST=localhost -e DB_PORT=5432 myapp
docker run -d --env-file .env myapp
```

---

## 4. Managing Containers

### List Containers

```bash
docker ps                            # Running containers
docker ps -a                         # All containers (including stopped)
docker ps -q                         # Only container IDs
```

### Start/Stop/Restart

```bash
docker start container_name          # Start stopped container
docker stop container_name           # Graceful stop
docker restart container_name        # Restart
docker kill container_name           # Force stop
```

### Remove Containers

```bash
docker rm container_name             # Remove stopped container
docker rm -f container_name          # Force remove (even running)
docker rm $(docker ps -aq)           # Remove all stopped containers
docker container prune               # Remove all stopped containers
```

### Interact with Containers

```bash
# Execute command inside running container
docker exec -it container_name bash
docker exec -it container_name sh

# View logs
docker logs container_name
docker logs -f container_name        # Follow (live)
docker logs --tail 50 container_name # Last 50 lines

# Copy files
docker cp file.txt container_name:/path/
docker cp container_name:/path/file.txt ./

# Inspect container details
docker inspect container_name
```

---

## 5. Building Images (Dockerfile)

### Sample Dockerfile

```dockerfile
# Use Node.js base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Expose port
EXPOSE 3000

# Start command
CMD ["npm", "start"]
```

### Build & Tag

```bash
docker build -t myapp .                      # Build image
docker build -t myapp:v1.0 .                 # Build with tag
docker build -t myapp -f Dockerfile.prod .   # Custom Dockerfile
docker build --no-cache -t myapp .           # Build without cache
```

---

## 6. Docker Compose

Manage multi-container applications.

### Sample `docker-compose.yml`

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DB_HOST=db
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      - POSTGRES_PASSWORD=secret
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

### Docker Compose Commands

```bash
docker compose up                    # Start all services
docker compose up -d                 # Start in background
docker compose down                  # Stop and remove
docker compose ps                    # List running services
docker compose logs -f               # Follow logs
docker compose build                 # Rebuild images
docker compose exec web bash         # Shell into a service
docker compose restart web           # Restart a service
```

---

## 7. Docker Cleanup

```bash
# Remove unused data
docker system prune                  # Clean unused containers/images/networks
docker system prune -a               # Also remove unused images
docker system prune --volumes        # Include volumes
docker system df                     # Show disk usage
```

---

## 💡 Tips

- Use `-d` for background, `-it` for interactive sessions
- Always name your containers with `--name`
- Use Docker Compose for multi-container apps
- Use `.dockerignore` to exclude files from builds
- Use Alpine-based images for smaller sizes

---

[← Previous: Performance](05-performance-monitoring.md) | [Back to Index](../README.md) | [Next: Git →](07-git-version-control.md)
