FROM php:5.6-cli

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        liblzo2-dev \
        lzop \
        git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-configure pdo_mysql \
    && docker-php-ext-install pdo_mysql

RUN apt-get install -y libicu-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

RUN pecl channel-update pecl.php.net \
    && pecl install redis-4.3.0 \
    && docker-php-ext-enable redis \
    && php -m 

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/usr/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN apt-get install -y libmcrypt-dev \
    && docker-php-ext-configure mcrypt \
    && docker-php-ext-install mcrypt

RUN docker-php-ext-configure zip \
    && docker-php-ext-install zip

COPY uploads.ini /usr/local/etc/php/conf.d