# Docker Cheatsheet

## Images

### Build Images

```bash
# Build image from Dockerfile
docker build -t my-image:latest .

# Build with specific Dockerfile
docker build -t my-image:v1.0 -f Dockerfile.prod .

# Build from stdin
docker build -t my-image:latest - < Dockerfile

# Build with build arguments
docker build -t my-image:latest --build-arg NODE_ENV=production .

# Build with multiple tags
docker build -t my-image:latest -t my-image:v1.0 .

# Build without cache
docker build --no-cache -t my-image:latest .

# Build with progress output
docker build --progress=plain -t my-image:latest .
```

### Manage Images

```bash
# List local images
docker images

# List images with filter
docker images my-image

# Search Docker Hub
docker search nginx

# Pull image
docker pull ubuntu:22.04

# Push image to registry
docker push my-registry/my-image:latest

# Tag image
docker tag my-image:latest my-registry/my-image:v1.0

# Remove image
docker rmi my-image:latest

# Remove dangling images
docker image prune

# Remove all unused images
docker image prune -a

# Inspect image
docker inspect my-image:latest

# Save image to tar
docker save my-image:latest > my-image.tar

# Load image from tar
docker load < my-image.tar
```

## Containers

### Run Containers

```bash
# Run container
docker run -d my-image:latest

# Run with name
docker run -d --name my-container my-image:latest

# Run with port mapping
docker run -d -p 8080:8080 my-image:latest

# Run with environment variables
docker run -d -e NODE_ENV=production my-image:latest

# Run with multiple env vars
docker run -d \
  -e NODE_ENV=production \
  -e DATABASE_URL=postgres://... \
  my-image:latest

# Run with volume mount
docker run -d -v /host/path:/container/path my-image:latest

# Run with volume and read-only
docker run -d -v /host/path:/container/path:ro my-image:latest

# Run with network
docker run -d --network my-network my-image:latest

# Run with resource limits
docker run -d --memory=512m --cpus=0.5 my-image:latest

# Run with user
docker run -d --user 1000:1000 my-image:latest

# Run interactively
docker run -it ubuntu:22.04 /bin/bash

# Run with restart policy
docker run -d --restart=always my-image:latest
```

### Manage Containers

```bash
# List running containers
docker ps

# List all containers
docker ps -a

# List container IDs only
docker ps -q

# List with formatting
docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"

# Pause container
docker pause my-container

# Unpause container
docker unpause my-container

# Stop container
docker stop my-container

# Force stop container
docker kill my-container

# Start container
docker start my-container

# Restart container
docker restart my-container

# Remove container
docker rm my-container

# Remove running container (force)
docker rm -f my-container

# Remove all stopped containers
docker container prune

# Rename container
docker rename my-container new-name
```

### Container Inspection

```bash
# View container logs
docker logs my-container

# Follow logs in real-time
docker logs -f my-container

# View last 100 lines
docker logs --tail=100 my-container

# View logs with timestamps
docker logs -t my-container

# Inspect container
docker inspect my-container

# Get container IP address
docker inspect --format='{{.NetworkSettings.IPAddress}}' my-container

# Get container PID
docker inspect --format='{{.State.Pid}}' my-container

# View container top processes
docker top my-container

# View container stats
docker stats my-container

# View all container stats
docker stats
```

### Executing Commands

```bash
# Execute command in running container
docker exec my-container ls -la

# Execute interactively
docker exec -it my-container /bin/bash

# Execute as specific user
docker exec -u 1000 my-container whoami

# Execute with environment variables
docker exec -e MY_VAR=value my-container printenv MY_VAR
```

## Networks

```bash
# List networks
docker network ls

# Create network
docker network create my-network

# Create custom bridge network
docker network create --driver bridge my-network

# Remove network
docker network rm my-network

# Inspect network
docker network inspect my-network

# Connect container to network
docker network connect my-network my-container

# Disconnect container from network
docker network disconnect my-network my-container
```

## Volumes

```bash
# List volumes
docker volume ls

# Create volume
docker volume create my-volume

# Inspect volume
docker volume inspect my-volume

# Remove volume
docker volume rm my-volume

# Remove dangling volumes
docker volume prune

# Mount volume in container
docker run -d -v my-volume:/data my-image:latest

# Mount directory as volume
docker run -d -v /host/path:/container/path my-image:latest

# Mount read-only
docker run -d -v /host/path:/container/path:ro my-image:latest
```

## Docker Compose

```bash
# Start services (creates and starts)
docker-compose up

# Start in detached mode
docker-compose up -d

# Build services
docker-compose build

# Build and start
docker-compose up --build

# Stop services
docker-compose down

# Stop without removing
docker-compose stop

# Remove services
docker-compose rm

# View logs
docker-compose logs

# Follow logs
docker-compose logs -f

# View logs for specific service
docker-compose logs service-name

# Execute command in service
docker-compose exec service-name bash

# Scale service
docker-compose up -d --scale service-name=3

# View running services
docker-compose ps
```

## Debugging

```bash
# View container logs
docker logs my-container

# Follow container logs
docker logs -f my-container

# Get detailed container info
docker inspect my-container | jq '.

# Check container exit code
docker inspect my-container | jq '.State.ExitCode'

# Copy files from container
docker cp my-container:/path/to/file ./local-file

# Copy files to container
docker cp ./local-file my-container:/path/to/file

# Run temporary debug container
docker run -it --rm --entrypoint /bin/bash my-image:latest
```

## Useful One-Liners

```bash
# Remove all stopped containers
docker container prune -f

# Remove all unused images
docker image prune -a -f

# Remove all unused volumes
docker volume prune -f

# Stop all running containers
docker stop $(docker ps -q)

# Kill all containers
docker kill $(docker ps -q)

# Remove all containers
docker rm $(docker ps -aq)

# Show all images with sizes
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"

# Get container IP addresses
docker ps -q | xargs -I {} docker inspect --format='{{.Name}} {{.NetworkSettings.IPAddress}}' {}

# View Docker disk usage
docker system df

# Clean up everything (warning: removes all unused resources)
docker system prune -a
```
