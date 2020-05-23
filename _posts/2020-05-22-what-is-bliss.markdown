---
layout: post
title:  "What is bliss?"
date:   2020-05-22 14:36:00 -0500
categories: general
author: Brian M. Hofmeister
---

I have subtitled this blog "A Developer's journey toward bliss." What is bliss for a developer?

Certainly we developers should all like to think we could write code without defects. It might be blissful to write code effortlessly and without thought or planning. It would seem enchanting to have our code work with code from others without flaw and without painful integration errors. We might wish to forego any efforts at testing our code to remain blissful. Code would never leak memory, throw exceptions in production, experience network disruptions, nor accept invalid inputs leading to invalid outputs. Bliss, right?

Reality must settle upon us now. We're human. Fallable. Perfection is not a realistic goal. So what does a realistic picture of "bliss" look like? I think, to be completely realistic, we have to consider our chosen environment. Bliss in a C code base will appear very different from a blissful Java code base. Functional environments like Haskell or Erlang would perhaps have their own blissful reality. Even domain specific languages like SQL prove to have examples of blissful versus not-so-blissful code. If it requires such unique effort to achieve bliss in our code, how can we hope to ever do so, and to do so consistently?

I think there are some common traits that make a code base more blissful. I think these traits are independent of language, environment, platform, or domain. I think, far too often, we developers focus far too much on making code "work" and not enough on making code "workable." I mean that our code ends up becoming a mess to maintain because we are forced to get something to work, something to ship, something to deliver. Workable code would allow us to deliver sooner, with better quality, if only we have the room to make our code workable -- in many cases, I would settle for "more" workable code in hopes of iterating toward a better state over time.

That was a mouthful. What about those traits I mentioned?  These are drawn from many sources. I'll highlight some of the best sources a bit later.

1. Appropriate modularity.
2. Small functions, minimize imperative logic.
3. Isolate external dependencies, minimize contract surfaces.
4. Automate all the tests.

### Appropriate Modularity

Modularlity is hardly new, it should not be controversial, and it is possible in some form in pretty much every programming environment. What do I mean by "appropriate" modularity? In the simplest terms possible, I believe a module should provide a minimal amount of functionality with obviouis purpose. The [Single Responsiblity Principle (SRP)](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html) says that a module should have only one reason to change. As indicated in Uncle Bob's piece, modules are best separated by functional concern, which again leads us to higher levels of cohesion. We can loosen coupling in various ways:

