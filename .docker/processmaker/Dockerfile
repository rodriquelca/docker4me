FROM php:7.4-fpm-alpine


# Maintainer
LABEL maintainer="rodrigo@processmaker.com"

LABEL processmaker-stack="N275"

# Install extensions
RUN apk add --no-cache \
        freetype-dev \
        libjpeg-turbo-dev \
        libwebp-dev \
        libpng-dev \
        libzip-dev \
        zip \
        imap-dev \
        openssl-dev \
        git \
        bash \
        openssh-keygen \
        openssh-client

RUN docker-php-ext-configure gd --with-freetype --with-webp --with-jpeg  \
    && docker-php-ext-configure imap --with-imap --with-imap-ssl
 
RUN    docker-php-ext-install pdo_mysql bcmath opcache zip gd imap


# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Configure PHP
COPY .docker/php.ini $PHP_INI_DIR/conf.d/opcache.ini

# Use the default development configuration
RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

# Create user based on provided user ID
ARG HOST_UID
RUN adduser --disabled-password --gecos "" --uid $HOST_UID demo

# Switch to that user
USER demo   
