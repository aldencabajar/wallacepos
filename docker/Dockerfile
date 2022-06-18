FROM php:7.4-apache

# WORKDIR /app
# COPY . .
RUN useradd -ms /bin/bash admin
COPY --chown=admin:admin . /app
WORKDIR /app

RUN apt update && \
    apt install -y zlib1g-dev libpng-dev libcurl4-gnutls-dev  && rm -rf /var/lib/apt/lists/* &&\
    docker-php-ext-install pdo pdo_mysql gd curl && \
    docker-php-ext-enable pdo_mysql gd curl
RUN a2enmod proxy_http proxy_wstunnel rewrite

RUN cp ./virtual-host.conf /etc/apache2/sites-available/wpos.conf && \
    a2ensite wpos.conf && \
    a2dissite 000-default.conf && \
    service apache2 restart

# install node js and socket io libraries
RUN apt-get update && \
    apt-get install -y nodejs && apt-get install -y npm && \
    cd /app/api && \
    npm install socket.io

# install composer and php pacakges in composer lock
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    composer install

USER admin
    
    



    