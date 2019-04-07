FROM php:7.2.5-apache

MAINTAINER Zsolt

COPY .docker/vhost.conf /etc/apache2/sites-available/000-default.conf

WORKDIR /srv/www

RUN docker-php-ext-install mbstring pdo pdo_mysql \
    && chown -R www-data:www-data /srv/www \
    && a2enmod rewrite \
    && apt-get -y update \
    && apt-get -y install dialog apt-utils vim nano curl libxml2-dev net-tools libfontconfig1 libxrender1 libxext6 git zip unzip \
    && docker-php-ext-install soap \
    && yes | pecl install xdebug

RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
    && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
    # Make sure we're installing what we think we're installing!
    && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
    && php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --snapshot \
    && rm -f /tmp/composer-setup.*

COPY .docker/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
