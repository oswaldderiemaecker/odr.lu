---
layout: post
title:  "Behat: Multiple Context files with Sub-Context"
date:   2012-01-15 21:47:09
categories: php
---

Rather than having all your Step Definitions in the FeatureContext.php file and to keep your Contexts organized you would like to have three differents Context files, in your bootstrap folder you have:

FeatureContext.php
MyFirstContext.php
MySecondContext.php
MyThirdContext.php

Behat only reads the FeatureContext.php file, you need to source these three Context files, for this you can use sub-context in your FeatureContext.php file as this:

{% highlight php %}
<?php

public function __construct(array $parameters) {
    $this->parameters = $parameters;

    // Load Context Class
    $this->useContext('my_label_first_context', new MyFirstContext());
    $this->useContext('my_label_second_context', new MySecondContext());
    $this->useContext('my_label_third_context', new MyThirdContext());
}
{% endhighlight %}

Your Context file Classes must extend from BehatContext like:

{% highlight php %}
<?php

use Behat\Behat\Context\BehatContext;

/**
 * Features context.
 */
class MyFirstContext extends BehatContext
{
}
{% endhighlight %}
