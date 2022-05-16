FROM php:8.0-cli

# Install required system packages
RUN apt-get update && \
    apt-get -y install \
        bash \
        git \
        zlib1g-dev \
        libzip-dev \
        unzip \
        --no-install-recommends && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install php extensions
RUN docker-php-ext-install \
        bcmath \
        zip

# Install pecl extensions
RUN pecl install \
        apcu \
        xdebug-3.0.4 && \
        docker-php-ext-enable \
        apcu.so \
        xdebug

# Configure php
RUN echo "date.timezone = UTC" >> /usr/local/etc/php/php.ini

# Install composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer \
        --install-dir=/usr/local/bin

ENV XDEBUG_MODE=coverage 

COPY . /project

WORKDIR /project

RUN composer install

# ENTRYPOINT ["tail", "-f", "/dev/null"]
# # Runs the command as bash
# CMD ["/bin/bash"]