FROM php:8.4-fpm

# 必要な拡張モジュールをインストール
RUN apt-get update && apt-get install -y \
    libpq-dev libonig-dev libzip-dev unzip \
    && docker-php-ext-install pdo pdo_mysql mbstring zip intl

# Composer のインストール
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Symfony アプリケーションのソースをコピー
WORKDIR /var/www
COPY . /var/www

# 権限の設定
RUN chown -R www-data:www-data /var/www

# 必要ポートの公開
EXPOSE 9000
