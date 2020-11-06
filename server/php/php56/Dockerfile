# PHP Version
ARG PHP_VERSION=5.6.40-zts-jessie

# The Base Image
FROM php:${PHP_VERSION}

# Author Information
LABEL maintainer="Uncle Qiu <poetuncle@gmail.com>"

# Software env
ENV SOFTWARE_LIST \
        vim \
        curl \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libssl-dev \
        libzip-dev \
        libmcrypt-dev

#--------------------------------------------------------------------------
# Change Apt Source And Backup
#--------------------------------------------------------------------------
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
        echo "deb http://mirrors.163.com/debian jessie main" > /etc/apt/sources.list && \
        echo "deb http://mirrors.163.com/debian-security jessie/updates main" >> /etc/apt/sources.list && \
        echo "deb http://mirrors.163.com/debian jessie-updates main" >> /etc/apt/sources.list && \
        apt-get update -yqq

#--------------------------------------------------------------------------
# Install Software
#--------------------------------------------------------------------------       
RUN set -eux; \
    apt-get install -y --no-install-recommends \
            ${SOFTWARE_LIST}; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \ 
    pecl channel-update pecl.php.net; \
    # Install PECL extention (redis|mongodb|zip)
    pecl install -o -f redis-4.2.0 mongodb-1.2.8 zip; \
    docker-php-ext-enable redis mongodb zip; \
    rm -rf /tmp/pear; \
    docker-php-ext-install -j$(nproc) bcmath; \
    # Install the PHP pdo_mysql extention
    docker-php-ext-install pdo_mysql; \
    # Install the PHP pdo_pgsql extention
    docker-php-ext-install pdo_pgsql; \
    # Install the PHP gd library
    docker-php-ext-configure gd \
            --with-jpeg-dir=/usr/lib \
            --with-freetype-dir=/usr/include/freetype2; \
    docker-php-ext-install gd; \
    php -r 'var_dump(gd_info());'
