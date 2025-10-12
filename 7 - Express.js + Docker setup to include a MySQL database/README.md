Excellent 👌 — let’s extend your **Express.js + Docker** setup to include a **MySQL** database using **Docker Compose**.

You’ll end up with:

* A Node.js Express API (web app)
* A MySQL database running in another container
* Both connected through Docker networking

---

## 📁 Final Project Structure

```
node-express-mysql-app/
│
├── Dockerfile
├── docker-compose.yml
├── package.json
├── server.js
└── public/
    └── index.html
```

---

## 🧱 Step 1: `server.js`

Add a simple MySQL connection using `mysql2`.

```js
// server.js
const express = require("express");
const mysql = require("mysql2");
const path = require("path");

const app = express();
const PORT = process.env.PORT || 3000;

// Serve static HTML
app.use(express.static(path.join(__dirname, "public")));

// Database connection
const db = mysql.createConnection({
  host: process.env.DB_HOST || "db",
  user: process.env.DB_USER || "root",
  password: process.env.DB_PASSWORD || "root",
  database: process.env.DB_NAME || "testdb",
});

// Connect to MySQL
db.connect((err) => {
  if (err) {
    console.error("❌ MySQL connection failed:", err.message);
  } else {
    console.log("✅ Connected to MySQL Database");
  }
});

// API endpoint
app.get("/api/users", (req, res) => {
  db.query("SELECT NOW() AS current_time", (err, result) => {
    if (err) return res.status(500).json({ error: err.message });
    res.json({ message: "Hello from Express + MySQL!", result });
  });
});

app.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
});
```

---

## 📦 Step 2: `package.json`

```json
{
  "name": "express-mysql-docker-app",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mysql2": "^3.9.0"
  }
}
```

> Install locally before building (optional):
>
> ```bash
> npm install express mysql2
> ```

---

## 🐳 Step 3: `Dockerfile`

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

## ⚙️ Step 4: `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    build: .
    container_name: express_app
    ports:
      - "8080:3000"
    environment:
      - DB_HOST=db
      - DB_USER=root
      - DB_PASSWORD=root
      - DB_NAME=testdb
    depends_on:
      - db
    volumes:
      - .:/app

  db:
    image: mysql:8.0
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: testdb
    ports:
      - "3307:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

---

## 🌐 Step 5: `public/index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <title>Express + MySQL Docker App</title>
</head>
<body>
  <h1>Express.js + MySQL 🐳</h1>
  <p>Check the API at <a href="/api/users">/api/users</a></p>
</body>
</html>
```

---

## 🚀 Step 6: Run Everything

From your project folder:

```bash
docker-compose up --build
```

Then open in your browser:

* 🌍 HTML page → [http://localhost:8080](http://localhost:8080)
* 📡 API endpoint → [http://localhost:8080/api/users](http://localhost:8080/api/users)

You’ll see JSON output from MySQL with the current timestamp.

---

## ✅ Optional: `.dockerignore`

```
node_modules
npm-debug.log
Dockerfile
docker-compose.yml
.dockerignore
```

---

Would you like me to add **Sequelize ORM** next (so you can create real tables and models in MySQL from your Express app)?
