---
layout: post
title: Acceptance Tests?
tags: [TDD, OusideIn, bla bla, thoughts]
comments: true
---

Some years ago, I learn that the way I liked to deliver software was Outside-In TDD. Sandro Mancuso 
has talked a lot about it, so if you would like to know more about check [this] or [that]

I tried different approaches to build the outside test. Cucumber, custom DSL, JGiven.

Cucumber is great, being able to write the Spec? at front with Business is very powerful.
The problem I found with Cucumber, is that is hard to maintain when there hundreds scenarios as the steps 
definitions and code are separated. Also, I experienced in many places those defintiions where not written by developers,
sometimes where Business Analyst or QA writting them, which made it even harder. 
 
Using a custom DSL representing your domain, is ideal. You are able to represent your Acceptance using you own Domain.
The experience was very pleasant, the only problem I found is that defining a DSL is hard and takes time. 
At times, this time is not always an option, but if you have the time is a very good option.
In our case, we build a DSL that allowed us to run with the same defintion, integration and/or functional tests

From sometime ago, I have being using JGiven. Because bla bla bla bla

As a practice, writing your Acceptance Test first helps me to now to drives deeper into the 
implementation details. Gives me the feedback, once I am done with the functionallity I am working on
and gives the confidence to change implementation details in the feature knowing the functionallity will keep working
as expected.

I found it very useful to expose these tests as a documentation of the service. Useful for business but 
also for new developers joining the team.












