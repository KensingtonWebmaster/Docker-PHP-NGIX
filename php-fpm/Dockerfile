FROM php:7.4-fpm

COPY ./php.ini /usr/local/etc/php/

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils \
    libonig-dev \
    default-mysql-client \
    vim \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libzip-dev \
    zip \
    libldap2-dev \
    && docker-php-ext-install -j$(nproc) bcmath mbstring iconv zip pdo pdo_mysql \
    && docker-php-ext-configure zip \
    && docker-php-ext-configure gd \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install ldap \
    && rm -rf /var/lib/apt/lists/*
    
# Install PECL and ssh2
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    locales \
    git \
    ssh \
    unzip \
    libmemcached-dev \
    zlib1g-dev \
    libicu-dev \
    libpq-dev \
    libssh2-1-dev \
    libxml2-dev \
    libwebp-dev \
    libmagickwand-dev \
    && rm -rf /var/lib/apt/lists/* \
    && dpkg-reconfigure -f noninteractive locales \
    && echo 'en_US.UTF-8 UTF-8' > /etc/locale.gen \
    && echo 'fr_FR.UTF-8 UTF-8' >> /etc/locale.gen \
    && locale-gen

RUN docker-php-ext-configure gd \
    && docker-php-ext-install -j$(nproc) intl gd opcache mbstring zip pdo_pgsql pdo_mysql soap pgsql mysqli exif

# PECL modules
# RUN pecl install imagick && docker-php-ext-enable imagick \
#     && pecl install apcu && docker-php-ext-enable apcu \
#     && pecl install apcu_bc-beta && docker-php-ext-enable --ini-name=docker-php-ext-apcu_bc.ini apc \
#     && pecl install ssh2-1.0 && docker-php-ext-enable ssh2


RUN { \
    echo 'opcache.memory_consumption=128'; \
    echo 'opcache.interned_strings_buffer=8'; \
    echo 'opcache.max_accelerated_files=4000'; \
    echo 'opcache.revalidate_freq=60'; \
    echo 'opcache.fast_shutdown=1'; \
    echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && ln -s $(composer config --global home) /root/composer
ENV PATH=$PATH:/root/composer/vendor/bin COMPOSER_ALLOW_SUPERUSER=1

# Install Node.js
# Whatever you did in your PHP Dockerfile
# this was missing for some reason. Didn't require last time.
# RUN apt-get install -y gnupg2
# Removing /var/lib/apt/lists worked
RUN rm -rf /var/lib/apt/lists/ && curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install nodejs -y

# RUN usermod -u 1000 www-data
# # RUN usermod -u 1002 www-data
# RUN chown -R www-data:www-data /var/www
# RUN chmod -R 755 /var/www