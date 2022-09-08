FROM php:7.4-fpm-alpine


# Maintainer
LABEL maintainer="rodrigo@processmaker.com"

LABEL processmaker-stack="pm4"

# Install extensions
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
        sudo \
        bash \
        openssh-keygen \
        openssh-client \
        bzip2-dev \
        libmcrypt-dev \
        autoconf g++ imagemagick imagemagick-dev libtool make pcre-dev \
        nodejs npm

RUN docker-php-ext-configure gd --with-freetype --with-webp --with-jpeg  \
    && docker-php-ext-configure imap --with-imap --with-imap-ssl 
RUN pecl install mcrypt imagick
RUN docker-php-ext-enable mcrypt imagick
RUN docker-php-ext-install pdo_mysql opcache zip gd imap bz2 calendar mysqli soap pcntl exif

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/


# Install PHP extensions
RUN install-php-extensions ldap

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer


# Configure PHP
COPY .docker/opcache.ini $PHP_INI_DIR/conf.d/opcache.ini

# Use the default development configuration
RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini


## configure php.ini ##
RUN  sed -i '/short_open_tag = Off/c\short_open_tag = On' $PHP_INI_DIR/php.ini ;
RUN  sed -i '/post_max_size = 8M/c\post_max_size = 24M' $PHP_INI_DIR/php.ini ;
RUN  sed -i '/upload_max_filesize = 2M/c\upload_max_filesize = 24M' $PHP_INI_DIR/php.ini ;
RUN  sed -i '/;date.timezone =/c\date.timezone = America/New_York' $PHP_INI_DIR/php.ini ;
RUN  sed -i '/expose_php = On/c\expose_php = Off' $PHP_INI_DIR/php.ini ;
RUN  sed -i '/memory_limit = 128M/c\memory_limit = 512M' $PHP_INI_DIR/php.ini ;
#edit error reporting
RUN  sed -i '/error_reporting = E_ALL/c\error_reporting = E_ALL & ~E_DEPRECATED' $PHP_INI_DIR/php.ini;



# Create user based on provided user ID
ARG HOST_UID
RUN adduser --disabled-password --gecos "" --uid $HOST_UID demo

# Switch to that user
USER root   
