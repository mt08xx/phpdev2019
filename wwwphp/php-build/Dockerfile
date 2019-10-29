FROM php:7.3-apache-stretch
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN docker-php-ext-install gettext && docker-php-ext-enable gettext
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libc-client2007e-dev libkrb5-dev \
        zlib1g-dev libicu-dev g++ \
        ssmtp && \
    rm -r /var/lib/apt/lists/*
RUN docker-php-ext-install gd
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
    docker-php-ext-install imap
RUN docker-php-ext-configure intl \
    && docker-php-ext-install intl
RUN pecl install apcu && docker-php-ext-enable apcu
RUN docker-php-ext-install opcache 
RUN pecl install xdebug && docker-php-ext-enable xdebug
RUN a2enmod rewrite
