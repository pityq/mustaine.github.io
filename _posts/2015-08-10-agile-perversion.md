---
layout: post
title: Agile Perversion
tags: [DDD, thoughts]
comments: true
---

For a long time, I was thinking to write this post because. I was not happy how we were spending
the time estimating stories and how people from management were using <b>estimates</b> as deadlines.

<figure>
	<a href="/images/no-estimates.jpg"><img src="/images/no-estimates.jpg"></a>
</figure>

Also because many companies that say they are Agile, focuses on process management using Scrum
but know nothing about eXtremme Programming (XP).

## Story points - bullshit

Estimating stories is trying to predict the time a story will take. But as an estimate,
it is a guess because of the uncertainty and unknowns.

After estimating stories, the release planning takes in consideration these estimates. Other roles takes these
estimates as deadlines. So when there isn't time enough and some story falls from the sprint, some people point to the developers.

Having such behaviour, makes developers overestimate the stories.

After some sprints, come up a *velocity* of the team, another number that some people use
to point to developers when a sprint is under the *velocity* of the team.
So, at the end, developers split the stories in more stories to make more story points.

Making estimates or not doing it, what I believe is that story points
should be used at development level at maximum. Otherwise people will take them as deadlines.

You could think that these behaviour are from companies implementing bad Scrum,
but in my experience the non-technical roles try to converge story points with time, so with deadlines.

Recently I discovered the [#noEstimates](https://twitter.com/hashtag/noestimates){:target="_blank"} discussion on twitter.

Some interesting articles about it

* ["My First, Last & Only Blog Post About #NoEstimates"](http://codemanship.co.uk/parlezuml/blog/?postid=1316){:target="_blank"} by [@jasongorman](https://twitter.com/jasongorman){:target="_blank"}

* ["Steve McConnell on #NoEstimates"](http://ronjeffries.com/articles/015-jul/mcconnell/){:target="_blank"} by [@RonJeffries](https://twitter.com/ronjeffries){:target="_blank"}


## Agile Perversion

Many companies moved to Agile, probably because it was the trend or they wanted to be cool or maybe just
because their projects were failing. Whatever the reason, probably, for these ones mainly focus
on process management (Scrum), by now they realise that the projects still failing.

[Sandro Mancuso](https://twitter.com/sandromancuso){:target="_blank"} name it as the "Agile Hangover" in his book
["The Software Craftsman"](http://www.amazon.co.uk/books/dp/0134052501){:target="_blank"}.

Taking a deeper look to XP (eXtremme Progamming) I realised that Scrum is only focus on the red circle.

<figure>
	<a href="/images/xp.jpg"><img src="/images/xp.jpg"></a>
</figure>

I am strongly convinced that Agile and the success of a project depends in majority for the
circles blue, green and red (in such order). But looks like *many* Agile **coaches** only focuses
on the management process and not in the technical process when they are teaching Agile.

Understanding that for a coach has to be difficult to explain to a manager the importance of Pair Programming,
Refactoring, TDD, Continuous Integration, etc. Them should start explaining that to be an Agile company should be a cultural thing.

## Conclusion

Many projects are failing because of bad technical process. Companies should rethink the
methodologies that their projects use or at least validate if the technical process applied, if any, is working well.

Agile born to break the wall between business and development building trust in both ways. So, let's stop to create
numbers to point to the development team.

Projects are measured by value not by estimates, maybe we should think again the estimation stuff.














