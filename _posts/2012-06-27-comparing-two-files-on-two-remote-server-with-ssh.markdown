---
layout: post
title:  "Comparing two files on two remote Servers using SSH"
date:   2012-06-27 21:47:09
categories: linux
---
To find out the difference between two files from two remote servers using SSH, e.g use:

{% highlight bash %}
diff <(ssh -A root@server_ip_1 'cat /etc/php5/cli/php.ini') \ 
<(ssh -A root@server_ip_2 'cat /etc/php5/cli/php.ini')
{% endhighlight %}

Note, you need to add your keys using ssh-add
