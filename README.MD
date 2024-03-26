# Containerized Teamspeak 3 web application.

[![Publish Docker image](https://github.com/ChSergeiG/ts3-web/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/ChSergeiG/ts3-web/actions/workflows/docker-publish.yml)

Based on [Wruczek GitHub project](https://github.com/Wruczek/ts-website).

### Launch

To start stack you may use docker-compose.yml file using command
```shell
docker-compose -f ./docker-compose.yml up -d
#or
docker compose -f ./docker-compose.yml up -d
```

### Content
Stack consists of 3 containers:
* mysql - database to store web server data
* php - php-fpi to execute php script
* web - nginx reverse proxy, that pass requests to php container

### Setup
1) MySQL container
   1) It is optional; you may use any mysql installation.
   2) Default MySQL port if 3306.
   3) To set db password set `services.mysql.environment.MYSQL_ROOT_PASSWORD` variable.
   4) To persist data on container recreation mount `/var/lib/mysql` directory outside container.
2) PHP container
   1) Default PHP-FBI service port is 9000.
   2) To persist data on container recreation mount `/var/www/html` directory outside container. NB: this directory contains private directory with sensitive data.
3) Nginx proxy container
   1) Nginx service port is 80. This port must ve mount outside container. Default port is 9999.
   2) Nginx configuration may be inspected in `./fpi.conf` file.
   3) You dont need to mount any files outside container. 

