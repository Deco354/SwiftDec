---
layout: article
title: F.I.R.S.T
categories: [testing]
tags: [clean, code, unit, first, fast, uncle, bob]
author: Declan McKenna
icon-url: images/first/1st-medal.svg
---

F.I.R.S.T is a widely used acronym for good testing practices.

## Fast
We want to run our tests regularly and often by smashing <kbd>cmd U</kbd> whenever we’ve made changes or we're about to commit our work.

For this to work we need our tests to be fast.
How fast is fast? We’re going to be writing a lot of tests so at the very minimum **a test should run in less than 0.1 seconds.**

You can see how fast your tests are by going to Xcode's report manager tab (<kbd>cmd 9</kbd>) and clicking on your last test's report.

<figure>
  <img src="/images/first/xcode-test-report.png" alt="Xcode test report screenshot">
  <figcaption>Xcode test speed report</figcaption>
</figure>

## Independent
Don’t write tests that depend on each other.

Our test suite is much more useful to us if our failing test immediately highlight the problem rather than having an entire batch of tests fail because one test they all depended on failed and you now have to go investigate which test caused the failure.

**You should be able to run your tests in any order and have them pass.**
<figure>
  <img src="/images/first/xcode-randomization.png" alt="Xcode randomization screenshot">
  <figcaption>Randomizing test execution in Xcode</figcaption>
</figure>

Instance variables are already taken care of. Every function in an XCTestCase class that begins with “test” will receive a new instance of your test class so no instance variables will be carried over between tests. `setup()` and `teardown()` can be used to clear persistent resources but it would be better still to mock them.

## Repeatable
Tests should run within any environment.

If your unit tests only work on your laptop, your companies VPN or with a
network connection they can’t be run reliably and your team will second guess why they’ve failed or ignore them.

**Unit tests do not test external systems.** External dependencies such as databases or network connections should be mocked out.

## Self-Validating
Tests should simply fail or pass and not rely on a log. The name of a test should tell you immediately what failed and not require the inspection of a log or diving in to the test code.

One large test function that tests many cases and puts them in to a log can be easier to write but it will cost you a lot of time in the long run as the inspection and maintanance of this log will become a tedious manual process that you have to instruct your colleagues to maintain and use. It will soon fall in to disrepair.

## Timely
Write tests with all code you commit.
If you do it afterwards your production code may be difficult to test especially once more functionality is added to it.
