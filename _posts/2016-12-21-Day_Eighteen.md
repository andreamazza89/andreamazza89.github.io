---
layout: post
title: 8th Light apprenticeship - Day Eighteen
categories: 8thLight apprenticeship
---

### Pills
- To test or not to test. Or rather; definitely test, but what?!?

Today I finished playing with fizzbuzz using Plug and started working on building
a web version of my Elixir tictactoe. First I had a think about how a game would 
play, as this time the running of the game is to be driven by individual events
rather than a loop. Because of this, I need to actually introduce something that
holds onto the game state on the server side.
One way to think about the application is that everything revolves around this
game state. Everything the Client and Server do is around updating this state and 
displaying it to the user. I also played around with Plug to figure out if I could 
store the process ID of the state in a session, which I can, so multiple clients
can have multiple games, but this is probably something I should worry about later.
So the first thing I did was to define unit tests for the game 'state manager' and
implemented it. 

The next step, thinking about how to kick-start development of the app architecture,
was much more painful and is still under process...There is no one-right-way to
do this and I always find it difficult to find a good test that is meaningful and 
drives my development. I started thinking I should test my controllers, however I 
now feel this might be unnecessary, since if the controllers are left to only implement their only
responsibility (be the bridge between an incoming request and the response sent),
then they are just 'orchestrating' other modules, and so could simply be covered
by integration tests. One thing I like to do is list the 'knowns' in the project
as this gives me some confidence and makes it easier to be pragmatic in thinking
about what to test. I think that though I could start working on the modules that the
controller uses to come back with an appropriate response to an incoming request 
(inside-out), this time I would like to try an outside-in approach, where I define
an integration test that touches the whole of the application; this would then 
guide me in creating the individual building blocks that make the feature possible.
I will see how that goes. I think one of the reasons outside-in is more difficult
than inside-out is that it requires more forethought and abstraction than an individual
unit test does. This is normal as you are describing the system as a whole rather than
an individual part of it. Of course you do not have to know how the whole thing 
fits together, in fact, the test should not be about _how_ it fits together, but
rather _what_ it does as a whole. I feel experience can help with this, but you
have to start from somewhere! I also like the fractal nature of an integration
test: you define what a system does; this does not work as you have not written the
system yet. The system you defined is made up of several subsystems, so
in order to build these you write tests that describe what each subsystem does, 
only to realise that (perhaps) the subsystem also has a few sub-sub-systems...
Until you reach the end of the tunnel, to finally ripple back and have a working 
feature! My head is spinning. So, maybe integration and unit test are two different
flavours of the same thing: a description of a black box that does a thing. From the point of 
view of the observer it does not matter what is inside, as long as you get what 
you need.

Ciao
