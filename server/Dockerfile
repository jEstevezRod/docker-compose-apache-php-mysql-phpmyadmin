FROM php:7.3.4-apache

RUN apt-get update \
&& apt-get install -y apt-utils vim curl sqlite3 \
&& pecl install xdebug

ADD ./php.ini /usr/local/etc/php/

ADD ./sites /etc/apache2/sites-available

RUN a2ensite httpd.conf

EXPOSE 80
