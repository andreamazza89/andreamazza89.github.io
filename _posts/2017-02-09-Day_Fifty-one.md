---
layout: post
title: 8th Light apprenticeship - Day Fifty-one
categories: 8thLight apprenticeship
---

### Pills
- Data comes first when designing a program.

Today I had a very interesting conversation with one of the Crafters while pairing
over my chat application in java.

When thinking about how to arrange code in an object oriented fashion, the easiest
thing to do for a beginner is to look for an analogy with real world objects and
make these into classes. This can be helpful and has its value: it is much easier
to relate to real-world concepts and understand how they fit together. For example,
if I was building a cooking app, it would be much less surprising to see classes
like 'Chef', 'Ingredient', 'Oven' than 'Actor', 'Component' and 'Processor'.
This is however very easy to overdo: metaphors are an aid to help understanding
a concept that relates to the real one but can be fundamentally different, and trying
to enforce metaphors too much can make us go in the opposite direction of simplicity.

There needs to be a shift in focus: using a consistent and appropriate language
is very important, but it is not why we write a program. We do not write a program
just so we can create this imaginary world where all these objects resembling
reality live in harmony; we create programs to solve problems, to deliver value
to somebody, and the value delivered to them is (in some form or another) data.
If the above is true, then our design focus should be on data operations. With this in
mind, we can then worry about bundling data and the behaviour around it in objects,
whose name is of course of paramount importance, as is the metaphor (if possible),
though it should not be the drive in our design.

Another side effect of having primary focus on the metaphor is that it can be in
direct contrast with one of the rules of simple design:

_Minimize the number of classes, methods and other moving parts._

If we get carried away with the real-world analogy, we might find ourselves adding
more classes just so the analogy is more fitting. I was guilty of this myself in
the chat application, as I introduced a 'Concierge' class to greet the user and
register them into a room. The cost of having a metaphor that better resembles
a real-life scenario was to have code more scattered around, thus harder to understand
overall.
