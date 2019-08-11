---
title: Unit Testing Best Practices
author: Declan McKenna
date: Jun 29th 2019
layout: article
icon-url: images/unit-testing-best-practices/check-box.svg
---

When I first learnt how to unit test every tutorial I came across would show me how to write a test for a simple model and it seemed pretty straight forward.

When I tried to apply what I learnt in my project I soon encountered a lot
of difficulties when trying to test:
* ViewControllers
* Private functions
* Static functions
* Network calls
* UserDefaults
* "Legacy Code"

Unit testing isn’t nearly as simple as it first appears. I feel this is one of the main reasons so many teams have little to no coverage in their projects.
Yet having good coverage is vital to having a maintainable project.
<blockquote>"Unit tests keep our code flexible, maintainable and reusable. Without them every change is a
possible bug."<footer>Uncle Bob - <cite>Clean Code</cite></footer>
</blockquote>


Over the coming series of blog posts I will attempt to cover the questions above when it comes to unit testing in Xcode.

For now we will go over some overarching concepts I have found very useful mostly taken from Robert Martin’s
excellent book Clean Code.

## F.I.R.S.T
All tests should follow the FIRST principles

### Fast
We want to run these tests regularly and often by smashing <kbd>command U</kbd> whenever we’ve made changes or we're
about to commit our work.

For this to work we need our tests to be fast.
How fast is fast? We’re going to be writing a lot of tests so at the very minimum a test should run in less than 0.1 seconds.

### Independent
Don’t write tests that depend on each other.

Xcode actively helps us with this. Every function in an XCTestCase class that begins with “test” will receive a
new instance of your test class so no instance variables will be carried over.

`setup()` and `teardown()` can be used to clear persistent resources but it would be better
still to mock them.

You should be able to run your tests in any order and have them pass.

### Repeatable
Tests should run on any environment.
If your unit tests only work on your laptop, your companies VPN or with a
network connection they can’t be run reliably and your team will second guess why they’ve failed or ignore them.

### Self-Validating
Tests should simply fail or pass and not rely on a log.


I slipped up on this point when writing one of my first “unit tests” which tested the parsing of phone numbers from
email text. It had a giant log with the % of phone numbers we parsed alongside a print out of all the ones which had
failed and a spreadsheet I’d keep up to date with the results for each version of our app.

This was an unnecessary manual process which made the tests unmaintainable and required inside
knowledge to utilize.

If these tests had been written individually the name of the test would immediately tell the user
what had broke.

### Timely
Write tests with all code you commit.
If you do it afterwards your production code may be difficult to test especially once more functionality is added to it

## Clean Tests

Do not adopt a dual standard where readability is neglected for tests.

As your code changes your tests will have to
change. People who did not initially write them will have to understand them so that they can adapt, extend,
or remove them.

XCTest classes can and should have instance
variables and helper functions to enhance readability and reduce repetition.

## Smaller Tests
Minimize the number of asserts and test just one concept per test function.

Smaller tests are easier to read and give clearer failure results.
Any repetitive setup involved in doing this can be handled by <code>setup()</code> or
private helper functions

<figure>
{% splash %}
func testAddMonths() {
    let may31 = SwiftyDate(day: 31, month: 5, year: 2019)

    let addMonthResult = may31.addMonths(1)
    XCTAssertEqual(addMonthResult.day, 30)
    XCTAssertEqual(addMonthResult.month, 6)
    XCTAssertEqual(addMonthResult.year, 2019)

    let addMonthResult2 = may31.addMonth(2)
    XCTAssertEqual(addMonthResult2.day, 31)
    XCTAssertEqual(addMonthResult2.month, 7)
    XCTAssertEqual(addMonthResult2.year, 2019)

    let june30 = SwiftyDate(day: 30, month: 6, year: 2019)
    let addMonthResult3 = june30.addMonths(1)
    XCTAssertEqual(addMonthResult3.day, 30)
    XCTAssertEqual(addMonthResult3.month, 7)
    XCTAssertEqual(addMonthResult3.year, 2019)
}
{% endsplash %}
<figcaption>Bad: Three different scenarios bundled in to one test</figcaption>
</figure>

<figure>
{% splash %}
func testDay31To30DayMonthJump() {
    let addMonthResult = may31.addMonths(1)
    XCTAssertEqual(addMonthResult.day, 30)
    XCTAssertEqual(addMonthResult.month, 6)
    XCTAssertEqual(addMonthResult.year, 2019)
}

func test31st2MonthJumpThroughA30DayMonth() {
    let addMonthResult = may31.addMonths(2)
    XCTAssertEqual(addMonthResult.day, 31)
    XCTAssertEqual(addMonthResult.month, 7)
    XCTAssertEqual(addMonthResult.year, 2019)
}

func testDay30To31DayMonthJump() {
    let addMonthResult = june30.addMonths(1)
    XCTAssertEqual(addMonthResult.day, 30)
    XCTAssertEqual(addMonthResult.month, 7)
    XCTAssertEqual(addMonthResult.year, 2019)
}
{% endsplash %}
<figcaption>Good: Tests split up for different scenarios</figcaption>
</figure>

## Enable Test Coverage
Using a coverage tool allows you to immediately see gaps in your testing strategy.
Xcode has a built in coverage tool which at the time of writing is not enabled by default.

<figure>
  <img src="images/unit-testing-best-practices/scheme-screenshot.png" alt="Scheme settings screenshot">
  <figcaption>Enable code coverage by going to your scheme -> <em>edit schemes -> Test -> Options</em> and check <em>Gather coverage</em></figcaption>
</figure>
<figure>
  <img src="images/unit-testing-best-practices/coverage-screenshot.png" alt="Test coverage report screenshot">
  <figcaption>View your projects coverage within the reports tab <kbd>cmd-9</kbd></figcaption>
</figure>
<figure>
  <img src="images/unit-testing-best-practices/untested-highlighting-screenshot.png" alt="Xcode untested code highlighting screenshot">
  <figcaption>Xcode will highlight untested functions with a red margin</figcaption>
</figure>
## Setup Continuous Integration
Tools such as Jenkins or Travis can run your tests when someone makes a Pull Request and automatically block the PR
if the tests failed.
This is a vital tool to have as without it it’s possible to merge branches with failing tests.
If tests can be ignored then there’s no point in having them.

## Test Behaviours rather than functions
Just because a function has test coverage doesn't mean it's fully tested.
Push your code to the limit and write tests for every edge case you can think of. It's better to have too many tests than too few.

The date tests above are a good example of this.
One test for `addMonths()` will satisfy code coverage tools that the function is tested but there is
far more behaviour to be tested. For example what date do we land on when we add one month to January 30th?

## Conclusion
Testing isn’t as straight forward as most tutorials will tell you, this article doesn’t come close to covering all
the difficulties you can potentially face either.

However it’s one of the most important skills you can learn. It is universally
needed across all languages and platforms and is highly valued in the job market.

Stay tuned and we’ll have more articles diving in to testing in more detail in the coming
weeks.
