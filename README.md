# Init

```
git clone https://github.com/tiwuofficial/laradock-sample.git
cd laradock-sample
git submodule add https://github.com/Laradock/laradock.git
cd laradock
cp env-example .env
APPLICATION=../testapp/
docker-compose up -d workspace
docker exec -it laradock_workspace_1 /bin/bash
cd /var/www/testapp
composer install
cp .env.example .env
php artisan key:generate
exit
docker-compose stop
vi nginx/sites/default.conf
vi .env
vi nginx/Dockerfile
docker-compose up -d php-fpm nginx mysql
```

http://localhost:8888/

## nginx/sites/default.conf

```
# before
root /var/www/public;
# after
root /var/www/testapp/public;
```

## .env

macのapacheとmysqlとportが被るので変更する

```
# before
NGINX_HOST_HTTP_PORT=80
MYSQL_PORT=3306
# after
NGINX_HOST_HTTP_PORT=8888
MYSQL_PORT=3307
```

## nginx/Dockerfile

下記PRがmergeされれば、Dockerfileの書き換えは不要

https://github.com/laradock/laradock/pull/1288/files

```
# before
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories
# after
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories \
```