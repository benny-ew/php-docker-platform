FROM php:7.4-apache
SHELL ["/bin/bash", "-c"]

RUN ln -s ../mods-available/{expires,headers,rewrite,ssl}.load /etc/apache2/mods-enabled/
COPY apache2.conf /etc/apache2/apache2.conf
COPY php.ini /usr/local/etc/php/php.ini

RUN apt update

RUN apt install -y apt-utils
RUN apt install -y libmcrypt-dev
RUN apt install -y libicu-dev
RUN apt install -y unzip

RUN docker-php-ext-install -j$(nproc) intl
RUN apt install -y curl
RUN apt install -y libpq-dev
RUN docker-php-ext-install pgsql
RUN docker-php-ext-install pdo_pgsql
RUN docker-php-ext-install fileinfo
RUN apt install -y libxml2-dev
RUN docker-php-ext-install xml
RUN apt install -y libzip-dev
RUN docker-php-ext-install zip
RUN docker-php-ext-install json
RUN docker-php-ext-install tokenizer
RUN apt install -y libssl-dev
RUN docker-php-ext-install ftp
RUN docker-php-ext-install opcache
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

RUN pecl install xdebug-2.9.8 \
    #&& docker-php-ext-enable xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_connect_back=off" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_log=/var/www/html/xdebug.log" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_handler=\"dbgp\"" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=\"docker.for.win.host.internal\"" >> /usr/local/etc/php/conf.d/xdebug.ini