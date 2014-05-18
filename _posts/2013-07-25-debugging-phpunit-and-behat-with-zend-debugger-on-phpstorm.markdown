---
layout: post
title:  "Debugging PHPUnit and Behat with Zend Debugger on PHPStorm"
date:   2013-07-25 21:47:09
categories: php
---

Debugging PHPUnit with ZendDebugger (PHPStorm):

{% highlight bash %}
export QUERY_STRING="start_debug=1&debug_stop=1&debug_fastfile=1&
use_remote=1&send_sess_end=1&debug_session_id=2000&debug_start_session=1&
debug_port=10137&debug_host=<PHPStorm_IP_address>"
export PHP_IDE_CONFIG="serverName=<hostname>" 
{% endhighlight %}

In your phpunit boostrap.php add:

Zend_Session::$_unitTestEnabled = true;
