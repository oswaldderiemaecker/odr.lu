---
layout: post
title:  "MySQL Group by Month, Year, Day"
date:   2012-01-13 21:47:09
categories: mysql
---
Group Stats by Month and YEAR


{% highlight mysql %}
SELECT COUNT(id) 
FROM stats 
WHERE YEAR(record_date) = 2011 
GROUP BY YEAR(record_date), MONTH(record_date)
{% endhighlight %}

If you want to group by date:

{% highlight mysql %}
SELECT COUNT(id)
 FROM stats
  GROUP BY DAYOFMONTH(record_date)
{% endhighlight %}

Per day of the month showing the Year and Month:

{% highlight mysql %}
SELECT YEAR(record_date), MONTH(record_date), DAYOFMONTH(record_date), COUNT(id)
FROM stats 
WHERE YEAR(record_date) = 2011 
GROUP BY YEAR(record_date), MONTH(record_date), DAYOFMONTH(record_date)
{% endhighlight %}
