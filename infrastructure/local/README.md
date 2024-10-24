# How Run Local Development Environment

## Prerequisites

### Docker

Install Docker and run Docker Desktop

### Setup DNS

Open `/etc/hosts` using your favorite editor.

For example:

```bash
sudo vim /etc/hosts
```

If you don't use `sudo` to edit the file, you might get an error.

After opening the file, add the following line:

```
127.0.0.1 local.tsubocho.com
```

### Git ignore

Add the following line to `.gitignore`:

```bash
infrastructure/local/database_data/*
infrastructure/local/nginx/ssl/*
```

## Setup

All step need to be done in the `infrastructure/local` directory

```
cd infrastructure/local
```

### 1. Set up variables

```
cp .env_example .env
```

### 2. Set up database configuration

Edit variables in `.env` to whatever you want.

For example:

```bash
DB_USER=foo
DB_PASSWORD=boo
```

### 3. Generate SSL Certificate

Run this command to generate SSL certificate for `localhost`.
Be aware you should in the `infrastructure/local/` directory.

```bash
cd nginx/ssl

openssl req -x509 -newkey rsa:2048 -nodes -keyout localhost.key -out localhost.crt -days 365 -subj "/C=TW/ST=Taiwan/L=Taipei/O=Company/CN=localhost"
```

This command will generate two files: `localhost.key` and `localhost.crt`.

## Run

> Be aware you should in the `infrastructure/local/` directory.

```
docker-compose up -d
npm run dev
```

After the command is done, you should be able to access the website at `https://local.tsubocho.com`.

## Troubleshooting

### Can not connect to PostgreSQL

Make sure you have set up the database configuration in `.env`.

```
php artisan db
```

If it shows `sqlite` then you should change the database configuration in `.env` to `pgsql`.

If it shows error `sh: line 0: exec: psql: not found`.
You can install `postgresql` and `psql` in your system, it mean your `.env` is configured correctly.

In this case, you can ignore this error.

Then open a terminal using the following command to connect to PostgreSQL:

```
cd infrastructure/local
docker-compose exec postgre psql -U admin_user -d tsubocho
```
