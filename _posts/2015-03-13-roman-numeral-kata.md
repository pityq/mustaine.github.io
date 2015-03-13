---
layout: post
title: Roman Numerals Kata
excerpt: "J blocks, and more."
modified: 2013-05-31
tags: [TDD, kata]
comments: false
---

In order to practice [Test-Driven-Development (TDD)](http://en.wikipedia.org/wiki/Test-driven_development), I did the [Roman Numerals Kata](https://github.com/mustaine/katas/tree/master/roman-numerals).
TDD has 3 simple steps, even their are easy, it is difficult to educate yourself to follow them

 1. Write a failing test
 2. Write the simplest bit of code that makes it pass
 3. Refactor the code to follow the rules of simple design


I wanted to practice TDD and solve the problem using Java8.


    I started with the most simple case, write a test for convert the decimal number 1 -> I
It was straight forward, so I continue writing test cases to convert other decimals numbers to roman numerals, but doing baby steps in each iteration.
Once I was comfortable converting decimal numbers to roman numerals, I added the cases for what I consider the exceptions.

 * The '1' symbols ('I', 'X', and 'C') can only be subtracted from the 2 next highest values ('IV' and 'IX', 'XL' and 'XC', 'CD' and 'CM').
 * The '5' symbols ('V', 'L', and 'D') can never be subtracted.

If you are interested on the steps that I follow to solve the kata, you can check the [history commits](https://github.com/mustaine/katas/commits/master/roman-numeral)

{% gist mustaine/0885ce42e46c0b1e129d %}

An interesting video from Sandro Mancuso [video](https://youtu.be/iZjgj1S0FCY?list=PLGS1QE37I5lQX33-yrnNasV_dHRh2oSkx)