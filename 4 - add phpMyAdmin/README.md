Got it 👍 — you’re switching from using a prebuilt PHP image to **building your own PHP image with mysqli installed**, and you also want to **add phpMyAdmin**.

Let’s fix and complete your setup properly 👇

---

## ✅ Final Working Setup

### 🧱 Folder Structure

```
php-mysql-docker/
│
├── docker-compose.yml
├── Dockerfile
├── php/
│   └── index.php
└── db/
    └── init.sql
```

---

### 🐳 Step 1: `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    build: .
    container_name: php_app
    volumes:
      - ./php:/var/www/html
    ports:
      - "8080:80"
    depends_on:
      - db

  db:
    image: mysql:8.0
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: testdb
      MYSQL_USER: testuser
      MYSQL_PASSWORD: testpass
    volumes:
      - db_data:/var/lib/mysql
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: always
    environment:
      PMA_HOST: db
      PMA_USER: root
      PMA_PASSWORD: root
    ports:
      - "8081:80"
    depends_on:
      - db

volumes:
  db_data:
```

---

### 🐋 Step 2: `Dockerfile`

```Dockerfile
# Use official PHP Apache image
FROM php:8.2-apache

# Install MySQLi and enable it
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli

# Optional: enable mod_rewrite for Laravel/other frameworks
RUN a2enmod rewrite

# Copy project files (optional if using volume)
COPY ./php /var/www/html/

EXPOSE 80
```

---

### 🗄️ Step 3: `db/init.sql`

```sql
CREATE TABLE IF NOT EXISTS users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100)
);

INSERT INTO users (name, email) VALUES
('Prabhu', 'prabhu@example.com'),
('John Doe', 'john@example.com');
```

---

### 🧾 Step 4: `php/index.php`

```php
<?php
$servername = "db";
$username = "testuser";
$password = "testpass";
$dbname = "testdb";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

echo "<h2>Connected successfully!</h2>";

$result = $conn->query("SELECT * FROM users");

if ($result->num_rows > 0) {
  echo "<table border='1'><tr><th>ID</th><th>Name</th><th>Email</th></tr>";
  while($row = $result->fetch_assoc()) {
    echo "<tr><td>{$row['id']}</td><td>{$row['name']}</td><td>{$row['email']}</td></tr>";
  }
  echo "</table>";
} else {
  echo "No users found.";
}

$conn->close();
?>
```

---

### 🚀 Step 5: Build and Run

```bash
docker-compose up -d --build
```

Wait a few seconds for MySQL and phpMyAdmin to initialize.

---

### 🌐 Step 6: Access

| Service    | URL                                            | Login       |
| ---------- | ---------------------------------------------- | ----------- |
| PHP App    | [http://localhost:8080](http://localhost:8080) | —           |
| phpMyAdmin | [http://localhost:8081](http://localhost:8081) | root / root |

---

### 🧹 Stop and Clean

```bash
docker-compose down
```

Delete database volume too:

```bash
docker-compose down -v
```

---

Would you like me to extend this setup to include **Composer** inside the PHP container (useful if you plan to add frameworks like Laravel or install PHP packages)?
