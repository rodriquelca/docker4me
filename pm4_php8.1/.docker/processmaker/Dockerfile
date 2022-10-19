FROM php:8.1-fpm-alpine

# Install extra packages
# RUN apk --no-cache add bash mysql-client git 
RUN apk add --no-cache \
        freetype-dev \
        libjpeg-turbo-dev \
        libwebp-dev \
        libpng-dev \
        libzip-dev \
        libxml2-dev \
        zip \
        imap-dev \
        openssl-dev \
        git \
        bash \
        openssh-keygen \
        openssh-client \
        bzip2-dev \
        libmcrypt-dev \
        docker docker-compose


# RUN docker-php-ext-install pdo_mysql bcmath opcache gd imap
RUN docker-php-ext-configure gd --with-freetype --with-webp --with-jpeg  \
    && docker-php-ext-configure imap --with-imap --with-imap-ssl \
    && docker-php-ext-configure pcntl --enable-pcntl
RUN pecl install mcrypt
# Import extension installer
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/


# Install extensions
RUN install-php-extensions redis

RUN docker-php-ext-install pdo_mysql opcache zip gd imap bz2 calendar mysqli soap pcntl exif 
RUN docker-php-ext-enable mcrypt
RUN docker-php-ext-enable exif

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Configure PHP
COPY php.ini $PHP_INI_DIR/conf.d/opcache.ini

# Use the default development configuration
RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini



# Create user based on provided user ID
ARG HOST_UID
RUN adduser --disabled-password --gecos "" --uid $HOST_UID demo


# Switch to that user
USER root
