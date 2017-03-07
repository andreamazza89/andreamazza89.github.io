---
layout: post
title: 8th Light apprenticeship - Day Sixty-eight
categories: 8thLight apprenticeship
---

Today was my last day working on the HTTP server (sigh). I have spent the last
two and a half weeks working on it and it has been a lot of fun and learning.

I will try to summarise what I learned from it below, in mini-sections.

#### Getting faster at finding information about an unknown topic.
Though I have been aware of and using the HTTP protocol for a while now, this
exercise made me realise how much more there is to it. (By the way, this page is
delivered to you using HTTP).

Having to implement a server that conforms to the standard involved learning about
new topics and applying them. Whenever I hit an unknown, it is very important that
I try to first formally establish what information I am after in my head, then
go after it. This gives me a 'compass' in my research, that is, a way to stay on
track and know when I am done. During the research, there is a fine balance between
becoming a master of the topic, and not really understand it all. The sweet spot
is somewhere between the two ends. You want to understand the 'thing', but learning
too much about it slows the entire project down and is wasting your client's money.

#### Getting faster at debugging.
This is related to the previous point. Sometimes I would find a bug and dive
straight into the first idea I had for solving it. This is not good, as I should
instead

- stop
- analyse and describe the problem
- come up with hypotheses of how to resolve it
- sort the hypotheses in order of likelihood
- start working on a solution

This is counter intuitive: when a problem arises, we are annoyed, and perhaps
ashamed. We have to cover it up as soon as possible. We start with whatever plausible
cause comes to our mind. We add a few print statement, change some code, wait for
compilation, try again, realise we forgot to change some other code, compile again,
start panicking...aaaaahh! I shall make an active effort to always follow the analytical
steps above and formalise the debugging process; this way I will have a 'compass'
to guide me in finding the bug.

#### The middleware architecture
Once I had the server working, I was asked by my mentors to refactor it using the
middleware pattern. This is what the Ruby Rack gem is built upon.
I really like how simple and elegant it is; the patten describes how some kind of
input is processed by a system in a pipeline fashion before an 'answer' is returned.
This works particularly well for the HTTP request/response cycle.
A request is received by the system and is then processed by a number of middleware
layers. Each layer needs to conform to the same simple interface: it should have
a function, let's call it `generateResponseFor` that takes the request as input
and returns a response. Any given request is given to each layer until a Response
is ready to be dispatched. Any given layer does not know where it is in the stack,
and new layers can be 'slotted' in place very easily.

#### Evolutionary design
I used to spend way too long trying to design a system before I would build it.
This meant a lot assumptions and premature optimisations were made before I even had a chance
to see that the assumptions were wrong. It also made it much harder to make design
changes as it is difficult to let go of those ideas formed during that long design session.
I am now designing very little at the start of a project and allow myself to
figure out the design as I code, based on the latest requirements. I feel like
I am possibly overdoing it now, but feel it is best to air on the side of little
upfront design than too much.
