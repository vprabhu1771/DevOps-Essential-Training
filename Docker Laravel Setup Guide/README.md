Here’s a **step-by-step guide** to set up **Docker** and install **Laravel 12** for a full project setup — ideal for beginners.

---

## ✅ Prerequisites

* Docker Desktop installed: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
* Composer installed (optional, but helpful locally): [https://getcomposer.org/download/](https://getcomposer.org/download/)

---

## 🔧 Step 1: Create Your Project Directory

```bash
mkdir laravel-docker
cd laravel-docker
```

---

## 🐳 Step 2: Create `docker-compose.yml`

```yaml
version: '3.8'

services:
  app:
    image: laravelsail/php83-composer
    container_name: laravel_app
    ports:
      - "8000:8000"
    volumes:
      - .:/var/www/html
    working_dir: /var/www/html
    depends_on:
      - mysql
    command: >
      sh -c "composer install &&
             php artisan key:generate &&
             php artisan migrate &&
             php artisan serve --host=0.0.0.0"

  mysql:
    image: mysql:8.0
    container_name: mysql_db
    ports:
      - "3306:3306"
    environment:
      MYSQL_DATABASE: laravel
      MYSQL_ROOT_PASSWORD: root
      MYSQL_USER: laravel
      MYSQL_PASSWORD: secret
    volumes:
      - dbdata:/var/lib/mysql

volumes:
  dbdata:
```

---

## 📦 Step 3: Create `Dockerfile`

```dockerfile
FROM laravelsail/php83-composer

# Optional: Install Laravel dependencies if needed
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    git \
    curl

WORKDIR /var/www/html
```

---

## 🛠 Step 4: Create Laravel 12 App

Run this **outside** the container:

```bash
docker run --rm -v $(pwd):/app composer create-project laravel/laravel:^12.0 .
```

On **Windows (PowerShell or CMD)**, the `$(pwd)` syntax won't work. Here's how to rewrite the command to create a **Laravel 12** project using Docker on Windows:

---

## ✅ PowerShell

```powershell
docker run --rm -v ${PWD}:/app composer create-project laravel/laravel:^12.0 .
```

## ✅ CMD (Command Prompt)

```cmd
docker run --rm -v "%cd%":/app composer create-project laravel/laravel:^12.0 .
```

---

These commands:

* Mount the current directory (`%cd%` or `${PWD}`) to `/app` inside the container
* Use the official `composer` image to create a Laravel 12 project
* Install everything into the current directory (`.`)

---

Let me know if you're using **Git Bash** or **WSL**, as the syntax changes slightly there too.


---

The error:

```
Project directory "/app/." is not empty.
```

means that **Composer refuses to install Laravel** into a directory that is **not empty** (likely because you already have some files like `docker-compose.yml`, `Dockerfile`, etc.).

---

### ✅ Solutions

#### 🟢 Option 1: Install Laravel in a subdirectory (recommended)

```powershell
docker run --rm -v ${PWD}:/app composer create-project laravel/laravel:^12.0 laravel
```

This creates the Laravel project in a subdirectory called `laravel/` inside your current folder.

---

#### 🟡 Option 2: Remove existing files (⚠️ careful)

If you're okay deleting the existing files in the current folder:

```bash
# Make sure you want to remove these first!
rm -rf * .[^.]*   # For Git Bash / WSL / Linux / macOS
```

On **Windows PowerShell**:

```powershell
Remove-Item * -Recurse -Force
Remove-Item .* -Recurse -Force  # This deletes hidden files too (use with caution!)
```

Then re-run:

```powershell
docker run --rm -v ${PWD}:/app composer create-project laravel/laravel:^12.0 .
```

---

#### 🟣 Option 3: Create Laravel first, then add Docker files

1. Create the Laravel project **outside** your Docker setup:

```powershell
composer create-project laravel/laravel laravel-app "^12.0"
```

2. Then go inside that folder and add your `Dockerfile`, `docker-compose.yml`, etc.

---

Let me know which option you prefer, and I can tailor the Docker setup accordingly.


---

## ✏️ Step 5: Configure `.env`

In `.env` file, set DB credentials:

```dotenv
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

---

## 🚀 Step 6: Start Docker

```bash
docker-compose up --build
```

Visit your app at:
👉 [http://localhost:8000](http://localhost:8000)

---

## ✅ Optional: Laravel Sail Alternative

Laravel 12 comes with Sail. You can use:

```bash
composer require laravel/sail --dev
php artisan sail:install
./vendor/bin/sail up
```

This sets up a Docker-based environment tailored to Laravel.

---

## 🧪 Useful Commands (Inside the Container)

```bash
docker exec -it laravel_app bash
php artisan migrate
php artisan tinker
```

---

## 📁 Project Structure (after setup)

```
laravel-docker/
├── app/
├── bootstrap/
├── database/
├── docker-compose.yml
├── Dockerfile
├── public/
├── routes/
├── storage/
├── .env
└── vendor/
```

---

Let me know if you want a **Vue/React frontend**, **custom NGINX setup**, or **Laravel Breeze/Jetstream** added to the setup.
