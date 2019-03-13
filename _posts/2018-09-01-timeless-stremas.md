---
layout: post
title: Timeless Streams
tags: [EventSourcing, Streams, Kafka, Architecture]
comments: true
---

Event Sourcing as an application architecture pattern is raising in popularity  

Logging helps to diagnose problems and record what happened, therefore it is a common practice in software development. 
Unfortunately, developers usually do not put a lot of effort in writing logs. 

Most of the times, logs are not treated as production code, there is no test coverage around them, so adding, changing or removing a log does not make a test fail. 
Only when a developer has to investigate an issue in production, she realises that the logs are incomplete or incorrect, but unfortunately it is already too late.

Logging should be treated as a **first class citizen** in every system that aims to be easily diagnosed and maintained.

As developers, we tend to be lazy and because adding logging after the production code has been done is boring, we tend to miss it. 

Test driving your logs will help you increase the quality of your logs, and your confidence in them. 
Also will drive the rules and alerts in the monitoring systems. 

Because logging only makes sense in a context, writing tests first for it will help you remove noise around them, avoid mistakenly logging sensitive information or objects references.
 
We should test how robust are our non-functional capabilities, and not only our functional features. 
Being able to diagnose, and ultimately fix issues, is a non-functional dimension that should be subject to the same standards as performance, reliability or security.

## What is a Log

A Log is immutable, ordered, append-only and persistent data structure. It’s a very simple structure, but it is very powerful. 

Logs have a specific purpose: they record what happened, when and where. 

Logs are everywhere and used in many ways as:

- Application logs
- Database store data on disk reliably
- Database replicas synchronise
- Distributed streaming platform as Apache Kafka

## Common mistakes with logs

Some of the most common issues with logs I have found over time are.

#### Logging a object reference instead of its value

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

Fixing a typo, modifying or removing a log message might have implications in your monitoring systems. For example, alert rules 
might not get triggered or dashboards might display wrong information.

# Test driving log messages 

Ideally the expected message will be defined when a story is refined, but when that is not the case, 
writing a test for it will help you think about it from this perspective. 
Even just thinking about it when writing the test, will automatically improve its quality.  

When there is an unhappy path, the log message is fundamental. Writing the outside test should assert on your expected log message.

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

Implementing the feature will help you drive out the details. When test the insides, the assert should focus on the details.

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

There are more examples on how to use the library in [here](https://github.com/mustaine/logcapture/blob/master/src/test/java/com/logcapture/example/ExampleShould.java){:target="_blank"}.

## Conclusions

Test driving logs helps building production systems with confidence and the quality of its logs, thus making it easier to diagnose issues. 
Many of our monitoring dashboards are built from our logs. Alerts are triggered from our logs.
So treat your logs as a first class citizen and write tests first around them.

Once the logs are covered by tests, developers will be forced to care about them. Standards will be agreed around logging. 
Logs quality will increase and the time for investigating and resolving issues will be reduced, so we will all have more time to deliver new features.

## References

[I Heart Logs: Event Data, Stream Processing, and Data Integration](http://shop.oreilly.com/product/0636920034339.do) - by [@jaykreps](https://twitter.com/jaykreps)

[Narrative](https://github.com/felipefzdz/narrative) - by [@felipefzdz](https://twitter.com/felipefzdz)

[Event sourcing, CQRS, stream processing and Apache Kafka: What’s the connection?](https://www.confluent.io/blog/event-sourcing-cqrs-stream-processing-apache-kafka-whats-connection/)








