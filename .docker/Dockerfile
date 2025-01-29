FROM alpine:latest

LABEL maintainer="Caio Barilli <caiobarilli@gmail.com>"
LABEL description="alpine - custom webserver"

WORKDIR /var/www/html/

# Essentials
RUN echo "UTC" > /etc/timezone
RUN apk add --no-cache zip unzip curl sqlite nginx supervisor

# Installing bash
RUN apk add bash
RUN sed -i 's/bin\/ash/bin\/bash/g' /etc/passwd

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
    php83-redis \
    php83-posix \
    php83-pcntl \
    php83-pdo_sqlite

# Installing composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php83 composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN rm -rf composer-setup.php

# Configure supervisor
RUN mkdir -p /etc/supervisor.d/
RUN mkdir -p /var/log/supervisor

# Copy supervisor configuration
COPY ./supervisord.ini /etc/supervisor.d/supervisord.ini

# Create user and group for supervisor process
RUN addgroup -S supervisor && adduser -S supervisor -G supervisor
RUN chown -R supervisor:supervisor /var/log/supervisor /etc/supervisor.d

# Configure PHP
RUN mkdir -p /run/php/
RUN touch /run/php/php8.3-fpm.pid
RUN chown -R supervisor:supervisor /run/php/

COPY ./php.ini /etc/php83/php.ini
COPY ./conf/php-fpm.conf /etc/php83/php-fpm.conf
COPY ./conf/www.conf /etc/php83/php-fpm.d/www.conf

# Configure nginx
COPY ./conf/nginx.conf /etc/nginx/
COPY ./conf/nginx-php.conf /etc/nginx/modules/

RUN mkdir -p /var/lib/nginx/logs/ \
    mkdir -p /var/lib/nginx/tmp/client_body \
    mkdir -p /run/nginx/

RUN touch /run/nginx/nginx.pid

RUN chown -R supervisor:supervisor /var/lib/nginx /etc/nginx /run/nginx /run/nginx/nginx.pid /var/log/nginx

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
    ln -sf /dev/stderr /var/log/nginx/error.log


# Building process

COPY . .

EXPOSE 8000

USER supervisor

CMD ["supervisord", "-c", "/etc/supervisor.d/supervisord.ini"]
