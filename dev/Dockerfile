FROM webdevops/php-apache-dev:7.4

RUN apt-get update && apt-get upgrade -y && apt-get install -y sudo systemd w3m mariadb-client cron
RUN systemctl enable apache2

# install nodejs
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash
RUN apt-get update && apt-get install -y nodejs
RUN npm install -g npm@latest

# cypress
RUN apt-get install -y libgtk2.0-0 libgtk-3-0 libgbm-dev libnotify-dev libgconf-2-4 libnss3 libxss1 libasound2 libxtst6 xauth xvfb
RUN npm install cypress -g



COPY ./supervisor.conf /opt/docker/etc/supervisor.d/simple-home.conf
COPY ./cron.conf /etc/cron.d/laravel.conf

WORKDIR /app




RUN echo 'application ALL=(ALL) NOPASSWD:/usr/local/bin/service apache2 status, /usr/local/bin/service mysql status' >> /etc/sudoers

USER application
RUN cypress install

USER root

WORKDIR /app
