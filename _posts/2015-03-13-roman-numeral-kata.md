---
layout: post
title: Roman Numerals Kata
excerpt: "J blocks, and more."
modified: 2013-05-31
tags: [TDD, kata]
comments: true
---

Practicing [Test-Driven-Development (TDD)](http://en.wikipedia.org/wiki/Test-driven_development) with the [Roman Numerals Kata](https://github.com/mustaine/katas/tree/master/roman-numerals).

[Uncle Bob](http://en.wikipedia.org/wiki/Robert_Cecil_Martin) defines the [3 rules of TDD](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd)

1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

I wanted to practice TDD and some Java8, so I tried to solve the problem using streams and lambda expressions.

To start with, I wrote the most simple test scenario.

{% highlight java %}
assertThat(RomanNumeral.romanFrom(1), is("I"));
{% endhighlight %}

It was straight forward, so once I wrote the code to make it pass, I added more test cases:

{% highlight java %}
assertThat(RomanNumeral.romanFrom(1), is("I"));
assertThat(RomanNumeral.romanFrom(2), is("II"));
assertThat(RomanNumeral.romanFrom(3), is("III"));
{% endhighlight %}

Once the code made the tests pass, it was clear that this code needed refactoring.

{% highlight java %}
public static String romanFrom(int number) {
    if(number == 3) return "III";
    if(number == 2) return "II";
    return "I";
 }
{% endhighlight %}

So I refactored it.

I didn't want to add the cases where the number is reduced by subtraction like e.g. 4 -> IV, instead I added a new letter to convert 5 -> V.

After having passed this test, I continued by adding more numbers like 6 -> VI, 8 -> VIII...

Before adding the numbers that require subtraction, I decided to test the algorithm with more letters, so I added 10 -> X.

Once the new code made the test pass...

{% highlight java %}
public static String romanFrom(int number) {
    if (number == 5) return "V";
    return IntStream.iterate(number, i -> i - (i >= 10 ? 10 : i >= 5 ? 5 : 1))
        .limit(number)
        .filter(i -> i > 0)
        .mapToObj(i -> i >= 10 ? "X" : i >= 5 ? "V" : "I")
        .collect(Collectors.joining());
}
{% endhighlight %}

and again it needed refactoring.

So I refactored it by adding an Enum containing letters and decimals numbers like so:
{% highlight java %}
enum RomanToDecimal {
  X(10),
  V(5),
  I(1)
}
{% endhighlight %}

After the Enum was added, it seemed easy to add more numbers, excluding those that require subtraction.

I added roman numerals until thousand (M) and the algorithm was working, so I felt comfortable to add the remaining numbers which require subtraction.
{% highlight java %}
enum RomanToDecimal {
  M(1000), CM(900),
  D(500), CD(400),
  C(100), XC(90),
  L(50), XL(40),
  X(10), IX(9),
  V(5), IV(4),
  I(1);
{% endhighlight %}

After adding these cases to the Enum, the implementation was working fine.

I am pretty sure, that if I would have tried to solve this problem without following TDD, the result would have been different and probably not so clean.

To follow the steps for solving the kata, have a look at the [history commits](https://github.com/mustaine/katas/commits/master/roman-numeral).

Before seeing my solution, I would encourage you to try to solve it and share your solution. I would also recommend to watch this [video](https://youtu.be/iZjgj1S0FCY?list=PLGS1QE37I5lQX33-yrnNasV_dHRh2oSkx) from [Sandro Mancuso](@sandromancuso) where he solves this problem.

My solution of the problem after refactoring is:

{% gist mustaine/0885ce42e46c0b1e129d %}






