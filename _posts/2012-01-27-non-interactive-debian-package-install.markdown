---
layout: post
title:  "Non Interactive Debian Package Install"
date:   2012-01-27 21:47:09
categories: linux
---

To install Debian Package as non interactive use:

{% highlight bash %}
export DEBIAN_FRONTEND=noninteractive
aptitude install -y mysql-client mysql-server php5-mysql
export DEBIAN_FRONTEND=dialog
{% endhighlight %}
