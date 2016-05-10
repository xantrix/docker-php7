FROM php:7.0.6-fpm

RUN \
  apt-get update && \
  apt-get install -y \
  curl wget nano unzip bzip2 locate

# Install New Relic daemon
RUN wget -O - https://download.newrelic.com/548C16BF.gpg | apt-key add - && \
    echo "deb http://apt.newrelic.com/debian/ newrelic non-free" > /etc/apt/sources.list.d/newrelic.list

RUN apt-get update && \
    apt-get -yq install newrelic-php5

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

#COPY php.ini    /usr/local/etc/php/conf.d/

# Setup environment variables for initializing New Relic
ENV NR_INSTALL_PHPLIST "/usr/local/bin"
ENV NR_INSTALL_SILENT 1
ENV NR_INSTALL_KEY ""
ENV NR_APP_NAME "Appname"

# Enabling APM metrics for ${NR_APP_NAME}"
RUN newrelic-install install

EXPOSE 9000