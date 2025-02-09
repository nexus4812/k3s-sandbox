FROM php:8.2-fpm

# 必要な拡張モジュールをインストール
# 必要なPHP拡張をインストール
RUN apt-get update && apt-get install -y \
    unzip \
    git \
    libicu-dev \
    libpq-dev \
    libonig-dev \
    libzip-dev \
    && apt-get install -y \
    icu-devtools \
    && docker-php-ext-install pdo pdo_mysql mbstring zip intl

# Composer のインストール
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# FMPのConfigをコピー
COPY ./containers/php/www.conf /usr/local/etc/php-fpm.d/www.conf

# パッケージのインストール
# 先行してvendor配下のダウンロードを行うことで、vendorのキャッシュとしている
WORKDIR /var/www
COPY ./web/composer.json /var/www
COPY ./web/composer.lock /var/www
RUN composer install --no-dev --no-scripts

# ソースコード自体をコピーしてdump-autoload等の実行
COPY ./web /var/www
COPY ./web/.env.dev /var/www/.env
RUN composer dump-autoload && composer auto-scripts

RUN chown -R www-data:www-data /var/www
USER www-data

CMD ["php-fpm"]

# sample
# docker build -t php-fpm -f containers/php/Dockerfile .
