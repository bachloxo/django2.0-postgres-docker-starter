# Django 2.0, PostgreSQL and Docker

Minimal Docker dev stack for Django 2.0 with PostgreSQL.

## Installation

Build Docker image, create an empty PostgreSQL database, create a Django project:

```
docker-compose build
docker-compose up -d pgsql
docker-compose exec pgsql psql -Upostgres
create database django;
\q
docker-compose run --rm -u $(id -u) django django-admin startproject project .
```

Replace `DATABASES` setting in generated `django/project/settings.py` file with the following:

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'django',
        'USER': 'postgres',
        'PASSWORD': 'Wibble123!',
        'HOST': 'pgsql',
        'PORT': '5432',
    }
}
```

Run migrations, create an admin user, start the Django runserver:

```
docker-compose run --rm django ./manage.py migrate
docker-compose run --rm django ./manage.py createsuperuser
docker-compose up -d django
```

Django should now be available on http://localhost:8000
