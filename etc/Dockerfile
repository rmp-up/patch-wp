FROM php:7.3-apache

RUN apt update

# WP-CLI
RUN apt install -y mysql-client less

RUN a2enmod rewrite

# WordPress
RUN curl -sL https://deb.nodesource.com/setup_13.x | bash - # to build test environment
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN pecl install xdebug && echo "\
zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20180731/xdebug.so \n\
xdebug.remote_host=10.221.13.1\n\
xdebug.idekey=PHPSTORM\n\
xdebug.remote_enable=1\n\
xdebug.remote_autostart=1\n\
xdebug.profiler_enable_trigger=1\n\
" > /usr/local/etc/php/conf.d/custom.ini

# Composer
RUN apt install -y zip libzip-dev && docker-php-ext-install zip && docker-php-ext-enable zip
RUN curl -o composer-setup.php https://getcomposer.org/installer \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

WORKDIR /var/www
