Here’s a **simple and complete guide to Dockerize a Django application** — including `Dockerfile`, `docker-compose.yml`, and database setup (PostgreSQL or MySQL).

---

## 🧱 Project Structure

Example Django app layout:

```
my_django_app/
│
├── manage.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── .env
└── myproject/
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```

---

## ⚙️ Step 1: Create `requirements.txt`

```txt
Django>=5.0,<6.0
gunicorn
psycopg2-binary  # for PostgreSQL
# or use mysqlclient if you prefer MySQL
```

---

## 🐳 Step 2: Create `Dockerfile`

```dockerfile
# Use official Python image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Prevent Python from writing .pyc files and buffering stdout
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy project files
COPY . .

# Expose Django's port
EXPOSE 8000

# Run Gunicorn server
CMD ["gunicorn", "myproject.wsgi:application", "--bind", "0.0.0.0:8000"]
```

---

## 🧩 Step 3: Create `.env` File

```env
DEBUG=1
SECRET_KEY=django-insecure-key
DB_NAME=django_db
DB_USER=django_user
DB_PASSWORD=django_pass
DB_HOST=db
DB_PORT=5432
```

---

## 🐘 Step 4: Create `docker-compose.yml` (PostgreSQL example)

```yaml
version: "3.9"

services:
  web:
    build: .
    container_name: django_web
    command: gunicorn myproject.wsgi:application --bind 0.0.0.0:8000
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    env_file:
      - .env
    depends_on:
      - db

  db:
    image: postgres:15
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
```

> 💡 For MySQL instead, replace the `db` service with:
>
> ```yaml
> db:
>   image: mysql:8
>   restart: always
>   environment:
>     MYSQL_DATABASE: ${DB_NAME}
>     MYSQL_USER: ${DB_USER}
>     MYSQL_PASSWORD: ${DB_PASSWORD}
>     MYSQL_ROOT_PASSWORD: root
>   ports:
>     - "3306:3306"
>   volumes:
>     - mysql_data:/var/lib/mysql
> volumes:
>   mysql_data:
> ```

---

## 🧠 Step 5: Update `settings.py`

```python
import os
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = os.environ.get('SECRET_KEY', 'fallback-secret')
DEBUG = os.environ.get('DEBUG') == '1'

ALLOWED_HOSTS = ['*']

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',  # or 'django.db.backends.mysql'
        'NAME': os.environ.get('DB_NAME'),
        'USER': os.environ.get('DB_USER'),
        'PASSWORD': os.environ.get('DB_PASSWORD'),
        'HOST': os.environ.get('DB_HOST'),
        'PORT': os.environ.get('DB_PORT'),
    }
}
```

---

## 🚀 Step 6: Build and Run

```bash
docker-compose build
docker-compose up
```

---

## 🧩 Step 7: Run Migrations & Create Superuser

In a new terminal:

```bash
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
```

Then open:
👉 **[http://localhost:8000](http://localhost:8000)**

---

Would you like me to tailor this setup for **MySQL** instead of PostgreSQL?
