---
title: Drupal Docker Notes
layout: post
---

## Start database
```
docker run --name d8i-mysql -e MYSQL_ROOT_PASSWORD=drupal -d mysql
```

## Start drupal
```
docker run --name d8i --link d8i-mysql:mysql -p 8080:80 -d drupal:8.4.0-rc1-apache
```

## Run a bash prompt
```
docker exec -it d8i bash
```
