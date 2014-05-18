---
layout: post
title:  "Mysql copying one table structure to another"
date:   2013-01-20 21:47:09
categories: mysql
---

Using the LIKE keyword with mysql CREATE TABLE you can easily copy the structure of a table.

{% highlight mysql %}
CREATE TABLE new_table LIKE my_table;
{% endhighlight %}

This will create a table the "new_table" which will have exactly the same structure of "my_table".
