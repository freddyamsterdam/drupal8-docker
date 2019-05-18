# Drupal 8 and Docker Compose
A helping hand to get Drupal 8 containerised using Docker.

## Prerequisites
- [Docker](https://docs.docker.com/install/) and [Docker Compose](https://docs.docker.com/compose/install/) are installed on your system;
- [Composer Dependency Manager for PHP](https://getcomposer.org) is installed on your machine;
- Basic knowledge of aforementioned tools.

## Steps
Follow steps in order. Run all commands, except `docker-compose` commands, from project root.

### 1. Get Drupal
See [Drupal Project](https://github.com/drupal-composer/drupal-project) for more information, documentation, etc.

```bash
$ composer create-project drupal-composer/drupal-project:8.x-dev src --no-interaction
```

### 2. Hash salt
First up, let's generate a hash salt and save it to a file (`.salt`).

```bash
$ (openssl rand -base64 64 | tr -d '\n') > .salt
```

By default the `.salt` file is ignored by `.gitignore` in the project root directory. You can disable this if you'd like the file to be tracked.

The `.salt` file is read into the `$settings['hash_salt']` variable in [settings.php](/drupal/settings.php#L104).

### 3. Environment variables
The `.env` file contains a variety of environment variables ranging from database configuration to site domain configuration. Variables speak for themselves.

```bash
$ cp .env.example .env
```

Proceed to edit `.env` in your favourite editor.

### 4. Build
```bash
$ docker-compose build
```

### 5. Up
```bash
$ docker-compose up -d
```

### 6. Install Drupal
Open your browser, navigate to http://127.0.0.1:8080 and install Drupal.

### 7. Down
You can use -v to remove volumes when you're done, but take heed: this will delete your database, too.

```bash
$ docker-compose down
```

## Troubleshooting
Sometimes stuff just doesn't work out, so here are some useful commands to help you out.

### Container Logs
```bash
$ docker-compose logs
$ docker-compose logs apache
$ docker-compose logs mariadb
$ docker-compose logs php-fpm
```

### List containers
```bash
$ docker-compose ps
```

### Execute shell command in container
```bash
$ docker-compose exec [SERVICE] [COMMAND]
$ docker-compose exec php-fpm printenv
$ docker-compose exec php-fpm cat /opt/src/web/sites/default/settings.php
```
