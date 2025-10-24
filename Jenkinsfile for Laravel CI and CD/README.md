Below is a **simple and working Jenkinsfile** for **Laravel CI/CD using Docker Compose** 🚀
(Assuming Jenkins is installed with Docker + Docker Compose plugins and correct permissions).

---

### ✅ **Jenkinsfile (Declarative Pipeline)**

```groovy
pipeline {
    agent any

    environment {
        DOCKER_COMPOSE = "docker-compose -f docker-compose.yml"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://your-github-repo-url.git'
            }
        }

        stage('Build Docker Containers') {
            steps {
                sh "${DOCKER_COMPOSE} build --no-cache"
            }
        }

        stage('Start Containers') {
            steps {
                sh "${DOCKER_COMPOSE} up -d"
            }
        }

        stage('Install Dependencies') {
            steps {
                sh "${DOCKER_COMPOSE} exec app composer install"
                sh "${DOCKER_COMPOSE} exec app cp .env.example .env || true"
                sh "${DOCKER_COMPOSE} exec app php artisan key:generate"
            }
        }

        stage('Run Migrations') {
            steps {
                sh "${DOCKER_COMPOSE} exec app php artisan migrate --force"
            }
        }

        stage('Run Tests') {
            steps {
                sh "${DOCKER_COMPOSE} exec app php artisan test"
            }
        }

    }

    post {
        always {
            echo "Cleaning up containers..."
            sh "${DOCKER_COMPOSE} down"
        }
    }
}
```

---

### 📌 What This Pipeline Does

| Stage                   | Description                     |
| ----------------------- | ------------------------------- |
| Clone Repository        | Pulls Laravel source            |
| Build Docker Containers | Builds app, web, db, phpmyadmin |
| Start Containers        | Launch stack in background      |
| Install Dependencies    | Composer install + env + key    |
| Run Migrations          | Applies DB structure            |
| Run Tests               | Runs Laravel tests              |
| Cleanup                 | Stops services after pipeline   |

---

### ✅ Requirements in Jenkins Server

Make sure Jenkins node has:

```bash
docker -v
docker-compose -v
```

Allow Jenkins to run Docker commands:

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

### ✅ Optional Deployment Stage

If you want deployment to a production server, tell me:
✅ Hosting server?
✅ Deployment path?
✅ SSH credentials setup (Yes/No)?

---

If you want, I can also:
✅ Set up automated builds on every Git push
✅ Send notifications for failures
✅ Add caching to speed up the pipeline
✅ Deploy artifacts to production

Would you like me to include **Node build (npm install + Vite)** stage for frontend assets too?
