```
https://www.youtube.com/watch?v=3XcIG5U9Aao
```

```
Desktop -> web

web -> index.html

web -> main.css

web -> main.js
```

```
systemctl status docker
```

```
docker ps
```

```
docker images
```

```
hub.docker.com
```

```
https://hub.docker.com/_/httpd
```

# Create a `Dockerfile` in your project
```
FROM httpd:2.4
COPY . /usr/local/apache2/htdocs/
```

```
docker build . -t web:latest
```

```
docker images
```

```
docker run -d -p 80:80 web:latest
```

```
docker ps
```