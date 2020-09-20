FROM php:7.4-fpm

RUN set -eux; \
		apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
            curl \
            libz-dev \
            libjpeg-dev \
            libpng-dev \
            libfreetype6-dev \
            libssl-dev \
            libmcrypt-dev \
            libzip-dev \
            zip \
            unzip; \
    apt-get clean;

RUN docker-php-ext-install \
    pdo_mysql \
    bcmath \
    opcache \
    zip;

# For Redis
RUN set -eux; \
		pecl -q install redis \
		&& docker-php-ext-enable redis;

# For XDebug
RUN set -eux; \
		pecl -q install xdebug \
    && docker-php-ext-enable xdebug;
COPY ./docker/php-fpm/xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

COPY ./docker/php-fpm/php.ini /usr/local/etc/php/php.ini

# Remove tmp files
RUN rm -r /tmp/* /var/cache/*
