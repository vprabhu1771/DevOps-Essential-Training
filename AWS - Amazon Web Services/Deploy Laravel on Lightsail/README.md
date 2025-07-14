Deploying a Laravel application on **Amazon Lightsail** involves several steps. Here's a full guide to get you from local development to live deployment on Lightsail.

---

## ✅ Step 1: Launch a Lightsail Instance

1. Go to [Amazon Lightsail](https://lightsail.aws.amazon.com/).
2. Click **Create instance**.
3. Choose:
   - Platform: **Linux/Unix**
   - Blueprint: Choose **LAMP (PHP + MySQL)** (or plain Ubuntu if you want to set everything up manually).
   - Instance Plan: Choose your preferred specs.
4. Name your instance and click **Create instance**.

---

## ✅ Step 2: Connect to the Instance

1. Once the instance is running, click **Connect using SSH** from the Lightsail dashboard.
2. You're now in the terminal of your instance.

---

## ✅ Step 3: Prepare the Server (If LAMP not used)

If you chose **Ubuntu** (not LAMP), you'll need to install:

```bash
sudo apt update
sudo apt install apache2 php php-mbstring php-xml php-bcmath php-curl php-mysql php-zip unzip git curl mysql-server -y
```

Then install Composer:

```bash
cd ~
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

---

## ✅ Step 4: Upload Laravel Project

You can upload your Laravel project using **SFTP** or **Git**.

### Option 1: Using Git

```bash
cd /var/www
sudo git clone https://github.com/your-repo/your-laravel-app.git
cd your-laravel-app
```

### Option 2: Upload via SFTP
Use FileZilla or Cyberduck:
- Host: Your Lightsail IP
- Protocol: SFTP
- Username: `ubuntu` or `bitnami`
- Key File: Use the `.pem` file from your AWS SSH keys

---

## ✅ Step 5: Configure Laravel

```bash
cd /var/www/your-laravel-app

# Install dependencies
composer install

# Copy .env
cp .env.example .env

# Generate app key
php artisan key:generate
```

Set proper permissions:

```bash
sudo chown -R www-data:www-data .
sudo chmod -R 775 storage bootstrap/cache
```

---

## ✅ Step 6: Configure Apache

Edit the Apache config file:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Replace the `DocumentRoot` with:

```apache
DocumentRoot /var/www/your-laravel-app/public
<Directory /var/www/your-laravel-app/public>
    AllowOverride All
</Directory>
```

Enable mod_rewrite:

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

---

## ✅ Step 7: Set Up MySQL

1. Login to MySQL:

```bash
sudo mysql
```

2. Create database and user:

```sql
CREATE DATABASE laravel_db;
CREATE USER 'laravel_user'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON laravel_db.* TO 'laravel_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

3. Update `.env`:

```
DB_DATABASE=laravel_db
DB_USERNAME=laravel_user
DB_PASSWORD=yourpassword
```

---

## ✅ Step 8: Migrate and Seed

```bash
php artisan migrate --seed
```

---

## ✅ Step 9: Set Up Domain (Optional)

In Lightsail:
- Go to Networking
- Attach a static IP
- Update your DNS records to point your domain to that static IP

---

## ✅ Step 10: Secure Your Site (Optional)

Use Let’s Encrypt for SSL:

```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache
```

---

Let me know if you want a **bash script** to automate all of this or help configuring your `.env` or domain setup.