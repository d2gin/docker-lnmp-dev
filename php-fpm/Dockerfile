FROM php:7.4-fpm AS dev-php
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini \
# 依赖更新
&& apt update -y \
# && apt upgrade -y \
# 安装常用工具
&& apt install -y vim procps wget zip unzip openssl zlib1g-dev \
# gd
libpng-dev \
# sqlite3
sqlite3 libsqlite3-dev \
# soap
libxml2 libxml2-dev \
# zip
libzip-dev \
# imap
libc-client-dev libkrb5-dev \
# redis
libzstd-dev \
# openssl相关
libssl-dev libcurl4-openssl-dev \
&& docker-php-source extract \
# 安装进程扩展
&& docker-php-ext-install pcntl \
&& docker-php-ext-install pdo_mysql \
&& docker-php-ext-install pdo_sqlite \
&& docker-php-ext-install soap \
&& docker-php-ext-install zip \
&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
&& docker-php-ext-install imap \
&& docker-php-ext-install gd \
&& docker-php-ext-install exif \
&& docker-php-ext-install bcmath \
# 安装swoole
# 通过docker-php-ext-install安装
&& wget -O ~/swoole.tgz https://pecl.php.net/get/swoole-4.8.12.tgz \
&& mkdir -m 0777 -p /usr/src/php/ext/swoole \
&& tar -xvf ~/swoole.tgz -C ~/ \
&& mv ~/swoole-4.8.12/* /usr/src/php/ext/swoole \
&& docker-php-ext-configure swoole --enable-openssl --enable-swoole-curl --enable-http2 --enable-swoole-json \
&& docker-php-ext-install swoole \
&& rm -rf /usr/src/php/ext/swoole \
&& rm -f ~/swoole.tgz \
# 或者通过pecl安装
# && pecl install -D 'enable-swoole-json="yes" enable-swoole-curl="yes" enable-openssl="yes" enable-http2="yes" enable-mysqlnd="no" enable-sockets="no" enable-cares="no"' swoole-4.8.12 \
# && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/php-ext-swoole.ini \
# 安装redis扩展
&& yes|pecl install igbinary \
&& echo "extension=igbinary.so" > /usr/local/etc/php/conf.d/php-ext-igbinary.ini \
&& yes|pecl install redis \
&& echo "extension=redis.so" > /usr/local/etc/php/conf.d/php-ext-redis.ini \
# 安装composer
&& wget -O ~/composer-setup.php https://install.phpcomposer.com/installer \
&& php ~/composer-setup.php \
&& mv composer.phar /usr/local/bin/composer \
&& rm -f ~/composer-setup.php
WORKDIR /var/www
# ENTRYPOINT ["/bin/bash"]