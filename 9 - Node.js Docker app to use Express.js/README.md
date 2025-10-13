Perfect 👍 Let's upgrade your Node.js Docker app to use **Express.js**, so it can serve both:
✅ a simple **HTML page**, and
✅ an **API endpoint** (`/api/hello`).

---

## 🧩 Folder Structure

```
node-express-app/
│
├── Dockerfile
├── package.json
├── server.js
└── public/
    └── index.html
```

---

## 🧱 Step 1: `public/index.html`

This will be served as your home page.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Node Express Docker App</title>
</head>
<body>
  <h1>Hello from Express.js inside Docker 🐳</h1>
  <p>This page is served from a Docker container.</p>
</body>
</html>
```

---

## ⚙️ Step 2: `server.js`

This Express app serves static HTML and provides a JSON API endpoint.

```js
// server.js
const express = require("express");
const path = require("path");

const app = express();
const PORT = process.env.PORT || 3000;

// Serve static files from "public" folder
app.use(express.static(path.join(__dirname, "public")));

// API endpoint
app.get("/api/hello", (req, res) => {
  res.json({ message: "Hello from the Express API inside Docker!" });
});

// Start server
app.listen(PORT, () => {
  console.log(`🚀 Server is running on port ${PORT}`);
});
```

---

## 📦 Step 3: `package.json`

```json
{
  "name": "express-docker-app",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

> You can also create this automatically:
>
> ```bash
> npm init -y
> npm install express
> ```

---

## 🐳 Step 4: `Dockerfile`

```dockerfile
# Use official Node.js LTS version
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files first (for better caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy rest of the app
COPY . .

# Expose app port
EXPOSE 3000

# Start the server
CMD ["npm", "start"]
```

---

## ⚡ Step 5: Build & Run the Container

### Build the image

```bash
docker build -t express-docker-app .
```

### Run the container

```bash
docker run -d -p 8080:3000 express-docker-app
```

---

## 🌐 Test It

* Visit [http://localhost:8080](http://localhost:8080) → shows your HTML page
* Visit [http://localhost:8080/api/hello](http://localhost:8080/api/hello) → returns JSON:

  ```json
  { "message": "Hello from the Express API inside Docker!" }
  ```

---

## 🧹 Step 6 (Optional): `.dockerignore`

```
node_modules
npm-debug.log
Dockerfile
.dockerignore
```

---

Would you like me to also show how to add **docker-compose.yml** (to include MongoDB or MySQL later)?