1. Use simple data structures (maps, lists) as interface parameters rather than complex types
2. Apply the [dependency inversion principle](https://deviq.com/dependency-inversion-principle/)

### Small functions, minimize imperative logic

I know. The notion that code functions should be small is so often stated it should seem obvious. I have found, however, that so many projects I have worked on flatly ignore this idea. I've worked with code in many languages (C, C++, RPG, Java) and seeing functions with 100 or more lines was not uncommon. What's the big deal, you might ask. So what if the code in one function here and there gets kind of long. Most of the functions are short.

The dilemma with long functions is a matter of what we expect from readers of the code. I believe using a debugger to understand what a piece of code is doing should be a last resort. The intent of the code should be as obvious as possible. Shorter functions lend themselves to being much more obvious. Secondarily, a longer function has a higher liklihood of needing to change for more than one reaason, which violates the SRP.

Imperative logic creates a heavy mind-burden on the reader of code. Longer functions will no doubt contain a lot of imperative code. Declarative functions tend to be named with more intent, their meaning becomes more obvious. The ["tell, don't ask"](https://martinfowler.com/bliki/TellDontAsk.html) principle provides one manner in which to produce more declarative function interfaces. It's not a silver bullet, but it helps.

_Note:_ When I talk about imperative vs. declarative here, I am not implying one programming paradigm has advantages over another. Declarative functions can be used in any paradigm, and in my opinion lead to far more readable code when used effectively.


### Isolate external dependencies, minimize contract surfaces

A major cause for frustration and code health issues occurs when external project dependencies become so tightly woven into the fabric of an application that you just cannot break the connections between them.  This kind of abuse leads to two inevitable problems:

1. Dependency version lock - you simply cannot update the version of your dependency without possibly breaking something, somewhere
2. Testing impossibility - you shouldn't mock what you do not own, and if you've sprinkled something you do not own throughout your code base, testing becomes all but impossible

There are some code libraries out there (Java especially) that are defacto standards for their purpose. They're so widely used that it is pretty much impossible to find an alternative. I can think of two such libraries for sure. Both of them are nearly impossible to integrate with cleanly, and most uses of them that I have seen end up peppering the dependency among many classes, in different packages, throughout a project. That sucks. Now if I wanted to write unit/integration tests for my core logic I can't, because I cannot isolate my code from my dependencies in these situations.

When evaluating an external dependency it is *critical* in my opinion to use various techniques (inversion of control, facade) to guarantee that dependency is closely held by a single module of a project, and that module wraps and fronts the dependency with interfaces which are under my control. I can then mock my interfaces to test my interactions behave as expected within my code, and I should not need to worry about testing the dependency itself. If the dependency changes in a way that breaks my uses of it, or if I want to add some new use case requiring that dependency, my changes are isolated to one module, and are far simpler to test and integrate.

Through isolation of the type described, the surface area of the contract between a project and its dependency can also be managed, and minimized. The integration module only provides the functions necessary to interact with the depedent library to satisfy known use cases, nothing more, nothing less. This restricts the possibility of abusing the use of a dependent library and may, in some cases, reveal that the dependency is simply not needed if a simple implementation is possible.  Careful management of this contract surface also means that when new use cases require interaction with the dependency, they can be added on-demand.

### Automate all the tests

Unit, integration, functional, system, user acceptance -- all different kinds of tests, all of which should be automated. Before we get to why tests should be automated let us first talk about why we test. This is another conversation that perhaps should be obvious, but I fear often is not in some contexts.

Why test? What goals do we achieve by testing our code? The answer I hear most often is "to find and eliminate bugs before customers hit them."

Okay. What is a bug? I know it's a snarky question. Is slower than usual performance a bug? Is a failure caused by a broken network connection a bug? Is a faulty sensor reading a bug? I think the obvious answer to each of these questions is a resounding "it depends." That's a developer classic, right?

The problem with defining a goal of testing as "eliinating bugs" is that you cannot eliminate bugs. Bugs happen. Always. So why test?

Testing is about managing risk. In some environments the risks associated with new software releases can result in loss of life. Certainly, bugs in such cases can be tragically bad. Some bugs, however, even in such critical environments, are less risky, and fixing them may actually cause more harm. So we cannot view testing only through the lens of eliminating defects, we must view it in terms of evaluating the risks of releasing software.

With that settled, why do we need to automate tests?

Code usually grows over time. With no testing, certainly you increase the risk of releasing more code. With manual testing, which seems to still be prevalent in places, there are limits to the understanding of risk that you can achieve. Confidence increases as you can increase your understanding of the risk associated with releasing code at a given point in time. 

With modern continuous delivery methods and cloud based services, the need for confidence is even greater. In a cloud service, when a bug is found in production, the mean time to repair is greatly reduced through automated testing. Reduced time to repair defects, over time, should increase mean time between failures, which increases availability and in such cases likely improves revenue outlook as well. When a failure does occur, adding a test for that failure helps to ensure the issue stays resolved, further improving average quality over time.

Without automated testing, it is nearly impossible to evaluate the metrics above, such as mean time between failure or mean time to repair. There's a saying, you can only manage what you measure, and in software, risk to release is perhaps the most important measurement required.

## Summary

As a developer, I would rather write code than fix code. I have spent most of my career fixing code, however, and the lessons I've learned about code quality have been reinforced time and again. Code quality is more than just working code, and therefore quality cannot explicitly be "tested" into code. Testing is a tool for risk management, and at the unit and integration levels testing also helps us prove our designs are robust in the face of change. Quality is measurable in various ways, and without question the manner in which we approach the issues we face has a measureable impact on the level of our bliss.