---
layout: article
title: Clean Tests
categories: [testing]
tags: [clean, code, unit, first, fast, uncle, bob]
author: Declan McKenna
icon-url: images/clean-tests/clean-screen.svg
---

## The Dual Standard

Do not adopt a dual standard where readability is neglected for tests.

As your code changes your tests will have to
change. People who did not initially write them will have to understand them so that they can adapt, extend,
or remove them.

XCTest classes can and should have instance variables and helper functions to enhance readability and reduce repetition.
Both of which are perfectly safe to use, instance variables will be reset upon every test case.
Helper functions will not be treated as tests as long as they don't start with the word test.

## Smaller Tests
Minimize the number of asserts and **test just one concept per test function.**

Smaller tests are easier to read and give clearer failure results.
A tests failure should ideally be identifiable by its name and not require the user to dive deep in to the test code.

Any repetitive setup involved in doing this should be handled by <code>setup()</code>, instance variables or
private helper functions

<figure>
{% include code-snippets/big-test.html %}
<figcaption>Bad: Three different scenarios bundled in to one test</figcaption>
</figure>

<figure>
{% include code-snippets/small-tests.html %}
<figcaption>Good: Tests split up for different scenarios</figcaption>
</figure>
