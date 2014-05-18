---
layout: post
title:  "Getting MYSQL table rows count while importing table"
date:   2013-10-04 21:47:09
categories: mysql
---

When importing a large table, the table is locked and it is not possible to use a "SELECT COUNT(*) FROM Table;", to follow the import progress use:

{% highlight mysql %}
use information_schema;
SELECT TABLE_NAME, TABLE_ROWS FROM `information_schema`.`tables` WHERE 
`table_schema` = 'YOUR_DB_NAME';
{% endhighlight %}

To find out when the last insert happened use:
{% highlight mysql %}
use information_schema;
SELECT table_schema,table_name,max_time FROM information_schema.tables t1 JOIN 
(select MAX(t2.create_time) AS max_time FROM information_schema.tables t2 WHERE 
table_schema ='YOUR_DB_NAME') AS t3   ON t1.create_time = t3.max_time;
{% endhighlight %}

