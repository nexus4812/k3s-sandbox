FROM composer:2 AS composer-build
WORKDIR /var/www

# composer.json と composer.lock をコピーして、先行して install
COPY ./web/composer.json ./web/composer.lock /var/www/
RUN composer install --no-dev --no-scripts --no-cache --working-dir=/var/www/ --prefer-dist

FROM php:8.2-fpm-alpine
RUN apk add --no-cache --virtual .build-deps \
    oniguruma-dev \
    && docker-php-ext-install pdo pdo_mysql mbstring  && \
    apk del --purge .build-deps

# FMPのConfigをコピー
COPY ./containers/php/www.conf /usr/local/etc/php-fpm.d/www.conf

# パッケージのインストール
# 先行してvendor配下のダウンロードを行うことで、vendorのキャッシュとしている
WORKDIR /var/www
COPY --chown=www-data:www-data ./web/.env.dev /var/www/.env
COPY --chown=www-data:www-data --from=composer-build /var/www /var/www
# COPY --from=composer:2 /usr/bin/composer /usr/bin/composer # for debug

CMD ["php-fpm"]

# Build sample
# docker build -t php-fpm -f containers/php/Dockerfile .

# Run sample
# docker run -it --rm php-fpm ash
