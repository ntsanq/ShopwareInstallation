# Shopware Installation

## Requirements:

- docker
- docker-compose

## Clone the project:

```
   git clone https://github.com/shopware/development.git
```

Using docker-compose for hosting:

- Create a new .{PROJECT}/docker-compose.yml

```
   cd {PROJECT}
   touch docker-compose.yml
```

- Edit the docker-compose.yml:

```
version: "3"

services:

    shopware:
      # use either tag "latest" or any other version like "6.1.5", ...
      image: dockware/dev:latest
      container_name: shopware
      ports:
         - "80:80"
         - "3306:3306"
         - "22:22"
         - "8888:8888"
         - "9999:9999"
      volumes:
         - "db_volume:/var/lib/mysql"
         - "shop_volume:/var/www/html"
      networks:
         - web
      environment:
         # default = 0, recommended to be OFF for frontend devs
         - XDEBUG_ENABLED=1
         # default = latest PHP, optional = specific version
         - PHP_VERSION=7.4

volumes:
  db_volume:
    driver: local
  shop_volume:
    driver: local

networks:
  web:
    external: false
```

- Run docker-compose:

```
docker-compose up -d 
```

- If you get this error:
  `Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:80 -> 0.0.0.0:0: listen tcp 0.0.0.0:80:
  bind: address already in use`
    - Try to change some ports in docker-compose.yml:
  ````
   ports:
         - "81:80"
  ````   

- If you get this error in Symfony debug UI:
  `Unable to find a matching sales channel for the request: http://example.com/". Please make sure the domain mapping is correct.`
    - Try logging into http://example.com/admin (username: admin , password: shopware)
    - Go to the Storefront in `Sales channel` >> Find `Domains`. There you can configure the domain.

## Success Installation

-- SHOP URL: http://localhost

-- ADMIN URL: http://localhost/admin

-- ADMINER URL: http://localhost/adminer.php

-- MAILCATCHER URL: http://localhost/mailcatcher

## Author
[@ntsanq - Nguyen Thanh Sang](https://github.com/ntsanq)
