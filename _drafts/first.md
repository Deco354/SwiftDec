---
layout: article
title: F.I.R.S.T
categories: [testing]
tags: [clean, code, unit, first, fast, uncle, bob]
author: Declan McKenna
icon-url: images/unit-testing-best-practices/check-box.svg
---

## Fast
We want to run these tests regularly and often by smashing <kbd>command U</kbd> whenever we’ve made changes or we're
about to commit our work.

For this to work we need our tests to be fast.
How fast is fast? We’re going to be writing a lot of tests so at the very minimum a test should run in less than 0.1 seconds.

## Independent
Don’t write tests that depend on each other.

Xcode actively helps us with this. Every function in an XCTestCase class that begins with “test” will receive a
new instance of your test class so no instance variables will be carried over.

`setup()` and `teardown()` can be used to clear persistent resources but it would be better
still to mock them.

You should be able to run your tests in any order and have them pass.

## Repeatable
Tests should run on any environment.
If your unit tests only work on your laptop, your companies VPN or with a
network connection they can’t be run reliably and your team will second guess why they’ve failed or ignore them.

## Self-Validating
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
