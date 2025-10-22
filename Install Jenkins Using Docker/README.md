#  How to Install Jenkins Using Docker on Windows 11 (2025) 
```
https://www.youtube.com/watch?v=N1MhQcFPIUI
```

Here’s a **step-by-step guide** to install **Jenkins using Docker** — clean, simple, and works on any OS (Windows, macOS, Linux).

---

## 🧩 1. **Prerequisites**

Make sure you have these installed:

* **Docker Engine** → [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)
* **Docker Compose (optional)** → [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

To check:

```bash
docker --version
docker compose version
```

---

## ⚙️ 2. **Pull the Jenkins Image**

Use the official Jenkins LTS (Long Term Support) image:

```bash
docker pull jenkins/jenkins:lts
```

---

## 🚀 3. **Run Jenkins Container**

Run Jenkins in a Docker container (persistent setup):

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

```bash
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

### Explanation:

* `-d` → run in detached mode
* `--name jenkins` → name your container
* `-p 8080:8080` → Jenkins web UI port
* `-p 50000:50000` → for Jenkins agents
* `-v jenkins_home:/var/jenkins_home` → persistent volume (saves Jenkins data)

---

## 🌐 4. **Access Jenkins**

Once running, open:

```
http://localhost:8080
```

To get the **initial admin password**, run:

```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Copy that password → paste it into the web setup wizard → install suggested plugins → create admin user.

---

## 🐳 5. **Using Docker Compose (Optional)**

Create a `docker-compose.yml` file:

```yaml
version: '3.9'
services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
volumes:
  jenkins_home:
```

Then run:

```bash
docker compose up -d
```

---

## 🔧 6. **(Optional) Give Jenkins Access to Docker**

If you want Jenkins to run Docker commands inside its jobs:

1. Share the Docker socket:

   ```bash
   docker run -d \
     --name jenkins \
     -p 8080:8080 \
     -p 50000:50000 \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v jenkins_home:/var/jenkins_home \
     jenkins/jenkins:lts
   ```

2. Install Docker inside the Jenkins container or use **Docker-in-Docker** approach.

---

## 🧰 7. **Stop / Restart Jenkins**

Stop:

```bash
docker stop jenkins
```

Start again:

```bash
docker start jenkins
```

Remove:

```bash
docker rm -f jenkins
```

---

Would you like me to include **Docker + Jenkins + MySQL + Nginx** in a single `docker-compose.yml` setup (for CI/CD pipeline example)?
