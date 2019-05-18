FROM php:7.1.27-fpm-jessie
# Install modules
RUN apt-get update && apt-get install -y \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libmcrypt-dev \
	libpng-dev
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && docker-php-ext-install gd
RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install opcache
COPY ./php-fpm/php.ini /usr/local/etc/php/php.ini
RUN mkdir -p /opt/src/web
RUN chown -R www-data:www-data /opt/src/web
CMD ["php-fpm"]
