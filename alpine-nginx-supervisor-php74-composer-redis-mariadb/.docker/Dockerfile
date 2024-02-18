FROM alpine:3.14

LABEL maintainer="Caio Barilli <caiobarilli@gmail.com>"
LABEL description="alpine:3.14-custom-v1"

WORKDIR /var/www/html/

# Essentials
RUN echo "UTC" > /etc/timezone \
    && apk add --no-cache zip unzip curl sqlite nginx supervisor bash

# # Install packages not yet updated for the current alpine version
RUN echo 'https://dl-cdn.alpinelinux.org/alpine/v3.14/community' >> /etc/apk/repositories \
    && echo 'https://dl-cdn.alpinelinux.org/alpine/v3.14/main' >> /etc/apk/repositories

# Updating
RUN apk update && apk upgrade

# Installing PHP 7.4
RUN apk --no-cache add \
    php7 \
    php7-fpm \
    php7-opcache \
    php7-pecl-apcu \
    php7-mysqli \
    php7-pgsql \
    php7-json \
    php7-openssl \
    php7-curl \
    php7-zlib \
    php7-soap \
    php7-xml \
    php7-fileinfo \
    php7-phar \
    php7-intl \
    php7-dom \
    php7-xmlwriter \
    php7-xmlreader \
    php7-ctype \
    php7-session \
    php7-iconv \
    php7-tokenizer \
    php7-zip \
    php7-simplexml \
    php7-mbstring \
    php7-pdo \
    php7-pdo_mysql \
    php7-gd \
    php7-sqlite3 \
    php7-pdo_sqlite

# Installing Node.js and npm
RUN apk --no-cache add nodejs npm

# Configure supervisor
RUN mkdir -p /etc/supervisor.d/
COPY ./supervisord.ini /etc/supervisor.d/supervisord.ini

# Configure PHP
RUN mkdir -p /run/php/
RUN touch /run/php/php-fpm.pid

COPY ./php.ini-production /etc/php7/php.ini
COPY ./php-fpm.conf /etc/php7/php-fpm.conf

# Installing composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && rm -rf composer-setup.php

# Configure nginx
COPY ./nginx.conf /etc/nginx/
COPY ./nginx-laravel.conf /etc/nginx/modules/

RUN mkdir -p /run/nginx/
RUN touch /run/nginx/nginx.pid

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

# Building process
COPY . .

EXPOSE 8000
CMD ["supervisord", "-c", "/etc/supervisor.d/supervisord.ini"]
