FROM webdevops/php-apache:7.4
ENV WEB_DOCUMENT_ROOT=/app/public

RUN apt-get update && apt-get upgrade -y && apt-get install -y sudo systemd w3m mariadb-client cron
RUN systemctl enable apache2

# install nodejs
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash
RUN apt-get update && apt-get install -y nodejs
RUN npm install -g npm@latest

COPY ./prod/supervisor.conf /opt/docker/etc/supervisor.d/simple-home.conf
COPY ./prod/cron.conf /etc/cron.d/laravel.conf

USER application
RUN git clone https://github.com/Simple-Home/Simple-Home.git /app
WORKDIR /app
RUN git checkout master

RUN composer install
RUN npm install

USER root

RUN chown application:application /app -R
RUN chmod 775 /app
RUN chmod 775 /app/storage/framework
RUN chmod 775 /app/storage/logs
RUN chmod 775 /app/bootstrap/cache
RUN npm run production



RUN echo 'application ALL=(ALL) NOPASSWD:/usr/local/bin/service apache2 status, /usr/local/bin/service mysql status' >> /etc/sudoers
WORKDIR /app