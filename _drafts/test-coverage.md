---
layout: article
title: Test Coverage
categories: [testing]
tags: [clean, code, unit, coverage]
author: Declan McKenna
icon-url: images/test-coverage/percentage-monitor.svg
---

## How to get test coverage on Xcode?
Using a coverage tool allows you to immediately see gaps in your testing strategy.
Xcode has a built in coverage tool which at the time of writing is not enabled by default.

<figure>
  <img src="/images/test-coverage/scheme-screenshot.png" alt="Scheme settings screenshot">
  <figcaption>Enable code coverage by going to your scheme -> <em>edit schemes -> Test -> Options</em> and check <em>Gather coverage</em></figcaption>
</figure>
<figure>
  <img src="/images/test-coverage/coverage-screenshot.png" alt="Test coverage report screenshot">
  <figcaption>View your projects coverage within the reports tab <kbd>cmd-9</kbd></figcaption>
</figure>
<figure>
  <img src="/images/test-coverage/untested-highlighting-screenshot.png" alt="Xcode untested code highlighting screenshot">
  <figcaption>Xcode will highlight untested functions with a red margin</figcaption>
</figure>

## A warning on Code coverage
Code coverage reports are great but don't treat them as the source of all truth.
**Just because a function has test coverage doesn't mean it's fully tested.**

Your tests should be testing the functionality of your code not the functions.
Push your code to the limit and write tests for every edge case you can think of. It's better to have too many tests than too few.

Let's take an example from a previous post where we have an `addMonths(_: Int)` function that adds a month to the current date.
One test that calls `addMonths` will satisfy code coverage tools that the function is tested but there is
far more behavior to be tested than just calling this function once.

Rather than just test the function addMonth() we should be testing
* Adding a month on the 30th January gives us February 28th
* Adding a month on the 31st May gives us the 30th of June
* Adding two months on the 31st May gives us the 31st of July

On the other side of the coin I wouldn't suggest always striving for 100% coverage. Xcode will often want coverage for things such as enums cases or state with no functionality which are likely not worth your time.
