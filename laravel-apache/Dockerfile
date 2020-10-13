FROM php:7.1-apache

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

ENV APACHE_DOCUMENT_ROOT /var/www/html/public
ENV APACHE_LOG_DIR /var/www/html/storage/logs
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN pwd
# 自定义apache配置
COPY ./apache.conf /etc/apache2/sites-enabled/000-default.conf

#启用伪静态
# RUN ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enable/rewrite.load
RUN a2enmod rewrite

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
COPY ./opcache.ini $PHP_INI_DIR/conf.d/

# 安装Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# COPY entrypoint.sh /etc/entrypoint.sh

COPY --chown=www-data:www-data . /var/www/html/

EXPOSE 80 443


# ENTRYPOINT ["sh", "/var/www/html/entrypoint.sh"]
