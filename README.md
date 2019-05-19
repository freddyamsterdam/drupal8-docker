# Drupal 8 and Docker Compose :fire::fire::fire::fire:
A helping hand to get Drupal 8 containerised using Docker.

## Prerequisites :v:
- [Docker](https://docs.docker.com/install/) and [Docker Compose](https://docs.docker.com/compose/install/) are installed on your system;
- [Composer Dependency Manager for PHP](https://getcomposer.org) is installed on your machine;
- Basic knowledge of aforementioned tools.

## <a name="run"></a> Run :shoe:
If you haven't installed Drupal yet, follow [the steps](#steps) below. Otherwise, you can run:

```shell
# To start in Foreground
$ docker-compose up
# To start in background
$ docker-compose up -d
# To stop
$ docker-compose down
# To stop and remove volumes (-v) and images (-rmi)
$ docker-compose down -v -rmi
```

## <a name="steps"></a> Steps :feet:
If you have already completed these steps, check [the run section](#run) to start and stop your containers.

To get started, follow the steps in order. If you don't...

![errors everywhere](https://www.craghoppers.com/community/wp-content/uploads/2016/02/errors-everywhere-meme.png)

All commands must be executed from the project root. The exception to this, being `docker-compose` commands – these can be run from anywhere in the project directory.

Let's crack on shall we? :rocket:

### 1. Get Drupal
See [Drupal Project](https://github.com/drupal-composer/drupal-project) for more information, documentation, etc.

```bash
$ composer create-project drupal-composer/drupal-project:8.x-dev src --no-interaction
```
Running the above command will put a Drupal installation into a directory named `src` in the project root. This entire project hinges on that fact.

Keep in mind that if you want to use a different Drupal installation, you may need to make substantial adjustments to both the [Apache Dockerfile](/apache/Dockerfile) and the [Drupal Dockerfile](/drupal/Dockerfile), and potentially other files, too.

### 2. Hash salt
Next up, let's generate a hash salt and save it to a file (`.salt`).

```bash
$ (openssl rand -base64 64 | tr -d '\n') > .salt
```

By default the `.salt` file is ignored by `.gitignore` in the project root directory. You can disable this if you'd like the file to be tracked.

The `.salt` file is read into the `$settings['hash_salt']` variable in [settings.php](/drupal/settings.php#L104).

### 3. Environment variables
The `.env` file contains a variety of environment variables ranging from database configuration to site domain configuration.

```bash
$ cp .env.example .env
```

Proceed to edit `.env` in your favourite editor. All of the variables are fairly self explanatory.

### 4. Build
Let's build all of the services.

```bash
$ docker-compose build
```

### 5. Up
```bash
$ docker-compose up -d
```

### 6. Install Drupal
Open your browser, navigate to http://127.0.0.1:8080 and complete the  Drupal installation.

:bulb: Alternatively, you could import an existing database by connecting to MariaDB with your favourite MySQL client.

### 7. Down
You can use -v to remove volumes when you're done, but take heed: this will delete your database, too.

```bash
$ docker-compose down
```

## Troubleshooting :japanese_ogre::gun:
Sometimes stuff just doesn't work out, so here are some useful commands to help you out.

### Container Logs
Logs from all containers
```bash
$ docker-compose logs
```
Logs from specific containers
```bash
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
