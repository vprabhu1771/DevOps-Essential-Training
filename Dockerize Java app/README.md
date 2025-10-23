Here is the **correct Dockerfile** format for dockerizing a simple Java application like `sample.java` 👇

---

### ✅ Dockerfile (Fixed)

```dockerfile
# Use official OpenJDK image for build & run
FROM openjdk:21

# Set working directory inside container
WORKDIR /app

# Copy all files to working directory
COPY . /app

# Compile the Java program
RUN javac sample.java

# Command to run the application
CMD ["java", "sample"]
```

---

### ✅ How to Build & Run

Run these commands in the same folder where Dockerfile and `sample.java` exist:

```sh
docker build -t sample-app .
```

```sh
docker run --rm sample-app
```

---

### 📌 Optional Improvement (Multi-Stage Build for Production)

This produces a cleaner & smaller final image:

```dockerfile
FROM openjdk:21 AS builder
WORKDIR /app
COPY . .
RUN javac sample.java

FROM openjdk:21
WORKDIR /app
COPY --from=builder /app/sample.class .
CMD ["java", "sample"]
```

---

If you want, I can also help:
✅ Create a `.dockerignore`
✅ Add JAR packaging instead of `.java`
✅ Add environment variables / arguments

Would you like to dockerize using **JAR file** instead?
