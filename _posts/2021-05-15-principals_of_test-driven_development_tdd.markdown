---
layout: post
title:      "Principals of Test-Driven Development (TDD)"
date:       2021-05-15 19:31:20 +0000
permalink:  principals_of_test-driven_development_tdd
---


​	Before I get started with the [principals of TDD](https://www.agilealliance.org/glossary/tdd), it’s important to distinguish the other different testing patterns; when to (or not to) use them, the distribution of them, and why that matters.

​	 TDD (Unit Testing) will be covered in-depth below so I’ll start with [Integration Testing](https://softwaretestingfundamentals.com/integration-testing). Integration Testing will test the behavior or “integration” between multiple areas of an application. [End-to-End](https://www.browserstack.com/guide/end-to-end-testing) (E2E) testing will test the application in large parts from a user’s perspective and the behavior of the application based on a user actions.

​	 The reason behind having a heavy emphasis on Unit testing and TDD is mostly for Code Coverage (more on that later) and speed. In terms of size, Unit test run very quickly and efficiently, even with thousands of them. The more thorough the test, the longer it takes to run through. Take a look at the diagram below, at the top of the pyramid tests take significantly longer to run but, cover more of the application. The general rule of thumb and best practice is 70% Unit, 20% Integration, and 10% E2E tests.

![test_pyramid](https://codeahoy.com/img/blogs/test_pyramid.png)

​		 														<sub>[Image source](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html)</sub>

​	TDD refers to a design pattern for programming in which a test is written on the behavior of one specific component or “unit”, then writing just enough code to make the test pass. Once the test passes, refactor the component to it’s simplest form and repeat the process for each new component. Test should not be overly large or, by the same concept, overly trivial. Test should be run often, preferably using Continuous Integration (CI) and Continuous Deployment (CD). Make full use of CI/CD pipelines and workflows for optimized automation, the more automation the better.

​	Use [Code Coverage](https://www.codegrip.tech/productivity/everything-you-need-to-know-about-code-coverage/) as an approach to measure the amount of code covered by tests. The [Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control) logs should show testing activity along with code changes and maintenance. Test should be written before the corresponding code. When a bug is found, write tests to add coverage before adding code to fix it. Tests here should be as simple as possible, fast as possible, and a good reference for bugs. Unit Tests should not overlap existing code, if there is a necessity to test multiple areas of code think “Integration Test”. When testing, make sure use many assertions meaning, for the same input test multiple outputs. For example, if you’re testing to make sure two numbers are added together properly, “assert” that `1 + 1 = 2` and that `2` is a Number and not a String.

​	Code Coverage is a metric used to show the percentage of code. There are four different methodologies that can be measured. Statement Coverage measures the amount of code (in lines) that is covered at least once. Function Coverage measures the amount of functions that are covered. Condition Coverage measures code that would have a true or false value and that each test covers both values. Branch Coverage measures the coverage for any “branching” (conditional) logic not resulting in a true or false value.



Resources:

[Agile - TDD](https://www.agilealliance.org/glossary/tdd)

[Integration Testing](https://softwaretestingfundamentals.com/integration-testing)

[E2E Testing](https://www.browserstack.com/guide/end-to-end-testing)

[Medium - TDD](https://medium.com/ibm-garage/solid-design-principles-makes-test-driven-development-faster-and-easier-35c9eec22ff1)

[Principles of TDD](https://chromatichq.com/blog/principles-testdriven-development)

[Google Blog - E2E](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html)

[Code Grip - Code Coverage](https://www.codegrip.tech/productivity/everything-you-need-to-know-about-code-coverage/)

[Blog - Code Coverage](https://www.softwaretestinghelp.com/code-coverage-tutorial/)

[Git - Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)


