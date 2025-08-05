Deploy React App to AWS EC2

```
https://www.youtube.com/watch?v=0gJQspNUaMM
```

AWS -> Login -> Console Home -> EC2 -> Launch instance


# Launch an instance

Name and tags

Name -> My Web Server


Application and OS Images (Amazon Machine Image)

Quick Start -> Ubuntu


Instance type

Instance type -> t2.micro Free tier eligible


Key pair (login)

Key pair name - required -> Create new key pair


# Create key pair

Key pair name -> some_name

Key pair type -> RSA

Private key file format -> pem


# Instances -> Connect -> SSH client -> Copy Paste Example ssh into Git Bash

sudo apt update

sudo apt install git

sudo apt install -y nodejs npm

# Clone React Project

git clone URL


Create key pair

# ----

Alright — let’s walk through **deploying a React app to AWS EC2** step-by-step.
I’ll assume you already have:

* A React project (created with `create-react-app` or Vite, etc.)
* An EC2 instance running Amazon Linux, Ubuntu, or similar
* SSH access to that instance
* A domain name (optional, but useful)

---

## **1. Build your React app locally**

React apps are static files (HTML, JS, CSS) after build, so you’ll first compile them.

```bash
cd your-react-app
npm install
npm run build
```

This will generate a `build/` folder (or `dist/` for Vite) containing static assets.

---

## **2. SSH into your EC2 instance**

Replace `your-key.pem` and `ec2-user@IP` with your info:

```bash
ssh -i your-key.pem ec2-user@your-ec2-ip
```

If you’re on Ubuntu EC2:

```bash
ssh -i your-key.pem ubuntu@your-ec2-ip
```

---

## **3. Install Node.js & npm (if needed)**

For Ubuntu/Debian:

```bash
sudo apt update
sudo apt install -y nodejs npm
```

For Amazon Linux:

```bash
sudo yum update -y
sudo yum install -y nodejs npm
```

(You actually don’t need Node to *serve* the static files, only if you plan to build on the server.)

---

## **4. Upload build files to EC2**

From your **local machine**, run:

```bash
scp -i your-key.pem -r build/* ec2-user@your-ec2-ip:/var/www/html/
```

*(If you don’t have `/var/www/html/` yet, we’ll create it when we set up Nginx.)*

---

## **5. Install & configure Nginx**

On EC2:

```bash
# Ubuntu
sudo apt install nginx -y

# Amazon Linux
sudo amazon-linux-extras enable nginx1
sudo yum install nginx -y
```

Start Nginx:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## **6. Deploy the static files**

Create the directory for Nginx:

```bash
sudo mkdir -p /var/www/html
```

Move your files there (if not already):

```bash
sudo cp -r ~/build/* /var/www/html/
```

---

## **7. Configure Nginx for React routing**

Open the default config:

```bash
sudo nano /etc/nginx/sites-available/default
```

Replace the server block with:

```nginx
server {
    listen 80;
    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri /index.html;
    }
}
```

Save and restart Nginx:

```bash
sudo systemctl restart nginx
```

---

## **8. Open EC2 Security Group for HTTP**

In AWS EC2 Console:

* Go to your instance → **Security Groups** → **Inbound rules** → Add:

  * Type: `HTTP`
  * Port: `80`
  * Source: `0.0.0.0/0`

---

## **9. Access your app**

Now visit:

```
http://your-ec2-public-ip
```

If you have a domain, point your DNS to the EC2 public IP.

---

✅ **Optional – Build directly on EC2**
Instead of building locally, you can push your code to EC2, then:

```bash
cd your-react-app
npm install
npm run build
sudo cp -r build/* /var/www/html/
```

---

If you want, I can give you **a version with HTTPS (SSL) using Let’s Encrypt** so your React app runs securely on EC2.
Would you like me to include that?

# ---

Gotcha — if `http://your-ec2-public-ip` is not loading, it usually means something in **security groups, firewall, or Nginx setup** is blocking access.

Let’s troubleshoot step-by-step:

---

## **1. Check EC2 Security Group**

Go to **AWS EC2 Console → Instances → Your Instance → Security → Security Groups → Inbound rules**.
Make sure you have:

| Type | Protocol | Port | Source    |
| ---- | -------- | ---- | --------- |
| HTTP | TCP      | 80   | 0.0.0.0/0 |
| HTTP | TCP      | 80   | ::/0      |
| SSH  | TCP      | 22   | Your IP   |

If HTTP (port 80) is missing → **Add rule → Save changes**.

---

## **2. Check Nginx is running**

SSH into EC2 and run:

```bash
sudo systemctl status nginx
```

If it’s not running:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## **3. Check EC2’s firewall (if using UFW)**

If you are on Ubuntu and have **UFW** enabled, allow HTTP:

```bash
sudo ufw allow 80
sudo ufw reload
```

---

## **4. Test locally inside EC2**

SSH into EC2 and run:

```bash
curl http://localhost
```

* If you see HTML output → Nginx is working.
* If you get a connection error → check Nginx logs:

```bash
sudo tail -n 20 /var/log/nginx/error.log
```

---

## **5. Verify files exist in `/var/www/html`**

Run:

```bash
ls -l /var/www/html
```

You should see `index.html` (from your React build).
If not, copy the build again:

```bash
sudo cp -r ~/build/* /var/www/html/
```

---

## **6. Restart everything**

```bash
sudo systemctl restart nginx
```

---

If you want, I can give you **a one-shot EC2 deployment command list** so your React app works immediately after SSH login.
Do you want me to prepare that?

