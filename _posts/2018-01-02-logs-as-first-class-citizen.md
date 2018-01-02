---
layout: post
title: Logs as first class citizen
tags: [TDD, production systems, thoughts]
comments: true
---

Logging helps to diagnose problems and record what happened, therefore it is a common practice in software development. 
Unfortunately, developers usually do not put a lot of effort when writing logs. 

Most of the times, logs are not treated as production code, there is no test coverage around them, so adding, changing or removing a log does not make a test to fail. 
Once the developer has to investigate an issue in production is when she realises the logs are incomplete, but unfortunately it is too late.

Logging should be treated as a **first class citizen** in every system that aims to be easily diagnosed and maintained.

As developers, we tend to be lazy, so logging after the production code has been done is boring, therefore we tend to miss it. 

Logging/testing first will drive your production code, achieving higher confidence and quality of your logs. 
It will drive the rules and alerts in the monitoring systems. 

Logging make sense in context, writing tests first should reduce the noise around and avoiding logging private information as passwords/tokens or object references.
 
We should test how robust are our non-functional capabilities, and not only our functional features. 
Being able to diagnose, and ultimately fix, issues is a non-functional dimension that should be subject to the same standards as performance, reliability or security.

## What is a Log

A Log is a data structure, immutable, ordered, append-only and persistent. It’s a very simple structure, but it is very powerful. 

Logs have a specific purpose: they record what happened, when and where. 

Logs are everywhere and used in many ways as:

- Application logs
- Database store data on disk reliably
- Database replicas synchronise
- Distributed streaming platform as Apache Kafka

## Common mistakes with logs

Some of the most common issues with the logs I have found over time are.

#### Logging a object reference instead of the value

{% highlight java %}
ObjectToPrint data = new ObjectToPrint("aValue");
log.error("an error {}", data);
{% endhighlight %}

{% highlight console %}
an error com.logcapture.LoggingErrors$ObjectToPrint@16b4a017
{% endhighlight %}

#### Missing context or bad quality log message

{% highlight java %}
ObjectToPrint data = new ObjectToPrint("aValue");
log.error("an error updating reference");
{% endhighlight %}

#### Logging sensitive information

{% highlight java %}
SensitiveObject data = new SensitiveObject("passw0rd");
log.error("an error {}", data);
{% endhighlight %}

{% highlight console %}
an error SensitiveObject{password='passw0rd'}
{% endhighlight %}

#### When a log message changes, rules of the monitoring system are not updated

Fixing a typo, modifying or removing a log message might have implications in your monitoring systems like 
rules that are not triggered or dashboards displaying wrong information.

# TDD logging messages 

Ideally the expected message will be defined in the requirement/story. 
But when it is not the case, having to assert the message will make you think how should looks like at this point. 
I think it is very important, because thinking about the log message, is by definition, increasing the quality of it.  

When there is an unhappy path, the log message is a requirement. Writing the outside test should assert on your expected log message

{% highlight scala %}
  scenario("Customer can play out if pass activation fails in UMV with pass already activated error") {
    val request = `play live request`
      .`coming from the`(user)
      .`attempting to watch the`(SportsChannel)
      .`having a valid entitlement activation pass for`(SportsChannel, Sports1W)
      .`with an entitlement already activated for the same pass but different duration`

    whenThe(request.`has been sent`)
      .thenThe(`response is successful`)
      .and(`the userToken has not been updated`)
      .and(`a log entry with`(aLog().warn().withMessage(startsWith("Pass already activated (UMV_015)")))
  }
{% endhighlight %}

Implementing the feature will drive you to the details. When test the insides, the assert should focus in the details.

{% highlight java %}
    
@Test
public void activateEntitlementDoesNotReturnAnEntitlementIfIsAlreadyActivated_ExceptionUMV_015IsThrown() throws Exception {
  // given
  activateEntitlement(pass).willThrow(new UmvException("UMV_015", "type entitlement already activated"));

  // when
  Optional<Entitlement> entitlement = umvEntitlements.activate(originalUserToken, pass);

  // then
  assertThat(entitlement).isEmpty();
  logCaptureRule.logged(aLog().warn().withMessage("Pass already activated (UMV_015): Pass{type=pass.type(), duration='pass.duration()', entitlementId='null'}");
}

{% endhighlight %}
 
These examples are using [**LogCapture**](https://github.com/mustaine/logcapture), a testing library for asserting logging messages. 
I expect to have a separate post talking about it, hopefully sooner than this post from my last one 😅

There are a few examples of how to use the library in [ExampleShould.java](https://github.com/mustaine/logcapture/blob/master/src/test/java/com/logcapture/example/ExampleShould.java){:target="_blank"}

## Conclusions

Testing logs first helps to build production systems giving confidence and improving logs quality. 
Dashboards plenty of metrics are built from our logs. Alerts are triggered from our logs.
So treat your logs as a first class citizen and write tests first around them.

Once the logs are covered by tests, some developers will care about them. Standards will be agreed around logging. 
Logs quality will increase and the time for investigate and resolve issues will be reduced, so we all will have more time to deliver more features.

## References

[I Heart Logs: Event Data, Stream Processing, and Data Integration](http://shop.oreilly.com/product/0636920034339.do) - by [@jaykreps](https://twitter.com/jaykreps)

[Narrative](https://github.com/felipefzdz/narrative) - by [@felipefzdz](https://twitter.com/felipefzdz)










