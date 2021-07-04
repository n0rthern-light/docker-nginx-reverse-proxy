FROM php:8.0-fpm

COPY composer.lock composer.json /var/www/

WORKDIR /var/www

RUN apt-get update -y --fix-missing && apt-get upgrade -y;

RUN apt-get install -y nodejs npm openssl unzip git zlib1g-dev libpng-dev libicu-dev
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN docker-php-ext-install pdo gd pdo_mysql pcntl intl exif

RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libjpeg-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

RUN pecl install redis && docker-php-ext-enable redis
RUN pecl install igbinary && docker-php-ext-enable igbinary
RUN pecl install swoole && docker-php-ext-enable swoole

RUN apt-get install -y \
        libzip-dev \
        zip \
  && docker-php-ext-install zip



# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Add user for laravel
#RUN groupadd -g 1000 www
#RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy application folder
COPY . /var/www

# Copy existing permissions from folder to docker
#RUN chmod -R 770 .
RUN chown -R www-data:www-data /var/www

RUN chmod -R 777 /var/www/


RUN composer update
RUN composer install
RUN npm install npm@latest -g
RUN npm cache clean -f
RUN npm install n@latest -g
RUN n latest
RUN npm install -g yarn
RUN yarn

#webserver expose
EXPOSE 9000
CMD ["php-fpm"]