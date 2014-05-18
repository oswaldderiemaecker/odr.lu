---
layout: post
title:  "PHP development with behat BDD and phpunit TDD"
date:   2012-05-03 21:47:09
categories: php
---
This post is explaining the usage of BDD with behat and TDD with PHPUnit for the development of a small Math class, the point is to show that using both BDD and TDD ensure:

1. The Acceptance Testing which will tell the developer that the code is doing the right things.
2. The Class code is build using Unit Test, which will tell the developer that the code is doing the things right.

Your Product Owner has the following User Story:

>"As a mathematician I want to have a basic Calculator to add two numbers so that I can get the result"

He has also provided a list of value to addition and their expected results.

| **Numerber 1 | **number 2 | ** number 3 |
|---|---|---|
| 10  | 12  | 22  |
| 5  | 3  | 8  |
| 5  | 5  | 10  |

So let's edit our behat feature math.feature file:

{% highlight bash %}
# features/math.feature
Feature: As a mathematician I want to have a basic Calculator to add two numbers so 
         that I can get the result. 
              
Background:
    Given I have basic calculator
 
Scenario Outline:
    Given I have entered <number1>
    And I have entered <number2>
    When I add
    Then The result should be <result>
  
   Examples:
      | number1 | number2 | result |
      | 10      | 12      | 22     |
      | 5       | 3       | 8      |
      | 5       | 5       | 10     |
{% endhighlight %}

Let's run our feature context file with:

{% highlight bash %}
$ behat math.feature 
# features/bootstrap/FeatureContext.php
Feature: As a mathematician I want to have a basic Calculator to add two numbers so 
         that I can get the result.
 
Background:
    Given I have basic calculator
 
Scenario Outline:
    Given I have entered <number1>
    And I have entered <number2>
    When I add
    Then The result should be <result>
 
Examples:
      | number1 | number2 | result |
      | 10      | 12      | 22     |
      | 5       | 3       | 8      |
      | 5       | 5       | 10     |
 
3 scenarios (3 undefined)
15 steps (15 undefined)
0m0.13s
 
You can implement step definitions for undefined steps with these snippets:
    /**
     * @Given /^I have basic calculator$/
     */
    public function iHaveBasicCalculator()
    {
        throw new PendingException();
    }
    /**
     * @Given /^I have entered (\d+)$/
     */
    public function iHaveEntered($argument1)
    {
        throw new PendingException();
    }
    /**
     * @When /^I add$/
     */
    public function iAdd()
    {
        throw new PendingException();
    }
    /**
     * @Then /^The result should be (\d+)$/
     */
    public function theResultShouldBe($argument1)
    {
        throw new PendingException();
    }
{% endhighlight %}

Let's implement the step definition by copying them in our bootstrap/FeatureContext.php file:

{% highlight php %}
vi bootstrap/FeatureContext.php
# features/bootstrap/FeatureContext.php
 
<?php
use Behat\Behat\Context\ClosuredContextInterface,
    Behat\Behat\Context\BehatContext,
    Behat\Behat\Exception\PendingException;
 
use Behat\Gherkin\Node\PyStringNode,
    Behat\Gherkin\Node\TableNode;
 
require_once 'mink/autoload.php';
// or, if you want to use phar from current dir:
// require_once __DIR__ . '/mink.phar';
 
class FeatureContext extends Behat\Mink\Behat\Context\MinkContext
{
    /**
     * @Given /^I have basic calculator$/
     */
    public function iHaveBasicCalculator()
    {
        throw new PendingException();
    }
 
    /**
     * @Given /^I have entered (\d+)$/
     */
    public function iHaveEntered($argument1)
    {
        throw new PendingException();
    }
 
    /**
     * @When /^I add$/
     */
    public function iAdd()
    {
        throw new PendingException();
    }
 
    /**
     * @Then /^The result should be (\d+)$/
     */
    public function theResultShouldBe($argument1)
    {
        throw new PendingException();
    }
}
{% endhighlight %}

Let's run again our behat:

{% highlight bash %}
$ behat math.feature 
# features/bootstrap/FeatureContext.php
Feature: As a mathematician I want to have a basic Calculator to add two numbers so 
         that I can get the result.
 
  Background:                     # math.feature:4
    Given I have basic calculator 
      TODO: write pending definition
 
  Scenario Outline:                    
    Given I have entered <number1>     
    And I have entered <number2>       
    When I add                         
    Then The result should be <result> 
 
    Examples:
      | number1 | number2 | result |
      | 10      | 12      | 22     |
      | 5       | 3       | 8      |
      | 5       | 5       | 10     |
 
