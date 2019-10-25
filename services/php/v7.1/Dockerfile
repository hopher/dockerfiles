FROM php:7.1-fpm
LABEL maintainer="hopher.hong <565983236@qq.com>"

# 设置服务根目录
ENV SERVICE_ROOT ./services/php/v7.1
# composer 允许root用户
# https://getcomposer.org/doc/03-cli.md#composer-allow-superuser
ENV COMPOSER_ALLOW_SUPERUSER=1

# 设置时区
ENV TIMEZONE Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone


# 设置安装源镜像
ENV MIRROR_SOURCE=aliyun
COPY ./mirrors/$MIRROR_SOURCE/debian/stretch.list /etc/apt/sources.list

# 设置工作目录
WORKDIR /app

# 安装 composer && 设置国内代理
COPY $SERVICE_ROOT/composer.phar /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# 更新安装依赖包和PHP核心拓展
# 
# –no-install-recommends 参数来避免安装非必须的文件，从而减小镜像的体积
# libpcre3-dev 是 opcache 必须库
# libpq-dev 是 pdo_pgsql 必须库
RUN apt-get clean all && apt-get update && apt-get install -y \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libpq-dev \
        libpcre3-dev \
        libmcrypt-dev \
        libzip-dev \
        zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-configure zip --with-libzip \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install pdo_pgsql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && docker-php-ext-install -j$(nproc) iconv mcrypt    