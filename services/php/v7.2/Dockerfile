FROM composer:latest
FROM php:7.2-fpm-alpine3.10
LABEL maintainer="hopher.hong <hopher.hong@qq.com>"

# setting timezone
ENV TIMEZONE Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

# setting alpine source mirror
RUN printf "\
http://mirrors.aliyun.com/alpine/v3.10/main\n\
http://mirrors.aliyun.com/alpine/v3.10/community\n\
" > /etc/apk/repositories

# Environments
ENV PHP_TIMEZONE Asia/Shanghai
ENV PHP_MEMORY_LIMIT 512M
ENV PHP_MAX_UPLOAD 50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_EXECUTION_TIME 30
ENV PHP_MAX_POST 100M
ENV PHP_DISPLAY_ERRORS On
ENV PHP_LOG_ERRORS On
ENV PHP_ENABLE_XDEBUG 0
ENV PHP_EXPOSE_PHP On
ENV PHPIZE_DEPS autoconf dpkg-dev dpkg file g++ gcc libc-dev make pkgconf re2c git
ENV COMPOSER_ALLOW_SUPERUSER 1

RUN apk upgrade --no-cache && apk add --no-cache bash imagemagick nano shadow tzdata

# Preparing
RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS coreutils \
\
# PHP GD
&& apk add --no-cache libpng libjpeg-turbo freetype libxpm \
&& apk add --no-cache --virtual .build-deps libpng-dev libjpeg-turbo-dev libxpm-dev freetype-dev \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-xpm-dir=/usr/include/ \
&& docker-php-ext-install gd \
\
# PHP Zip
&& apk add --no-cache libzip \
&& apk add --no-cache --virtual .build-deps zlib-dev \
&& docker-php-ext-configure zip \
&& docker-php-ext-install zip \
\
# PHP mysqli
&& docker-php-ext-install mysqli \
\
# PHP pdo_mysql
&& docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
&& docker-php-ext-install pdo_mysql \
\
# PHP bcmath
&& docker-php-ext-install bcmath \
\
# PHP opcache
&& docker-php-ext-install opcache \
\
# Cleaning
&& apk del .build-deps

# 安装 redis 扩展
ENV REDIS_VERSION 5.1.1
RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$REDIS_VERSION.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-* /usr/src/php/ext/redis
RUN docker-php-ext-install redis

# Set config parameters
RUN apk add --no-cache --virtual .temp py-pip \
  && pip install --no-cache-dir crudini \
  && crudini --set $PHP_INI_DIR/php.ini Date date.timezone '${PHP_TIMEZONE}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP memory_limit '${PHP_MEMORY_LIMIT}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP display_errors '${PHP_DISPLAY_ERRORS}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP log_errors '${PHP_LOG_ERRORS}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP upload_max_filesize '${PHP_MAX_UPLOAD}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP max_file_uploads '${PHP_MAX_FILE_UPLOAD}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP post_max_size '${PHP_MAX_POST}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP expose_php '${PHP_EXPOSE_PHP}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP max_execution_time '${PHP_MAX_EXECUTION_TIME}' \
  && crudini --set $PHP_INI_DIR/php.ini PHP cgi.fix_pathinfo 0 \
  && crudini --set $PHP_INI_DIR/../php-fpm.d/www.conf www listen 9000 \
  && crudini --set $PHP_INI_DIR/../php-fpm.d/www.conf www pm.max_children 20 \
  && crudini --set $PHP_INI_DIR/../php-fpm.d/www.conf www pm.status_path /status \
  && crudini --set $PHP_INI_DIR/../php-fpm.d/www.conf www catch_workers_output yes \
  && apk del .temp

# Composer
COPY --from=0 /usr/bin/composer /usr/bin/composer

RUN mkdir /app

# setting workdir
WORKDIR /app    