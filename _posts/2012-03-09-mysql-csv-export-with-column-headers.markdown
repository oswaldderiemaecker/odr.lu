---
layout: post
title:  "MySQL CSV export with column headers"
date:   2012-03-09 21:47:09
categories: mysql
---
Use UNION to add column headers to the CSV export like:

{% highlight mysql %}
SELECT 'Header1_ID','Header2_Lastname','Header3_Firstname' 
UNION 
SELECT id, lastname, firstname 
FROM persons 
INTO OUTFILE "/tmp/persons.csv" FIELDS TERMINATED BY ',' 
OPTIONALLY ENCLOSED BY '"' LINES   TERMINATED BY '\n';
{% endhighlight %}
