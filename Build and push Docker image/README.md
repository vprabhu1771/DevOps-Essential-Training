## ✅ Build Docker Image

### 1️⃣ Login to Docker Hub

```sh
docker login
```

### 2️⃣ Build Image

Replace `yourdockerhubusername` with your username:

```sh
docker build -t yourdockerhubusername/laravel-app:latest .
```

## 3️⃣ Check your local images

Run:

```bash
docker images
```

You will see something like:

```
REPOSITORY                          TAG       IMAGE ID       CREATED       SIZE
laravel-app                          latest    abcdef12345   2 minutes ago  1.2GB
```

### 4 Push Image to Docker Hub

```sh
docker push yourdockerhubusername/laravel-app:latest
```
