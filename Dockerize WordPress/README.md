Here’s a **step-by-step guide to Dockerize a WordPress site** using Docker Compose — perfect for local development or production deployment 🚀

---

## 🧱 Folder Structure

Create a folder (for example `wordpress-docker`) with this structure:

```
wordpress-docker/
├── docker-compose.yml
├── .env
└── wp-data/       # (auto-created for persistent storage)
```

---

## ⚙️ Step 1: Create `.env` file

This file stores environment variables (database name, user, password, etc.):

```env
# .env
MYSQL_DATABASE=wordpress
MYSQL_USER=wordpress
MYSQL_PASSWORD=wordpress123
MYSQL_ROOT_PASSWORD=root123
WORDPRESS_TABLE_PREFIX=wp_
WORDPRESS_PORT=8080
```

---

## ⚙️ Step 2: Create `docker-compose.yml`

```yaml
version: '3.9'

services:
  wordpress:
    image: wordpress:latest
    container_name: wordpress_app
    depends_on:
      - db
    ports:
      - "${WORDPRESS_PORT}:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: ${MYSQL_USER}
      WORDPRESS_DB_PASSWORD: ${MYSQL_PASSWORD}
      WORDPRESS_DB_NAME: ${MYSQL_DATABASE}
      WORDPRESS_TABLE_PREFIX: ${WORDPRESS_TABLE_PREFIX}
    volumes:
      - ./wp-data:/var/www/html
    restart: always

  db:
    image: mysql:8.0
    container_name: wordpress_db
    command: --default-authentication-plugin=mysql_native_password
    environment:
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    volumes:
      - db_data:/var/lib/mysql
    restart: always

volumes:
  db_data:
```

---

## ⚙️ Step 3: Start Containers

Run this in your project folder:

```bash
docker compose up -d
```

✅ This will:

* Pull the **WordPress** and **MySQL** images
* Create containers named `wordpress_app` and `wordpress_db`
* Expose WordPress on port **8080**

---

## 🌐 Step 4: Open in Browser

Visit:

```
http://localhost:8080
```

You’ll see the WordPress setup screen 🎉

---

## 🗃️ Step 5: Manage Containers

```bash
docker compose ps         # List containers
docker compose stop       # Stop containers
docker compose down       # Remove containers
docker compose down -v    # Remove containers + volumes
```

---

## ⚡ Optional Add-ons

### 🔹 phpMyAdmin

Add this block to your `docker-compose.yml`:

```yaml
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: wordpress_phpmyadmin
    environment:
      PMA_HOST: db
      PMA_USER: ${MYSQL_USER}
      PMA_PASSWORD: ${MYSQL_PASSWORD}
    ports:
      - "8081:80"
    depends_on:
      - db
```

Access phpMyAdmin at:

```
http://localhost:8081
```

---

## 🧩 Step 6 (Optional): Custom Theme/Plugin Development

Mount your theme or plugin folder like this:

```yaml
    volumes:
      - ./wp-data:/var/www/html
      - ./my-theme:/var/www/html/wp-content/themes/my-theme
```

---

Would you like me to include **NGINX + PHP-FPM + MySQL (production-grade setup)** next?
That’s a more advanced setup suitable for deployment.
