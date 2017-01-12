---
layout: post
title: 8th Light apprenticeship - Day Thirty-one
categories: 8thLight apprenticeship
---

### Pills
- I want to move faster through micro-tasks in my development cycle.
- Different tests at different levels for different purposes.

Today I did some pairing over my ContactManager application with Mateu, which was
very useful. One thing pairing makes me realise is that I need to worry less about
my code and just get on with what I am doing. This is because pairing makes you
accountable at all times, so you cannot allow yourself to get stuck in a thought:
you must keep moving forwards fearlessly. What do I mean by getting stuck in a
thought? Some times I find myself thinking for too long about what should come next
and worrying about the design of my application. This is not
necessarily a bad thing: I should worry about design and what my next step should
be. However, I should find ways to go about these two activities faster. For
instance, my current application is so small that I should not overly worry about
design and just write down a working implementation, then analyse it, as opposed
to what I sometimes do, which is think through all the ramifications that the
design might or might not take in the future. This is a change in perspective: rather than
trying to finalise the design before I write the program, I should have a vague
idea of what the design might look like and validate it by writing software *and
then* reason about concrete code rather than what it might look like in my head.

Another interesting thought from the pairing I had is to do with high-level
tests: after writing unit tests for a class that lets you store and retrieve
contacts, we went ahead to write integration tests. I like thinking of any test as a
different version of the same thing: there is a system under test, and a series
of things we expect from this system; this system might or might not use other
sub-systems to provide the service, but this should not matter from a testing perspective.
The above suggests a fractal nature of tests: starting from the outermost point
of view of a system (often human interaction), we define an action the user might
take and an expected result. We then move one step into the system and define tests
for the subsystems that allow the expected user functionality; these subsystems
might have subsystems of their own, and so on, until there are no more subsystems
and we are at the unit level. Notice how at any step, from a testing perspective,
we look at the system under test as a black box: it does not matter what is inside it,
we just care about the required
functionality. However, although all these tests seem similar, they have different
implications. The higher the level of the test, the more expensive it is, and the
more combinations to test will exist, as several subsystems are involved. So the
attitude towards a high-level test should be different than to a low-level one. For
instance, whereas in a unit test you should try to cover all cases to guarantee
the correct working of the module you are dealing with, in a high-level test you
would not be exhaustive and simply use it to drive development of the whole
application and show how this can be used.

Ciao.
