---
layout: post
title: Interface Verbosity
tags: [DDD, thoughts]
comments: true
---

Recently I was thinking about interface verbosity.

Currently I am working on a Spring Framework based project ( <cite>once again ;(</cite> ) and I am seeing a common coding pattern with which I disagree.

I would also like to recommend Uncle Bob's post about ["Interfaces Considered Harmful"](http://blog.cleancoder.com/uncle-bob/2015/01/08/InterfaceConsideredHarmful.html)
which inspired me to write this post.

How many times have you seen a structure like this one?

 {% highlight java %}
 public interface SomeService {
     void someMethod();
     void anotherMethod();
 }

 public class SomeServiceImpl implements SomeService {
      public void someMethod() {
        ...
      }
      public void anotherMethod() {
        ...
      }
  }
 {% endhighlight %}

Looking at the code, I think:

 1. Why the use of suffix "Impl"? (C# developers would use the prefix "I" for interfaces).
 2. Does the implementation really need an interface?

Regarding the first item: this naming pattern is used when the interface has the proper name and the developer can't find a better one for the implementation.

Second item: this one is probably related to the [Dependency Inversion Principle (DIP)](http://en.wikipedia.org/wiki/Dependency_inversion_principle):

        A. High-level modules should not depend on low-level modules. Both should depend on abstractions.
        B. Abstractions should not depend on details. Details should depend on abstractions.

Whenever it is hard to find a different interface/implementation name - this is hinting us that the interface only needs a <b>single</b> implementation.

In the old-style Spring flow pattern  "Controller -> Service -> DAO" we often see one interface paired with its implementation for each Service and DAO. But actually in 95%
of all scenarios only one interface implementation is required. (I am guessing it, don't take this as a law)

So should we always create 2 objects when we build a component? I would say <b>NO</b>.

I agree that there are some services which could have multiple implementations like e.g:

{% highlight java %}
 public interface PaymentService {
     void payAnInvoice();
 }

 public class StripePaymentService implements PaymentService {
      public void payAnInvoice() {
        ...
      }
  }

  public class PayPalPaymentService implements PaymentService {
        public void payAnInvoice() {
          ...
        }
    }

    ...
  {% endhighlight %}

In such cases... it is great to use an interface and inject the interface as a dependency. But it does not apply to all components you need. (<cite>To be honest, only in rare cases
you need an interface with multiple implementations</cite>)

## Conclusion

I think that interfaces are great when you are developing an API; they act like contracts between systems; or when you need multiple implementations.

But I am sure that we <b>should not</b> use an interface for every component we create and, sadly, in the Spring eco-system interfaces are used <del>abused</del> much to often.













