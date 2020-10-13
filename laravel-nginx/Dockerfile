FROM php:7.1-fpm

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

RUN apt-get update -y \
    && apt-get install -y nginx

COPY ./laravel-nginx/nginx-site.conf /etc/nginx/sites-enabled/default

RUN apt-get update \
    # 相关依赖必须手动安装
    && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    vim

# 安装 opcache gd 图库
RUN docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache gd

# 配置 opcache
COPY ./laravel-nginx/opcache.ini $PHP_INI_DIR/conf.d/

# 安装Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY --chown=www-data:www-data . /var/www/html

WORKDIR /var/www/html

EXPOSE 80 443

# ENTRYPOINT ["sh", "/var/www/html/entrypoint.sh"]