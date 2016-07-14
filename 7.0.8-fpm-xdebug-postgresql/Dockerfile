FROM php:7.0.8-fpm

RUN \
  apt-get update && \
  apt-get install -y \
  curl wget nano unzip bzip2 locate git npm

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libicu-dev \
        libicu52  \
    && docker-php-ext-install iconv \
    && docker-php-ext-install -j$(nproc) mcrypt \
    && docker-php-ext-install zip \
    && docker-php-ext-install exif \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo \
    && docker-php-ext-install intl \
    && docker-php-ext-install opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

#beta version support php7, change it with stable version in the future
RUN pecl install xdebug-beta \
    && docker-php-ext-enable xdebug

#postgres install
# https://github.com/allan-simon/docker-php7-fpm-postgresql/blob/master/Dockerfile
RUN buildDeps="libpq-dev libzip-dev " && apt-get update && apt-get install -y $buildDeps --no-install-recommends \
    && docker-php-ext-install pdo_pgsql pgsql

#npm, n, node, last version
RUN npm install -g npm && npm install -g n && n latest

#COPY php.ini    /usr/local/etc/php/conf.d/

EXPOSE 9000