Here’s a step-by-step guide to set up **Laravel 12 with FilamentPHP using Docker**. This will give you a local development environment with services like Nginx, PHP, MySQL, and Laravel with FilamentPHP pre-installed.

---

### ✅ Step 1: Project Structure

Create the following folder structure:

```
laravel-filament-docker/
├── docker
│   ├── nginx
│   │   └── default.conf
│   └── php
│       └── Dockerfile
├── .env
├── docker-compose.yml
└── (Laravel app will go here)
```

---

### ✅ Step 2: `docker-compose.yml`

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: docker/php/Dockerfile
    container_name: laravel_app
    volumes:
      - .:/var/www/html
    working_dir: /var/www/html
    depends_on:
      - db

  web:
    image: nginx:alpine
    container_name: nginx_web
    ports:
      - "8000:80"
    volumes:
      - .:/var/www/html
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app

  db:
    image: mysql:8
    container_name: mysql_db
    environment:
      MYSQL_DATABASE: laravel
      MYSQL_ROOT_PASSWORD: root
      MYSQL_USER: laravel
      MYSQL_PASSWORD: secret
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

---

### ✅ Step 3: `docker/php/Dockerfile`

```Dockerfile
FROM php:8.3-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    zip unzip git curl \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
```

---

### ✅ Step 4: `docker/nginx/default.conf`

```nginx
server {
    listen 80;
    index index.php index.html;
    root /var/www/html/public;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

---

### ✅ Step 5: Laravel Installation

Inside the root of the project:

```bash
docker-compose run --rm app composer create-project laravel/laravel .
```

---

### ✅ Step 6: Install FilamentPHP

```bash
docker-compose exec app bash
# Inside container
composer require filament/filament
php artisan migrate
```

Optionally, publish assets and create an admin user:

```bash
php artisan make:filament-user
```

---

### ✅ Step 7: Configure `.env`

Update your `.env` with:

```env
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

---

### ✅ Step 8: Run the app

```bash
docker-compose up -d
```

Visit [http://localhost:8000/admin](http://localhost:8000/admin) to access FilamentPHP.

---

Let me know if you want to include Redis, Mailhog, or any other services in the setup!
