FROM php:7.4.6-fpm
ARG TZ=Asia/Tokyo

RUN set -xe; \
    apt-get update -yqq && \
    apt-get install -yqq --no-install-recommends \
    apt-utils vim gettext git \
    default-mysql-client \
    # for gd
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libxpm-dev \
    # for ImageMagick
    libmagickwand-dev \
    # for zip
    libzip-dev zip unzip \
    # for oniguruma
    libonig-dev \
    && docker-php-ext-install bcmath gettext mbstring mysqli pdo pdo_mysql zip \
    && docker-php-ext-configure mbstring --disable-mbregex \
    # gd
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-install -j$(nproc) gd \
    # imagick
    && pecl install imagick-3.4.3 \
    && docker-php-ext-enable imagick \
    # for ssl
    && apt-get update \
    && apt-get -y install ngrep \
    && openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=JP/CN=localhost" \
        -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt \
    # set timezone
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
    # install composer
    && cd '/' \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# laravel installer
RUN composer global require "laravel/installer" \
    && composer global require "laravel/envoy" \
    && composer global require "laravel/ui"

WORKDIR /var/www/html

RUN composer create-project --prefer-dist laravel/laravel react_laravel_tutoreal

WORKDIR /var/www/html/react_laravel_tutoreal

RUN chown -R www-data:www-data /var/www/html/react_laravel_tutoreal