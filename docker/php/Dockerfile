ARG PHP_BASE_IMAGE_VERSION
FROM php:${PHP_BASE_IMAGE_VERSION}

# Install modules
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpq-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_pgsql mbstring exif pcntl bcmath gd

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupmod -o -g ${PGID} www-data && \
  usermod -o -u ${PUID} -g www-data www-data

RUN chown -R www-data /var/www

COPY ./php.ini /usr/local/etc/php/
COPY ./php-fpm.conf /usr/local/etc/php-fpm.conf
COPY ./docker-php-entrypoint /usr/local/bin/

RUN curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir=/usr/bin --filename=composer

WORKDIR /app

ENTRYPOINT ["docker-php-entrypoint"]
CMD ["php-fpm"]
