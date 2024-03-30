FROM alpine:3.19

LABEL maintainer="Caio Barilli <caiobarilli@gmail.com>"
LABEL description="alpine:3.19 - custom webserver"

WORKDIR /var/www/html/

# Essentials
RUN echo "UTC" > /etc/timezone
RUN apk add --no-cache zip unzip curl sqlite nginx supervisor

# Installing bash
RUN apk add bash
RUN sed -i 's/bin\/ash/bin\/bash/g' /etc/passwd

# Install packages not yet updated for the current alpine version TODO remove when no longer needed
RUN echo 'https://dl-cdn.alpinelinux.org/alpine/v3.14/community' >> /etc/apk/repositories
RUN echo 'https://dl-cdn.alpinelinux.org/alpine/v3.14/main' >> /etc/apk/repositories

# Updating
RUN apk update && apk upgrade

# Installing PHP
RUN apk --no-cache add \
    php83 \
    php83-fpm \
    php83-opcache \
    php83-pecl-apcu \
    php83-mysqli \
    php83-pgsql \
    php83-json \
    php83-openssl \
    php83-curl \
    php83-zlib \
    php83-soap \
    php83-xml \
    php83-fileinfo \
    php83-phar \
    php83-intl \
    php83-dom \
    php83-xmlwriter \
    php83-xmlreader \
    php83-ctype \
    php83-session \
    php83-iconv \
    php83-tokenizer \
    php83-zip \
    php83-simplexml \
    php83-mbstring \
    php83-mysqli \
    php83-pdo \
    php83-pdo_mysql \
    php83-gd \
    php83-sqlite3 \
    php83-pdo_sqlite

# Installing SQLite
# RUN apk add --no-cache php-sqlite3 \
#     php8.1-sqlite3 \
#     apk update && apk upgrade

# Installing composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php83 composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN rm -rf composer-setup.php

# Installing Node.js and npm
RUN apk --no-cache add nodejs npm

# Configure supervisor
RUN mkdir -p /etc/supervisor.d/
COPY ./supervisord.ini /etc/supervisor.d/supervisord.ini

# Configure PHP
RUN mkdir -p /run/php/
RUN touch /run/php/php8.3-fpm.pid

COPY ./php.ini-production /etc/php83/php.ini
COPY ./php-fpm.conf /etc/php83/php-fpm.conf

# Configure nginx
COPY ./nginx.conf /etc/nginx/
COPY ./nginx-laravel.conf /etc/nginx/modules/

RUN mkdir -p /run/nginx/
RUN touch /run/nginx/nginx.pid

RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log
RUN ln -s /usr/bin/php83 /usr/bin/php

# Building process
COPY . .

EXPOSE 8000
CMD ["supervisord", "-c", "/etc/supervisor.d/supervisord.ini"]