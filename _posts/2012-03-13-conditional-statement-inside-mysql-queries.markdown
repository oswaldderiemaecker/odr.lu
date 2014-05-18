---
layout: post
title:  "Conditional statements inside MySQL queries example"
date:   2012-03-13 21:47:09
categories: mysql
---
List id_person with value count < 50 OR id_person for which the  count(50<my_value<500)<=2 from the below Table1:

|id_person |  my_date | my_value |
|1 |  1990-10-11 | 550 | 
|2 |  1991-11-01 | 64  |
|3 |  2011-03-16 | 20  |
|4 |  2011-02-02 | 20  |
|5 |  2011-02-02 | 49  |
|1 |  1998-02-21 | 450 |
|3 |  1991-11-01 | 51  |
|3 |  0000-00-00 | 34  |
|3 |  2005-03-04 | 160 |
|3 |  2006-05-12 | 250 |
|2 |  2007-08-09 | 75  |

{% highlight mysql %}
SELECT id_person, SUM(IF(my_value<50,1,0)) as Sum_below_50,
        SUM(IF(my_value>50 AND 
        my_value<500,1,0)) as Sum_below_500, 
        SUM(IF(my_value<50,1,0))+SUM(IF(my_value>50 AND
        my_value<500,1,0)) as Total 
FROM Table1 
GROUP BY id_person HAVING Sum_below_500<=2;
{% endhighlight %}

| id_person |  Sum_below_50 |   Sum_below_500 |  Total|
|1 |  0 |  1 |  1|
|2 |  0 |  2 |  2|
|4 |  1 |  0 |  1|
|5 |  1 |  0 |  1|

Our id_person 3 is not listed because it has my_value above 50 and below 500 more than 2 times.
