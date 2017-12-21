FROM php:7.1.12-fpm-jessie
MAINTAINER jieyang <admin@jevic.cn>
ENV EFRESHED_AT 2017-12-21

ENV TZ=Asia/Shanghai

RUN set -xe \
    && echo "构建依赖" \
    && buildDeps=" \
        build-essential \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev" \
    && echo "运行依赖" \
    && runtimeDeps=" \
        libssl-dev \
        libfreetype6 \
        libjpeg62-turbo \
        libmcrypt4 \
    	libxml2-dev \
    	libxslt-dev \
        libpng12-0" \
    && echo "安装 PHP 以及编译构建组件所需包" \
    && DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get install -y ${runtimeDeps} ${buildDeps} --no-install-recommends \
    && echo "编译安装 php 组件" \
    && docker-php-ext-install iconv mcrypt mysqli pdo pdo_mysql \
       zip bcmath gettext pcntl shmop sysvsem xmlrpc xsl soap sockets \
    && docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && pecl install redis grpc swoole mongodb \
    && echo "开启扩展" \
    && docker-php-ext-enable redis swoole opcache mongodb grpc \
    && echo "清理" \
    && apt-get purge -y --auto-remove \
        -o APT::AutoRemove::RecommendsImportant=false \
        -o APT::AutoRemove::SuggestsImportant=false \
        $buildDeps \
    && rm -rf /var/cache/apt/archives/* \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/*
