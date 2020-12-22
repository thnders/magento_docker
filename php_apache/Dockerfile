FROM php:7.2.1-apache
RUN apt-get update
RUN apt-get install -y \
    git \
    nano \
    libpng-dev \
    libmcrypt-dev \ 
    libpq-dev \  
    zlib1g-dev \
    zip \
    unzip &&\
    a2enmod rewrite

RUN docker-php-ext-install pdo &&\
    docker-php-ext-install pdo_mysql &&\
    docker-php-ext-install pdo_pgsql &&\
    docker-php-ext-install zip &&\
    docker-php-ext-install gd &&\
    docker-php-ext-install pcntl 
RUN docker-php-ext-install pdo pdo_mysql mbstring 

ADD config/phpconf.ini /etc/php/7.2/apache2/php.ini

COPY /config/apache.conf /etc/apache2/sites-available/apache.conf

RUN a2ensite apache

ARG certInfo=

RUN mkdir /etc/apache2/ssl
RUN echo "US\nOklahoma\nEdmond\nLazslo\nYOURORG\napp.localhost\nyour@email.com\n" | openssl req -x509 -nodes -days 999 -newkey rsa:2048 -keyout /etc/apache2/ssl/ssl.key -out /etc/apache2/ssl/certificate.crt
RUN echo "US\nOklahoma\nEdmond\nLazslo\nYOURORG\napp.localhost\nyour@email.com\n\n" | openssl req -new -key /etc/apache2/ssl/ssl.key -out /etc/apache2/ssl/signing_request.csr

RUN a2enmod proxy_http
RUN a2enmod proxy
RUN a2enmod ssl
RUN a2enmod rewrite
RUN a2dissite 000-default.conf

CMD ["apache2ctl", "-D", "FOREGROUND"]