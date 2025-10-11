Here’s a **complete list of essential Docker commands** — categorized for clarity and quick reference 👇

---

## 🐳 **1. Docker Setup & Version**

```bash
docker --version          # Check Docker version
docker info               # Display system-wide information
docker help               # List all available commands
```

---

## 📦 **2. Images (Docker Hub & Local Images)**

```bash
docker pull <image_name>              # Download image from Docker Hub
docker images                         # List all images
docker rmi <image_id>                 # Remove image
docker tag <image_id> <repo:tag>      # Tag an image
docker inspect <image_name>           # Get detailed info about an image
docker history <image_name>           # Show image layers
```

**Example:**

```bash
docker pull php:8.2
docker images
```

---

## 🧱 **3. Containers (Run / Stop / Manage)**

```bash
docker run <image_name>                       # Run a new container
docker run -it <image_name> bash              # Run interactively with bash
docker run -d <image_name>                    # Run in detached (background) mode
docker run -p 8080:80 <image_name>            # Map port 8080 (host) to 80 (container)
docker run --name mycontainer <image_name>    # Give container a name
docker ps                                     # List running containers
docker ps -a                                  # List all containers (including stopped)
docker stop <container_id>                    # Stop running container
docker start <container_id>                   # Start stopped container
docker restart <container_id>                 # Restart container
docker rm <container_id>                      # Remove container
docker logs <container_id>                    # Show container logs
docker exec -it <container_id> bash           # Open shell inside running container
```

**Example:**

```bash
docker run -d -p 3306:3306 --name mysql-container mysql:latest
```

---

## 💾 **4. Volumes (Data Persistence)**

```bash
docker volume create myvolume               # Create volume
docker volume ls                            # List volumes
docker volume inspect myvolume              # Inspect volume
docker volume rm myvolume                   # Remove volume
docker run -v myvolume:/data <image_name>   # Mount volume in container
```

---

## 🌐 **5. Networks**

```bash
docker network ls                            # List networks
docker network create mynetwork              # Create network
docker network inspect mynetwork             # Inspect network
docker network connect mynetwork <container> # Connect container to network
docker network disconnect mynetwork <container> # Disconnect container
docker network rm mynetwork                  # Remove network
```

---

## 🧹 **6. Clean Up**

```bash
docker system prune                  # Remove all unused containers, networks, images
docker system prune -a               # Remove all unused (including images)
docker image prune                   # Remove dangling images
docker container prune               # Remove stopped containers
docker volume prune                  # Remove unused volumes
```

---

## 🧰 **7. Build & Compose**

```bash
docker build -t myapp:latest .       # Build image from Dockerfile
docker-compose up                    # Start services defined in docker-compose.yml
docker-compose up -d                 # Start in detached mode
docker-compose down                  # Stop and remove services
docker-compose ps                    # List services
docker-compose logs                  # View service logs
```

---

## 🧾 **8. Inspect & Copy Files**

```bash
docker inspect <container_id>                  # Detailed info about a container
docker cp <container_id>:/path/in/container .  # Copy from container to host
docker cp ./file.txt <container_id>:/app/      # Copy from host to container
```

---

## 🔒 **9. Login / Push / Pull from Docker Hub**

```bash
docker login                               # Log in to Docker Hub
docker tag <image_id> username/repo:tag    # Tag image for push
docker push username/repo:tag              # Push image to Docker Hub
docker pull username/repo:tag              # Pull image from Docker Hub
```

---

## ⚙️ **10. System Information**

```bash
docker stats                # Show real-time container resource usage
docker top <container_id>   # Show running processes inside container
docker events               # Show Docker events in real time
docker df                   # Show disk usage
```

---

Would you like me to give you a **cheat sheet (PDF)** version of these commands for offline reference?
