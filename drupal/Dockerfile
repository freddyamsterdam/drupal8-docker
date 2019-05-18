FROM composer:latest

# Parallel downloads
RUN composer global require hirak/prestissimo

# Set working directory
WORKDIR /opt/src

# Build
COPY ./src/composer.json /opt/src/composer.json
COPY ./src/composer.lock /opt/src/composer.lock
COPY ./src/scripts /opt/src/scripts
RUN composer install --ignore-platform-reqs

# Copy codebase and settings
COPY ./src /opt/src
COPY ./.salt /opt/src/.salt
COPY ./drupal/services.yml /opt/src/web/sites/default/services.yml
COPY ./drupal/settings.php /opt/src/web/sites/default/settings.php

# Create directories
RUN mkdir -p /opt/src/web/sites/default/sync
RUN mkdir -p /opt/src/web/sites/default/files

# Fix permissions
RUN chmod -R 777 /opt/src/web/sites/default/files
RUN chown -R ${APACHE_RUN_USER}:${APACHE_RUN_GROUP} /opt/src/web

CMD echo "Install complete"
