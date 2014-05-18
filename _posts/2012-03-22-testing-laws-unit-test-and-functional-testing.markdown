---
layout: post
title:  "Testing Laws: Unit Test and Functional Testing"
date:   2012-03-22 21:47:09
categories: testing
---

#**Testing Laws:**

* Not all test that passed for the first time will necessary passed for the second one.
* If you can't test the software that you build, you shouldn't build it.
* Examining a program to see if it does not do what it is supposed to do is only half of the battle. The other half is seeing whether the program does what it is not supposed to do.
* Test Early
* It is vital to foster a 100% positive, inclusive and team-oriented approach with a “test to break” mental attitude.
* Automation is not a replacement of Manual Testing
* A test case must include a definition of the expected result of the computer software program being tested.
* Test cases should be written in order to include unforeseen and invalid user inputs, as well as foreseen, valid user input.
* The proliferation of errors in a computer software program can be prevented through the employment of testing during the early stages of the software development lifecycle.

#**What is a Unit Test?**
It’s a test on a single unit, such as a class or method, in isolation.

>"Whenever you are tempted to type something into a print statement or a debugger expression, write it as a test instead." --Martin Fowler

#**Laws of TDD:**

* You are not allowed to write any production code unless it is to make a failing unit test pass.
* You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
* You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

#**The difference between Unit Tests and Acceptance Tests:**

* Unit Tests ensure you build the thing right (The code Testing)
* Functional Tests ensure you build the right thing (The functional Testing)
