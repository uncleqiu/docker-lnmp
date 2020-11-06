# PHP Version
ARG PHP_VERSION=7.3-fpm

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
# Change Apt Source And Backup And Install software
#--------------------------------------------------------------------------

RUN set -eux; \
    mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
        echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian buster main" > /etc/apt/sources.list && \
        echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main" >> /etc/apt/sources.list && \
        echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian buster-updates main" >> /etc/apt/sources.list && \
        rm -Rf /var/lib/apt/lists/* && \
        apt-get update -yqq && \
        apt-get upgrade -y; \
        apt-get install -y --no-install-recommends \
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
            libmcrypt-dev; \
    rm -rf /var/lib/apt/lists/*
RUN pecl install -o -f redis \
	&&  rm -rf /tmp/pear \
	&&  docker-php-ext-enable redis \
        && pecl install mongodb swoole xlswriter zip \ 
	&& docker-php-ext-enable mongodb swoole xlswriter zip
RUN set -eux; \ 
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
