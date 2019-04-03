---
layout: post
title: Monorepo - Just Do It
tags: [thoughts]
comments: true
---

It is been a while since I am working using mono-repository. Advantages and disadvantages between mono-repository and multi-repository have been extensible discussed in the last months, so instead of writing *just another post* about it - I will explain my experience with mono-repositories.

Recently has been a post that attracted my attention [Monorepos: Please don’t!](https://medium.com/@mattklein123/monorepos-please-dont-e9a279be011b). 
I am not going to discuss the points given in the article, but my opinion is that he focuses most of the arguments against mono-repo around it does not work at scale. 

Might be that I did not reach that scale because I did not experience the arguments given in the article, but companies who reached those problems have workarounds or have created products to solve those issues.

### Experience from the trenches

#### At **Sky** 

I joined the video-platform team on the Over-The-Top (OTT) department, where I learned tonnes of things and had my first experience with Pair Programming and [trunk based development](https://trunkbaseddevelopment.com/).

As a **product team** for the video-platform, we used mono-repo. It required some effort to improve the pipeline to allow CI/CD, but it worked very well because finally, we were getting the confidence that the whole platform was working on each feature, on each push, on each environment, and on each release.

Looking back now, a problem that could have been avoided in the OTT department was that another development team were building the **download-platform** which had many similarities with the video-platform but was implemented completely in a different way.

A lot of code could have been shared between both products, but it was not done because the similarities were not visible to us. 
After some time, when we did some pair rotations we discovered how much similar both products were, but how different they were implemented.

Having a mono-repo within an organisation or department would have helped. At least for the OTT platform, to reduce the cost of development, improve the overall quality of both projects and reduce the build time by simplifying the process.

A few weeks back, I was told that they were tackling this problem using a mono-repo for the department. :rocket:

#### At **Gov UK (Universal Credit)**, 

Initially, the project started using multi-repo. When I joined, were around 50+ microservices and it was a nightmare to manage the dependencies, pipelines, delivery and so on.

I was lucky enough that once I joined, the department started the migration from multi-repo to mono-repo naming it *universe* (I love the naming). 
Step by step, we were moving each service/repository into *universe*. To build the mono-repo we used [Buck](https://buckbuild.com) as build tool to allow multi-modules build using a cache.

When I left, *universe* had around 120 microservices in the mono-repo, all the services for UniversalCredit at the government and it was **successful**.

We worked hard improving the pipeline, thanks to the incremental builds and buck caches the build times were reduced considerably because the majority of builds only require a partial build and run tests only for the modules that have changed. 
Usually, builds could take up to 20 minutes having a release candidate as a result.

When using mono-repo looks like a good idea to run all the platform locally, but please do not do that. Initially, it is very tempting but later will require a very powerful laptop. Instead, define well the service boundaries, trust those boundaries and relay on stubs for those services.
 
#### At Zopa

It was funny because in the early days when I started, I was a bit frustrated when a developer told me
> Mono-repo? Oh we are not Google or Facebook

In the organisation, we had 10+ products with hundreds of repositories, a few common libraries and very a little code sharing between products. 
 
As a tech-lead, I had the opportunity to build up a team. I tried to push for values that I believe help software development, so the team was built around Pair Programming and embracing other Extreme Programming practices.

Initially, it was easy to push for mono-repo, although soon I started to have some friction about it. Hence, I did not want to enforce the idea, we split the product into 2 repositories. After all, it was not the end of the world, but indeed, it was the biggest mistake I ever did. 

One component kept as a mono-repo - the team were pairing, sharing knowledge and improving the quality continuously, quickly building up some standards and having some shared components between services to manage infrastructure or testing.

Another component ended up being split into 4 repositories. Developers were not able to re-use much code from other components and it was a nightmare to update dependencies and so on.

All it happened in less than a year so you can guess how it would look like after a few years. 

Developers working in the mono-repo were not feeling comfortable making changes in other repositories, devs were not agreeing with standards, some ego battles between ownership and so on.

Soon it might end up being 10+ repositories, having to manage it is a lot of waste. Within the mono-repo, it was easy to manage tests reports automatically, have dependency management migrated to Gradle 5 easily, not leak transitive dependencies, update third-party libraries, apply security patches, have a pipeline using remote cache and so on. Having to do it for 10+ repositories is a lot of *waste*, boring, hard to apply and to maintain.

A reason to have multiple repositories in the organisation was that all the teams worked with the same simple pipeline. It works but makes hard to parallelise steps, use incremental builds, use remote caches, etc. 

Another reason to use multi-repo was that the tooling for delivery did not support well mono-repos. 

Usually, when the problem is the process then I always push for **change the process** and remove things that do not add any value. We should not adopt bad practices, just because the process sucks.

### Conclusions

To use mono-repo, will require some tooling. As an organisation, it would simplify development, reduce development time and cost. 
For developers, it would help to have more visibility of the codebase, share more code, discover different approaches, make refactoring simpler and reduce the waste of time managing multiples repositories for bumping versions, managing pipelines, dependency management, reports and so on.

If you have any experience with multi-repo vs mono-repo, please share it in the *comments* or let's start a thread in [twitter](https://twitter.com/MuSTa1nE)

