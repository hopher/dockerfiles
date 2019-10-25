FROM php:7.2-fpm-alpine
LABEL maintainer="hopher.hong <565983236@qq.com>"

# 设置服务根目录
ENV SERVICE_ROOT ./services/php/v7.2
# composer 允许root用户
# https://getcomposer.org/doc/03-cli.md#composer-allow-superuser
ENV COMPOSER_ALLOW_SUPERUSER=1

# 设置时区
ENV TIMEZONE Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

# 设置安装源镜像
RUN printf "\
http://mirrors.aliyun.com/alpine/v3.10/main\n\
http://mirrors.aliyun.com/alpine/v3.10/community\n\
" > /etc/apk/repositories

# 安装 composer && 设置国内代理
COPY $SERVICE_ROOT/composer.phar /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# -- virtual 表示虚拟包，可以在下一个命令删除
RUN apk add --no-cache --virtual .ext-deps libxml2 libzip freetype libpng libjpeg-turbo libxslt icu && \
	apk add --no-cache --virtual .build-deps shadow libxml2-dev libzip-dev freetype-dev libpng-dev libjpeg-turbo-dev libxslt-dev icu-dev g++ && \
	export CFLAGS="-I/usr/src/php" && \
	docker-php-ext-install bcmath calendar iconv mysqli opcache pdo_mysql dom xml xmlreader exif hash json posix sockets tokenizer zip \
		xsl intl soap && \
	docker-php-ext-configure gd \
	--with-gd \
	--with-freetype-dir=/usr/include/ \
	--with-png-dir=/usr/include/ \
	--with-jpeg-dir=/usr/include/ && \
	NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
	docker-php-ext-install -j${NPROC} gd && \
#	/usr/sbin/usermod -u 100 www-data && \
#	/usr/sbin/groupmod -g 101 www-data && \
	apk del .build-deps

# 设置工作目录
WORKDIR /app    