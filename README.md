# Dolibarr on Docker

Docker image for Dolibarr with auto installer on first boot.

## Supported tags

* 5.0.7 5.0.7-php7.0 5.0.7-php7.1 
* 6.0.8 6.0.8-php7.0 6.0.8-php7.1 
* 7.0.5 7.0.5-php7.0 7.0.5-php7.1 
* 8.0.3 8.0.3-php7.0 8.0.3-php7.1 
* 8.0.4 8.0.4-php7.0 8.0.4-php7.1 
* 9.0.0 9.0.0-php7.0 9.0.0-php7.1 
* 9.0.1 9.0.1-php7.0 9.0.1-php7.1 
* 9.0.3 9.0.3-php7.0 9.0.3-php7.1 
* 10.0.1 10.0.1-php7.0 10.0.1-php7.1 


## What is Dolibarr ?

Dolibarr ERP & CRM is a modern software package to manage your organization's activity (contacts, suppliers, invoices, orders, stocks, agenda, ...).

> [More informations](https://github.com/dolibarr/dolibarr)

## How to run this image ?

This image is based on the [officiel PHP repository](https://registry.hub.docker.com/_/php/).

**Important**: This image don't contains database. So you need to link it with a database container.

Let's use [Docker Compose](https://docs.docker.com/compose/) to integrate it with [MariaDB](https://hub.docker.com/_/mariadb/) (you can also use [MySQL](https://hub.docker.com/_/mysql/) if you prefer).

Create `docker-compose.yml` file as following:

```
mariadb:
    image: mariadb:latest
    environment:
        MYSQL_ROOT_PASSWORD: root
        MYSQL_DATABASE: dolibarr

web:
    image: tuxgasy/dolibarr
    environment:
        DOLI_DB_HOST: mariadb
        DOLI_DB_USER: root
        DOLI_DB_PASSWORD: root
        DOLI_DB_NAME: dolibarr
        DOLI_URL_ROOT: 'http://0.0.0.0'
        PHP_INI_DATE_TIMEZONE: 'Europe/Paris'
    ports:
        - "80:80"
    links:
        - mariadb
```

Then run all services `docker-compose up -d`. Now, go to http://0.0.0.0 to access to the new Dolibarr installation.

## Environment variables summary

| Variable                      | Default value      | Description |
| ----------------------------- | ------------------ | ----------- |
| **DOLI_INSTALL_AUTO**         | *1*                | 1: The installation will be executed on first boot
| **DOLI_DB_HOST**              | *mysql*            | Host name of the MariaDB/MySQL server
| **DOLI_DB_USER**              | *doli*             | Database user
| **DOLI_DB_PASSWORD**          | *doli_pass*        | Database user's password
| **DOLI_DB_NAME**              | *dolidb*           | Database name
| **DOLI_ADMIN_LOGIN**          | *admin*            | Admin's login create on the first boot
| **DOLI_ADMIN_PASSWORD**       | *admin*            | Admin'password
| **DOLI_URL_ROOT**             | *http://localhost* | Url root of the Dolibarr installation
| **PHP_INI_DATE_TIMEZONE**     | *UTC*              | Default timezone on PHP
| **WWW_USER_ID**               |                    | ID of user www-data. ID will not changed if leave empty. During a development, it is very practical to put the same ID as the host user.
| **WWW_GROUP_ID**              |                    | ID of group www-data. ID will not changed if leave empty.