3 scenarios (3 pending)
15 steps (12 skipped, 3 pending)
0m0.05s
{% endhighlight %}

We have now the core of our Feature Context file, we need to implement the basic calculator Class, so we switch now to TDD. In your tests folder, edit a file called mathTest.php

{% highlight php %}
$ vi mathTest.php
<?php
 
require_once '$PATH_TO_YOUR_CLASS/math.php';                                          
 
class MathCalculatorTest extends PHPUnit_Framework_TestCase
{
 
    public function testClassInstantiate()
    {
        $this->assertInstanceOf('MathCalculator', new MathCalculator);
    }
}
?>
{% endhighlight %}

and your MathCalulator class:

{% highlight php %}
$ vi ../math.php
<?php
 
class MathCalculator
{
}
 
?>
{% endhighlight %}

now, let's run our test:

{% highlight php %}
$ phpunit tests/mathTest.php
PHPUnit 3.6.7 by Sebastian Bergmann.
.
Time: 0 seconds, Memory: 2.75Mb
 
OK (1 test, 1 assertion)
{% endhighlight %}

We have tested that our Class can be instantiated with TDD, we need to update our behat Feature Context file:

{% highlight php %}
$ vi bootstrap/FeatureContext.php
...
require_once 'mink/autoload.php';
require_once '$PATH_TO_YOUR_CLASS/math.php';

// or, if you want to use phar from current dir:
// require_once __DIR__ . '/mink.phar';
 
class FeatureContext extends Behat\Mink\Behat\Context\MinkContext
{
    /**
     * @Given /^I have basic calculator$/
     */
    public function iHaveBasicCalculator()
    {
        $this->mycalculator= new MathCalculator();
    }
...
{% endhighlight %}

Let's run our behat again:

{% highlight php %}
$ behat math.feature 
# features/bootstrap/FeatureContext.php
Feature: As a mathematician I want to have a basic Calculator to add two numbers so that I can get the result.
 
  Background:
    Given I have basic calculator 
 
  Scenario Outline:                    
    Given I have entered <number1>     
    And I have entered <number2>       
    When I add                         
    Then The result should be <result> 
 
    Examples:
      | number1 | number2 | result |
      | 10      | 12      | 22     |
        TODO: write pending definition
      | 5       | 3       | 8      |
        TODO: write pending definition
      | 5       | 5       | 10     |
        TODO: write pending definition
 
3 scenarios (3 pending)
15 steps (3 passed, 9 skipped, 3 pending)
0m0.059s
{% endhighlight %}

We now need to implement the function to set our number value in an array, we go back to TDD and implement 
the test to set it:

{% highlight php %}
$ vi tests/mathTest.php
<?php
...
    public function testsetNumber()
    {
        $this->mathCalculator->setNumber(5);
    }    
..
{% endhighlight %}

Let's run our PHP Unit:
{% highlight php %}
$ phpunit tests/mathTest.php
PHPUnit 3.6.7 by Sebastian Bergmann.

 .PHP Fatal error:  Call to undefined method MathCalculator::setNumber() in 
 /home/ctulhu/Dropbox/PHP_Cert/tutorials/oop/behat/features/tests/mathTest.php 
 on line 19
{% endhighlight %}

Obviously our test failed, we need to implement the function into our Class:

{% highlight php %}
$ vi ../math.php
 
<?php
class MathCalculator
{
    private $numbers;
 
    function setNumber($number = 0)
    {
        $this->numbers[]=intval($number);
    }
}
?>
{% endhighlight %}

and run our PHP Unit again:

{% highlight php %}
$ phpunit tests/mathTest.php
PHPUnit 3.6.7 by Sebastian Bergmann.
..
Time: 0 seconds, Memory: 2.75Mb
OK (2 tests, 1 assertion)
{% endhighlight %}

Our Unit Test pass, let's complete our Unit Test and refactor a little, in order to assert the setter we need 
to implement the getter and assert its value.

{% highlight php %}
$ vi tests/mathTest.php
<?php
class MathCalculatorTest extends PHPUnit_Framework_TestCase
{
    private $_mathCalculator;
 
    public function setUp() {
        $this->mathCalculator = new mathCalculator();
        $this->mathCalculator->setNumber(5);
        $this->mathCalculator->setNumber(5);
    }
 
    public function testClassInstantiate()
    {
        $this->assertInstanceOf('MathCalculator', new MathCalculator);
    }
 
