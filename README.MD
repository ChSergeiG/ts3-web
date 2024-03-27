# Containerized Teamspeak 3 web application.

[![Publish PHP Docker image](https://github.com/ChSergeiG/ts3-web/actions/workflows/docker-publish-php.yml/badge.svg?branch=main)](https://github.com/ChSergeiG/ts3-web/actions/workflows/docker-publish-php.yml)

[![Publish WEB Docker image](https://github.com/ChSergeiG/ts3-web/actions/workflows/docker-publish-web.yml/badge.svg?branch=main)](https://github.com/ChSergeiG/ts3-web/actions/workflows/docker-publish-web.yml)

[![Update Docker Hub Description](https://github.com/ChSergeiG/ts3-web/actions/workflows/docker-description.yml/badge.svg?branch=main)](https://github.com/ChSergeiG/ts3-web/actions/workflows/docker-description.yml)

## Based on [Wruczek GitHub project](https://github.com/Wruczek/ts-website).

### Project in [github](https://github.com/ChSergeiG/ts3-web)

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
   3) To set db password set `services.mysql.environment.MYSQL_ROOT_PASSWORD` variable. This password is sensitive
   4) To persist data on container recreation mount `/var/lib/mysql` directory outside container.
2) PHP container
   1) Default PHP-FBI service port is 9000.
   2) To persist data on container recreation mount `/var/www/html` directory outside container. NB: this directory contains private directory with sensitive data.
3) Nginx proxy container
   1) Nginx service port is 80. This port must be mounted outside container. Default port is 9999.
   2) Nginx's configuration may be inspected in `./fpi.conf` file.
   3) You don't need to mount any files outside container. 

### Installation
1) Go to your_service_ip:9999 address
2) Read the disclaimer and agree with it
3) Select checkboxes as you need. You need to accept license.
4) Press Start button
5) Requirements check must be passed
6) Press Next button
7) Enter url of MySQL db. As noticed it may be your own installation OR container from `docker-compose.yml` file.
   1) hostname: your_db_location OR mysql
   2) username: your_db_username OR username from `services.mysql.environment.MYSQL_USER` variable in `docker-compose.yml`
   3) password: your_db_password OR password from `services.mysql.environment.MYSQL_PASSWORD` variable in `docker-compose.yml`
   4) database name: you need to create new schema or use existing from `services.mysql.environment.MYSQL_DATABASE` variable
      To create new schema, connect to DB and use SQL command `create schema schema_name;`
8) Press Next button
9) Enter your TS3 server address and its ports: query and client
10) Enter query admin username and password. To set query admin password you may need to rerun server with `serveradmin_password=<>` parameter. In compose file you just need to add `command: serveradmin_password=new_password_value` in container context
11) And lastly add user-friendly server alias
12) Press Submit button
13) Press Next button
14) Fill base url, title and TZ
15) Press Submit button
16) Setup is ready to use

### TIPS
- To update DB connection info you need to attach to ts3-php container `docker exec -it php_container_id sh` and edit `vi ./private/dbconfig.php` file;
- To update other setting you may edit values in `ts3.tsw_config` table;
- It seems that usage of external IP in configuration is more stable, than usage of container names;
- To wipe configuration it is enough to remove `php` docker volume and restart container that assigned to it; 

