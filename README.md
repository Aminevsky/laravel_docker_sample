This is my Docker project for working Laravel.

# NOTICE
**ONLY USE FOR DEVELOPMENT.**

I am not concerned for using in production environment, so this might cause unexpected results.

# Containers

This project consists of the following containers:

- workspace
  - In this container, you can run composer commands, artisan commands and more.
- httpd
  - Version: 2.4
- php-fpm
  - Version: 7.4
- mysql
  - Version: 5.7
- mysql-testing
  - Same above mysql container
  - This could be used in executing test codes.
- redis
  - Version: 6
- mailcatcher

# How to Use

At first, copy `.env.example` and edit it.
I **strongly** recommend to change username and password etc.

```
$ cp .env.example .env
$ vi .env
```

Next, start the workspace container. This will automatically start other containers such as `httpd`, `php-fpm`, `mysql`.

```
$ docker-compose up -d workspace
```

Next, enter the container and create Laravel project.

```
$ docker-compose exec workspace bash
$ composer create-project --prefer-dist laravel/laravel laravel
```

Finally, open http://localhost/. You will see Laravel default page.

# How to Use XDebug

Use port `9000`.

If you get an error that the port is already used, change the following lines.

1. `docker-compose.yaml`

```
php-fpm:
  ports:
    - 9000
  expose:
    - 9000
```

2. `docker/php-fpm/xdebug.ini`

```
xdebug.remote_port=9000
```

3. `docker/workspace/xdebug.ini`

```
xdebug.remote_port=9000
```

# How to Connect MySQL

Words beginning with a capital letter are set in `.env` file.

- Username
  - `MYSQL_USER`

- Password
  - `MYSQL_PASSWORD`

| Database Name             | Port No. |
| ------------------------- | ---------- |
| `MYSQL_DATABASE`         | 3306       |
| `MYSQL_DATABASE_TESTING` | 3307       |

# How to Connect Redis

Start redis container and login Redis console.
Replace `{REDIS_PASSWORD}` to the value set in your `.env` file.

```
$ docker-compose up -d redis
$ docker-compose exec redis bash
$ redis-cli -a {REDIS_PASSWORD}
```

# How to Use MailCather

Start mailcatcher container.

```
$ docker-compose up -d mailcatcher
```

Open http://localhost:1080/.
When you send mails, you receive it in above URL.
