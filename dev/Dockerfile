FROM webdevops/php-apache-dev:7.4
ENV WEB_DOCUMENT_ROOT=/app/public

RUN apt-get update && apt-get upgrade -y && apt-get install -y sudo systemd w3m mariadb-client cron
RUN systemctl enable apache2

# install nodejs
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash
RUN apt-get update && apt-get install -y nodejs
RUN npm install -g npm@latest

# cypress
RUN apt-get install -y libgtk2.0-0 libgtk-3-0 libgbm-dev libnotify-dev libgconf-2-4 libnss3 libxss1 libasound2 libxtst6 xauth xvfb
RUN npm install cypress@8.7.0 -g


RUN echo "* * * * * application cd /app && php artisan schedule:run >> /dev/null 2>&1" > /etc/cron.d/laravel.conf

RUN echo "[program:laravel-worker] \
          command=php /app/artisan queue:work --sleep=3 --tries=3 --max-time=3600 \
          autostart=true \
          autorestart=true \
          stopasgroup=true \
          killasgroup=true \
          user=application \
          numprocs=1 \
          redirect_stderr=true \
          stdout_logfile=/app/storage/logs/laravel-worker.log \
          stopwaitsecs=3600 \
          \
          [program:cron] \
          command=/usr/sbin/cron -f -l 8 \
          autostart=true \
          stdout_logfile=/var/log/cron.out.log \
          stderr_logfile=/var/log/cron.err.log \
" > /opt/docker/etc/supervisor.d/simple-home.conf >


WORKDIR /app




RUN echo 'application ALL=(ALL) NOPASSWD:/usr/local/bin/service apache2 status, /usr/local/bin/service mysql status' >> /etc/sudoers

USER application
RUN cypress install

USER root

WORKDIR /app
