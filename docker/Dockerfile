FROM amazonlinux:2023

RUN dnf update \
    && dnf install -y nginx php8.2-fpm php8.2 php8.2-pdo php8.2-mysqlnd git

RUN dnf install -y python3-pip \
    && pip3 install supervisor

COPY ./supervisord.conf /etc/supervisor/conf.d/
COPY ./nginx.conf /etc/nginx/conf.d/
RUN mkdir /run/php-fpm


RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

WORKDIR /var/www/html

CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]