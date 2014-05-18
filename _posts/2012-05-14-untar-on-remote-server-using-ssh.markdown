---
layout: post
title:  "Untar on remote server through SSH"
date:   2012-05-14 21:47:09
categories: linux
---
If you need to untar a large file on a server where there is not enough space to upload the tar file and untar it, this untar through SSH command can come really handy:

{% highlight bash %}
cat your_tar_file.tar | ssh user@domain " cd /path/to/untar; tar xvf -"
{% endhighlight %}

