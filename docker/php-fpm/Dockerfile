FROM php:8.3.0-fpm
WORKDIR "/application"

ARG DEBIAN_FRONTEND=nointeractive

ENV TZ=Europe/Warsaw
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
    libzip-dev zip libicu-dev

COPY --from=composer:2.6.6 /usr/bin/composer /usr/local/bin/composer
COPY ./docker/php-fpm/php-ini-overrides.ini $PHP_INI_DIR/php.ini

ENV PHP_FPM_DOCKER_CONF=$PHP_INI_DIR-fpm.d/zz-docker.conf
RUN echo 'pm.max_children = 50' >> $PHP_FPM_DOCKER_CONF \
        && echo 'pm.max_request = 500' >> $PHP_FPM_DOCKER_CONF \
        && echo 'pm = static' >> $PHP_FPM_DOCKER_CONF

# Instalacja dodatkowych narzędzi >
RUN docker-php-ext-install pdo_mysql pcntl intl zip \
    && docker-php-ext-enable pcntl intl opcache
