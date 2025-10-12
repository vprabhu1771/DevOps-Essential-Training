Here’s a **simple, clean example** to dockerize a basic **Node.js web page** using **Docker**.

---

## 🧩 Step 1: Project Structure

```
node-app/
│
├── Dockerfile
├── package.json
└── server.js
```

---

## 🪶 Step 2: `server.js`

Create a simple Node.js web server.

```js
// server.js
const http = require("http");

const PORT = process.env.PORT || 3000;

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/html" });
  res.end("<h1>Hello from Node.js inside Docker 🐳</h1>");
});

server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

## 📦 Step 3: `package.json`

Initialize Node project:

```json
{
  "name": "node-docker-app",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  }
}
```

*(You can create this quickly using `npm init -y` and then edit it.)*

---

## 🧱 Step 4: `Dockerfile`

```dockerfile
# Use official Node.js LTS image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package.json
COPY package.json .

# Install dependencies
RUN npm install

# Copy app files
COPY . .

# Expose port
EXPOSE 3000

# Run the app
CMD ["npm", "start"]
```

---

## ⚙️ Step 5: Build and Run Container

### Build the Docker image

```bash
docker build -t node-app .
```

### Run the container

```bash
docker run -d -p 8080:3000 node-app
```

Now open your browser and go to 👉
**[http://localhost:8080](http://localhost:8080)**

You’ll see:

> “Hello from Node.js inside Docker 🐳”

---

## ✅ Optional: Add `.dockerignore`

To speed up builds, add a `.dockerignore` file:

```
node_modules
npm-debug.log
Dockerfile
.dockerignore
```

---

Would you like me to extend this example to use **Express.js** (for serving an HTML page or API endpoint)?
