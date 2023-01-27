FROM php:8.1-apache

ARG DEBIAN_FRONTEND=noninteractive

# install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# system dependencies
RUN apt-get update \
    && apt-get install -qq --yes --no-install-recommends \
    apt-utils \
    libxml2-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libxpm-dev \
    libwebp-dev \
    libzip-dev \
    libcurl4-openssl-dev \
    file \
    g++ \
    make \
    uuid-runtime \
    git \
    zip \
    unzip \
    libonig-dev \
    && apt-get autoremove \
    && apt-get clean \
    && apt-get autoclean

# php extensions
RUN docker-php-ext-install -j$(nproc) \
    dom \
    gd \
    simplexml \
    xml \
    pdo_mysql \
    curl \
    mbstring \
    opcache \
    zip

# composer memory limit
RUN cd /usr/local/etc/php/conf.d/ && \
  echo 'memory_limit = -1' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini

RUN pecl install redis &&\
    docker-php-ext-enable redis

# enable apache modules and
# test apache configuration
RUN a2enmod rewrite &&\
    apache2ctl configtest

WORKDIR /var/www/html/laravel

# RUN git clone https://github.com/laravel/laravel.git

# expose http and https ports
EXPOSE 80

# command
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
