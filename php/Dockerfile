FROM php:8.2-fpm

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
RUN composer install --no-dev --optimize-autoloader
RUN chown -R www-data:www-data /var/www

# 必要ポートの公開
EXPOSE 80

CMD ["php-fpm"]
