FROM php:7.4-fpm

COPY composer.lock composer.json /var/www/

WORKDIR /var/www


RUN apt-get update -y && apt-get install -y nodejs npm openssl zip unzip git zlib1g-dev libpng-dev libicu-dev
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN docker-php-ext-install pdo pdo_mysql gd pcntl intl mysqli
RUN pecl install redis && docker-php-ext-enable redis


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


RUN composer install
RUN npm install npm@latest -g
RUN npm install -g yarn
RUN yarn


# change current user to www
#USER www

#webserver expose
EXPOSE 9000
CMD ["php-fpm"]
