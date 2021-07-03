# AECB

## Requirements

You just need Docker (recommended version: 20.10.7) and Docker Compose (recommended version: 2.0.0).

## Configuration

To start using this app you need to add some configurations in environment files. When I want to add dummy values to be replaced in the variables, these have the format `ALL_IN_CAPS_WITH_LOW_DASHES`.

### AECB Backend

Inside of the aecb-backend folder you need to add the `.env` or `.dev.env` file depending on which kind
of environment you want to run to configure properly the Django app, but you also need to configure the database with a file named `.postgres.env` also located in the aecb-backend submodule.

Follow this structure on the `.env` file

```python
DJANGO_ENV="production"
DJANGO_SECRET_KEY="YOUR_SECRET_KEY_HERE"
DJANGO_ALLOWED_HOSTS="*"
DJANGO_DB_BACKEND="django.db.backends.postgresql"
DJANGO_DB_NAME="YOUR_DATABASE_NAME" # Same value as POSTGRES_DB in the .postgres.env file
DJANGO_DB_USER="YOUR_DATABASE_USER" # Same value as POSTGRES_USER in the .postgres.env file
DJANGO_DB_PASSWORD="YOUR_DATABASE_PASSWORD" # Same value as POSTGRES_PASSWORD in the .postgres.env file
DJANGO_DB_HOST="aecb-postgres"
DJANGO_DB_PORT=5432
DJANGO_ADMIN_DOMAIN="alumnos.udg.mx" # A Google domain that is going to represent the admin role e.g. alumnos.udg.mx
DJANGO_MEDIA_ROOT="/var/www/media"
AEBC_EXTERNAL_API_ENDPOINT="http://external-api:8080"
DJANGO_EMAIL_BACKEND="django.core.mail.backends.smtp.EmailBackend"
DJANGO_EMAIL_HOST="YOUR_SMTP_HOST" # Valid SMTP service host e.g. smtp.gmail.com
DJANGO_EMAIL_PORT=587 # Usually 587 or 465, depends on your smtp email host
DJANGO_EMAIL_HOST_USER="YOUR_ADMIN_EMAIL" # The email that is going to send the generated reports
DJANGO_EMAIL_HOST_PASSWORD="YOUR_ADMIN_EMAIL_PASSWORD"
DJANGO_TESTING_CLIENT="CLIENT_EMAIL@gmail.com" # A Google domain email to simulate a client account with the command python manage.py test_accounts
DJANGO_TESTING_ADMIN="ADMIN_EMAIL@DJANGO_ADMIN_DOMAIN" # A Google domain email to simulate a client account with the command python manage.py test_accounts
DJANGO_SU_PASSWORD="YOUR_SECURE_PASSWORD_HERE" # The password for the aecb-admin super user
```

For the `.dev.env` file is basically the same, you can use the same resources for the development environment but, it is important to select the development environment for Django.

```python
DJANGO_ENV="development"
```

And the last thing to configure for the backend is the database, just add the `.postgres.env` file and follow this structure.

```python
POSTGRES_USER="aecb"
POSTGRES_PASSWORD="YOUR_DATABASE_PASSWORD"
POSTGRES_DB="aecb"
```

### AECB Frontend

For the frontend you will need to create a Google Firebase project before because this application uses just Google based auth.

```python
REACT_APP_BACKEND_API="http://localhost:8000"
REACT_APP_FIREBASE_API_KEY="FIREBASE_API_KEY"
REACT_APP_FIREBASE_AUTH_DOMAIN="FIREBASE_AUTH_DOMAIN"
REACT_APP_FIREBASE_PROJECT_ID="FIREBASE_PROJECT_ID"
REACT_APP_FIREBASE_STORAGE_BUCKET="FIREBASE_STORAGE_BUCKET"
REACT_APP_FIREBASE_MESSAGING_SENDER_ID="FIREBASE_MESSAGING_SENDER_ID"
REACT_APP_FIREBASE_APP_ID="FIREBASE_APP_ID"
REACT_APP_FIREBASE_MEASUREMENT_ID="FIREBASE_MEASUREMENT_ID" # Optional
REACT_APP_GOOGLE_AUTH_CLIENT_ID="GOOGLE_AUTH_CLIENT_ID"
REACT_APP_ADMIN_DOMAIN="SAME_AS_DJANGO_ADMIN_DOMAIN"
```

## Build

Once you have the `.env`, `.dev.env` and `.postgres.env` files inside of the `aecb-backend` folder and also the diferent `.env` file with the Firebase data inside of `aecb-frontend` you are ready to build. Located in the `aecb` repository execute the command `docker compose -f docker-compose-prod.yaml build`. You can change the file to `docker-compose-dev.yaml` to build the images for the development enviroment.

## Run

To run the project you just need to execute the command `docker compose -f docker-compose-prod.yaml up` to run the production environment, once you have the production environment running, you can visit the frontend at `localhost:80` and the backend at `localhost:8000`.

For the development enviroment you need to run `docker compose -f docker-compose-dev.yaml up`, and when the environment is now up and running you can access the frontend in `localhost:3000` and the backend in `localhost:8000`.

To finish the execution you only need to `Ctrl+C` to exit the attached mode and run the same command that you used to run just changing the word `up` for `down`.

## About persistant data

The project uses Docker, so we need volumes to create persistant data, specifically for the media files and the database.

If you need to remove or clear the content just run `docker volume rm aecb_aecb-media` or `docker volume rm aecb_aecb-db` with your environment down.