FROM php:7.4-fpm

ARG USERNAME=www-data
ARG GROUPNAME=www-data
ARG UID=1000
ARG GID=1000

COPY php.ini /usr/local/etc/php/

#pdo_mysqlのインストール
#libpq-dev+pdo_pgsql
#herokuのインストール

RUN apt-get update && apt-get install -y \
  unzip \
  libzip-dev \
  zlib1g-dev \
  libpq-dev \
  git \
  make \
  curl \
  xz-utils \
  file \
  && docker-php-ext-install \
  zip \
  pdo_mysql \
  pdo_pgsql \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* 

#Composerのインストール
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#node.jsのインストール
RUN curl -SL https://deb.nodesource.com/setup_lts.x | bash
RUN apt-get install -y nodejs && \
  npm install -g npm@latest

RUN usermod -u 1000 www-data \
    && groupmod -g 1000 www-data

RUN chown ${USERNAME}:${GROUPNAME} -R /var/www && \
    chmod 770 /var/www
#1個上位階層に置かないとnpmインストール時にエラーが発生。

USER $USERNAME

RUN echo 'max_execution_time = 600' >> /usr/local/etc/php/conf.d/docker-php-maxexectime.ini;
  
WORKDIR /var/www/html