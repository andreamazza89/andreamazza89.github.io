---
layout: post
title: 8th Light apprenticeship - Day Twenty-three
categories: 8thLight apprenticeship
---

### Pills

- Started learning state machines.
- Got myself over-concerned with system design using a state machine and have now
built a turnstile in a distributed style (I think? Different elements of the
systems run in different processes).
- Not sure how you test a distributed system, so I will research that!

Today I started looking into state machines. This is a really interesting topic,
I am glad it made it into my TODOs.

There are several nuances to the topic, but in general state machines are used to
model systems that have a finite number of states they can be in. A number of
inputs (also finite) trigger a state transition and a number of actions can be
defined based on a which
state you are in (Moore machine), or the combination of state and input (Mealy
machine).

A simple example is a turnstile. A turnstile can either be in `open` or
`locked` state. If the state is `locked` and the `paid` input is received, the
state should move to `open` and the lock mechanism itself should be sent some
kind of `open` message. You get the picture. State machines can make your life
much easier when modelling a system, as they allow you to create these 'scenarios'
based on each state, with different rules depending on the state.

So I started working on the turnstile, creating an initial state, then sending a
specific message for a given state and checking that the state changed accordingly
(for example, starting from `locked`, if I send `paid`, the state should be `open`).
That was fine, and it is quite nice using pattern matching, as you can simply
pattern-match the combinations of state and input to generate the new state.

However, I had the same entity managing state **and** updating it. This is not great,
as I would prefer to test simple pure functions rather than mutable state. So I
turned the module under test to something that simply takes an input+state in
and returns a new state. Nice. Having done this, I started thinking about how
this unit would fit into a whole turnstile system: at the moment I am generating the next
state, but doing nothing else. I need actions, like 'release lock', or 'refund
if already paid and not crossed', etc...These are the side effects we need, and
a good rule of thumb is to push these as close to the boundaries of the system as
possible. Most implementations of state machines I have seen online simply
intersperse these side effects in the finite state machine, however, I really
wanted to isolate these from the logic that processes inputs/states. Also, having
just watched an amazing video describing a design approach called ['functional core,
imperative shell'](https://www.destroyallsoftware.com/talks/boundaries),
I wanted to try fit some of those ideas in my program.

So here is my thinking: I already have a module that takes state+input and
produces a new state. This is my functional core. Good old pure functions.
Now, I imagine that in a system like this I want the following to be separate entities:
- Something that keeps track of the mutable state.
- Something that deals with payments and sends the turnstile system a `paid`
message once the payment is made. It also accepts a `refund` message if necessary.
- Something that operates the locking mechanism. This will receive `open` and
`close`.
- Something that senses a person passing through the turnstile, which will send
a `push` message to the system.
- The system itself, to manage messages between all these parties.

Now, I thought it would be pretty cool if I could get a working skeleton that
uses the structure above and leverages Erlang's processes. What is really nice
about it is that each of the processes is independent, so that if for any reason
one of them went down, it would not crash the whole of the application, and can
be brought back to life by a Supervisor. What is even nicer, is that with this
modular approach, you could hot-swap any of the parts, as the only connection between
them are simple messages(these are just values), so that you could one day use a
more sophisticated locking system, or even use one written in a different
language (you would have to write an adapter to forward messages, but in theory
this is easy). So I started working on this, and after a while I had a working
thing!! I can load it up in the REPL, spawn the various processes (this could be
made automatic and supervised but it is not at the moment) and send messages to
the turnstile to see it open the lock, ask the payment provider to refund, etc...

There is an elephant in the room. I have done this without testing! That is
because I wanted to validate my idea before I invested any more time in it,
however I plan to research how to test a distributed system. I wonder if I
should 'slice' each individual process and assert that a certain message is sent
under certain conditions, although this is still one step away from a full
acceptance test. I look forward to finding out more!

Ciao
