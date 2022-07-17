FROM composer AS composer

WORKDIR /app
COPY ./ /app

RUN composer install \
  --optimize-autoloader \
  --no-interaction \
  --no-progress

FROM node:16 AS node

WORKDIR /app
COPY ./ /app

RUN npm install
RUN npm run build

FROM php:8.1-apache-buster

ENV PORT 80
EXPOSE 80

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

COPY --chown=www-data --from=composer /app /var/www/html
COPY --chown=www-data --from=node /app /var/www/html