# ─────────────────────────────────────────────
# Stage 1 : dependencies (PHP 8.3 = platform du composer.lock)
# ─────────────────────────────────────────────
FROM php:8.3-cli-alpine AS vendor

RUN apk add --no-cache git unzip \
    && docker-php-ext-install zip

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER=1

WORKDIR /app

COPY composer.json composer.lock ./
RUN composer install \
        --no-interaction \
        --no-dev \
        --prefer-dist \
        --optimize-autoloader \
        --no-scripts

# ─────────────────────────────────────────────
# Stage 2 : production image
# ─────────────────────────────────────────────
FROM php:8.3-fpm-alpine

# Extensions PHP nécessaires
RUN apk add --no-cache \
        libpng-dev \
        libjpeg-turbo-dev \
        libwebp-dev \
        freetype-dev \
        oniguruma-dev \
        libxml2-dev \
        zip \
        unzip \
        curl \
        nginx \
        supervisor \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install \
        pdo \
        pdo_mysql \
        mbstring \
        exif \
        pcntl \
        bcmath \
        gd \
        xml \
        opcache

WORKDIR /var/www/html

# Copier les dépendances depuis le stage vendor
COPY --from=vendor /app/vendor ./vendor

# Copier le code source
COPY . .

# Créer les dossiers requis par Laravel et fixer les permissions
RUN mkdir -p bootstrap/cache \
        storage/framework/sessions \
        storage/framework/views \
        storage/framework/cache \
        storage/logs \
    && chown -R www-data:www-data /var/www/html \
    && chmod -R 775 storage bootstrap/cache

# Config PHP optimisée pour la prod
COPY docker/php/php.ini /usr/local/etc/php/conf.d/app.ini
COPY docker/php/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# Config Nginx
COPY docker/nginx/default.conf /etc/nginx/http.d/default.conf

# Config Supervisor (nginx + php-fpm)
COPY docker/supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