    public function testsetNumber()
    {
        $result=$this->mathCalculator->getNumber();
        // Verify that result array contain 2 value;
        $this->assertEquals(2, count($result));
    }
}
{% endhighlight %}

Let's add the getter in our Math Class:

{% highlight php %}
$ vi ../math.php
<?php
...
    function getNumber()
    {
        return $this->numbers;
    }
...
{% endhighlight %}

and run our Unit Test again:

{% highlight php %}
$ phpunit tests/mathTest.php
PHPUnit 3.6.7 by Sebastian Bergmann.
..
Time: 0 seconds, Memory: 2.75Mb
OK (2 tests, 2 assertions)
{% endhighlight %}

Let's implement our testsumNumber test method:

{% highlight php %}
$ vi tests/mathTest.php
<?php
...
    public function testsumNumber()
    {
        $result=$this->mathCalculator->sumNumber();
 
        // Verify that result array contain 2 value;
        $this->assertEquals(10, $result);
    }  
...
{% endhighlight %}

and run our Unit Test again:

{% highlight php %}
$ phpunit tests/mathTest.php
PHPUnit 3.6.7 by Sebastian Bergmann.
 
..PHP Fatal error:  Call to undefined method MathCalculator::sumNumber() in 
  tests/mathTest.php on line 32
{% endhighlight %}

Let's add a constructor and the sumNumber method in our Math Class:

{% highlight php %}
$ vi math.php
<?php
class MathCalculator
{    
    private $result;
    private $numbers;
 
    function __constructor()
    {
        $this->$result=0;
        $this->$numbers=0;
    }
...
    function sumNumber()
    {
        $this->result  = array_sum($this->numbers);
        $this->numbers = array();
        return $this->result;
    }  
...
{% endhighlight %}

and run our Unit Test again:

{% highlight php %}
$ phpunit tests/mathTest.php
PHPUnit 3.6.7 by Sebastian Bergmann.
...
Time: 0 seconds, Memory: 2.75Mb
OK (3 tests, 3 assertions)
{% endhighlight %}

Our Class Math has been tested and developed with PHPUnit, we now have our code covered which
ensure that our test will break if any bad changes are made in our math Class.

Let's finish our behat FeatureContext file:

{% highlight php %}
$ vi bootstrap/FeatureContext.php
<?php
...
    /**
     * @When /^I add$/
     */
    public function iAdd()
    {
        $this->calculatedResult=$this->mycalculator->sumNumber();
    }
 
    /**
     * @Then /^The result should be (\d+)$/
     */
    public function theResultShouldBe($result)
    {
        assertEquals($result, $this->calculatedResult);
    }
...
{% endhighlight %}

Note the change of the $argument to $result in the function theResultShouldBe

Let's run our behat:

{% highlight php %}
$ behat math.feature 
# features/bootstrap/FeatureContext.php
Feature: As a mathematician I want to have a basic Calculator to add two numbers so 
         that I can get the result.
 
  Background:  
    Given I have basic calculator
 
  Scenario Outline:                    
    Given I have entered <number1>     
    And I have entered <number2>       
    When I add                         
    Then The result should be <result> 
 
    Examples:
      | number1 | number2 | result |
      | 10      | 12      | 22     |
      | 5       | 3       | 8      |
      | 5       | 5       | 10     |
 
3 scenarios (3 passed)
15 steps (15 passed)
0m0.056s
{% endhighlight %}

We have our math behat feature test passing, so we have covered the Acceptance testing.

Let's assume our Product Owner want to add new values and expected results because some 
business rules as changed:

{% highlight php %}
$ vi math.feature
...
    Examples:
      | number1 | number2 | result |
      | 10      | 12      | 22     |
      | 5       | 3       | 8      |
      | 5       | 5       | 10     |
      | 100     | 5       | 105    |
      | 66      | 33      | 100    | 
{% endhighlight %}

Let's run our behat:

{% highlight php %}
$ behat math.feature 
# features/bootstrap/FeatureContext.php
Feature: As a mathematician I want to have a basic Calculator to add two numbers so 
         that I can get the result.
 
  Background:
    Given I have basic calculator 
 
  Scenario Outline:                    
    Given I have entered <number1>     
    And I have entered <number2>       
    When I add                         
    Then The result should be <result> 
 
    Examples:
      | number1 | number2 | result |
      | 10      | 12      | 22     |
      | 5       | 3       | 8      |
      | 5       | 5       | 10     |
      | 100     | 5       | 105    |
      | 66      | 33      | 100    |
        Failed asserting that 99 matches expected '100'.
 
5 scenarios (4 passed, 1 failed)
25 steps (24 passed, 1 failed)
0m0.083s
{% endhighlight %}

This approach may take a little longer but you see the advantages, it switch the 
thinking about the testing before the development, it structure your Test and Class, coupled
with a Continuous Integration you will get instant feedback when something is broken in the 
code but also with the Acceptance Testing.

