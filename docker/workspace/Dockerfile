FROM php:7.4-fpm

##################################################
# Create non-root user
##################################################

USER root

# Create group
ARG GID=1000
RUN groupadd -g ${GID} laravel

# Create user
ARG UID=1000
RUN useradd -m -u ${UID} laravel -g laravel

##################################################
# Install
##################################################

USER root

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
            unzip \
            vim; \
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

COPY ./docker/workspace/xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

##################################################
# Install Composer
##################################################

USER root

RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer;

# For speed-up
USER laravel
RUN composer config -g repos.packagist composer https://packagist.jp;

##################################################
# Install node.js and npm
##################################################

USER root

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt-get install -y nodejs && \
    npm install npm@latest -g;

##################################################
# Termination Process
##################################################

USER root

# Remove tmp files
RUN rm -r /tmp/* /var/cache/*

# Switch to Laravel user
USER laravel
