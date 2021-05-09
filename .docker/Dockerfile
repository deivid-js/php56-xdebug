FROM php:5.6-apache
RUN docker-php-ext-install mysqli pdo pdo_mysql && docker-php-ext-enable pdo_mysql

COPY / /var/www/html
RUN a2enmod rewrite

RUN docker-php-ext-install sockets
RUN chown -R www-data:www-data /var/www
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=/usr/local/bin

RUN apt-get update
RUN apt-get install -y libzip-dev zlib1g-dev
RUN docker-php-ext-install zip

RUN apt-get --yes install git-all
RUN php /usr/local/bin/composer.phar config --global github-oauth.github.com 04d62c25ab831cda1243ae0d29d88b9ed0b1ed56

# xdebug
RUN BEFORE_PWD=$(pwd) \
    && mkdir -p /opt/xdebug \
    && cd /opt/xdebug \
    && curl -k -L https://github.com/xdebug/xdebug/archive/XDEBUG_2_5_5.tar.gz | tar zx \
    && cd xdebug-XDEBUG_2_5_5 \
    && phpize \
    && ./configure --enable-xdebug \
    && make clean \
    && sed -i 's/-O2/-O0/g' Makefile \
    && make \
    && make install \
    && cd "${BEFORE_PWD}" \
    && rm -r /opt/xdebug
RUN docker-php-ext-enable xdebug

RUN apt-get install --yes vim