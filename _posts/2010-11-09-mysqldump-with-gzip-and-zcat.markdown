---
layout: post
title:  "mysqldump with gzip and zcat"
date:   2010-11-09 21:47:0
categories: mysql 
---

## To dump a Mysql database and gzip it:

``` mysql
mysqldump -uroot -p my_database | gzip > my_database_dump.sql.gz
```

## To gunzip and install a database:

``` mysql
zcat my_database_dump.sql.gz | mysql -uroot -p my_database
```
