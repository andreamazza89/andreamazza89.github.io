---
layout: post
title: 8th Light apprenticeship - Day Fifty-eight
categories: 8thLight apprenticeship
---

Metaphors can be dangerous: at first they are very useful in helping to explain
something others do not know using a familiar domain, but it is easy to overdo it.
If we cannot let go of the metaphor, we become enslaved to it, and force it
upon the thing we were trying to explain, even though the thing might differ from
the metaphor in many respects.

With the above in mind, I am about to overdo a metaphor (sorry for the momentary lapse of hypocrisy).
Here is a thought I have had from building an HTTP server in Java:

Building a large\* system is like throwing pottery on the wheel.

###### \*(large in relative terms: this is large for me at this time)

You have to start from a ball of mud: you could start thinking about all the
requirements, spend days designing an architecture, then build each individual
part and glue them together, but I have found this approach not to be successful.
Instead, you do spend some time thinking about what the system might be like, but
then quickly move on to the ball of mud with an open mind, as exploring how the
system fits together might lead to new ideas that we simply could not foresee
before we started getting our hands dirty.

You have to work on the whole thing organically, and it is not going to be beautiful
right away: as the system grows and requirements increase in complexity, you might
need to rearrange code, which might affect other parts of the system. I find it
easiest to write a test for the new feature, then make it work as dirtily as
possible, then refactor. The timing in doing this is important: sometimes allowing
the dirty pattern to grow helps me better understand how to refactor it. Do it
too early and you might be going for the wrong abstraction; leave it too late, and
the mess will be so bad and inert that it might be difficult to move it.

You should listen to the clay; sometimes you want to force it one way, but it is
naturally going somewhere else: it is important to _listen_ to the code. As we
write code and put modules together to form a system, we might find code smells,
or it might seem difficult to implement a new feature. This could be the code
talking to us, and we need to listen: perhaps rather than forcing this new feature
into the system, we should amend the system so that it will be easy to add the
feature. Similarly, if it feels hard to add some code because we have to reach
inside an object contained by another object, maybe we should stop and listen to
Demeter screaming at us: _"tell, don't ask!!"_.
