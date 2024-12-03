FROM php:8.2-fpm

# Instalar dependencias del sistema necesarias

RUN apt-get update
RUN apt-get install -y \
    git \
    unzip \
    libicu-dev \
    libpq-dev \
    wget \
    libzip-dev \
    && docker-php-ext-install \
    intl \
    pdo \
    pdo_mysql \
    zip \
    && apt-get clean

WORKDIR /var/www/html

# Instalar Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Instalar Symfony CLI
RUN wget https://get.symfony.com/cli/installer -O - | bash && \
    mv /root/.symfony*/bin/symfony /usr/local/bin/symfony

# Copiar composer.json y composer.lock
COPY composer.json composer.lock ./

# Copiar el resto de los archivos del proyecto
COPY . .

# Establecer permisos para los directorios de caché y logs
RUN chmod -R 777 var/cache var/log
RUN mkdir -p /var/www/.symfony5 && \
    chown -R www-data:www-data /var/www

# Cambiar el UID y GID del usuario www-data a 1000 (para alinear con el host)
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

# Establecer usuario www-data para ejecutar el contenedor
USER www-data

# Instalar dependencias PHP
RUN composer install --no-scripts --no-interaction

# Exponer el puerto 80
EXPOSE 8000

CMD ["symfony", "serve", "--allow-all-ip", "--no-tls"]