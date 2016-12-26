---
layout: post
title: 8th Light apprenticeship - Day Twenty
categories: 8thLight apprenticeship
---

### Pills
- Mini-retro on feedback received.

I now have a first working implementation of my web tictactoe! I was super-lucky
to receive feedback on my latest pull request from three different crafters,
which is amazing, as I think this is one of the best ways to learn and improve.
I am so thankful for their time, and after working/thinking about the various
suggestions, I have decided to write down a mini-retrospective of what I have
learned.

In no particular order:

##### Commit history, advanced git
My commit history had a few inconsistencies, with a couple of 'WIP' commits. It
was pointed out that I should probably squash these into previous commits for a
more consistent history before I push to the server. Trying to rewrite history
having already pushed onto the server threw me down a rabbit hole, which made me
promise to myself that I would spend some time in the future learning more advanced
git features! In general, I should be more granular with my commits, trying to
'tell a story' that is easy to follow.

##### Do not leave tiny loose ends
There were two little things I could have improved on but thought I would leave
for the future: reorganising test helpers into separate files and moving the css
styling from the head of the html into a separate file. I was keen to create the
pull request and get feedback on what I had done, but in retrospective it is a
shame to leave something this trivial unfinished.

##### Strive for code simplicity and no repetition
This is something I think I am reasonably good at, however some of the feedback
made me realise I can always strive for more, in terms of readability, simplicity
and no repetition. I think something I should do is take a break away from the code
every now and then and have some fresh eyes to look over it again, trying to put
myself into someone else's shoes and trying really hard to make my intent as easy
to grasp as possible. The same goes for refactoring repeated knowledge: I should
always suspect that something is repeated and could be made smaller and re-useable.
In this instance, I had web helpers that looked like this:

_create a web connection, then do a, then do b_
_do c, then do b_
_create a web connection, then do a, then do c_
...

What I failed to see, is that I could simply make the functions do only one thing
(e.g. _create a connection_, _do a_, etc)and then mix them together to create all
the combinations required.

##### Presenter behaviour
I realised that my understanding of a presenter was fuzzy, but hopefully I now
have a better grasp: a very common pattern is for the View to display data that
is within the model. For example, say you want to display a list of all users,
your view is now accessing model data (users) and showing it on a web page. Well,
this data, say first and last name, might need some manipulation before it is put
on the page. For instance, you might want to capitalise both first and last name,
or shorten the first name, etc... Rather than having this logic scattered across
the many views that display users, a presenter is introduced to abstract this
behaviour and locate it into one single source of truth. Amongst other things,
this relieves the business logic from presentation concerns and allows future
changes to view logic to be easily made from one place only rather than all over the
specific views.

##### Never ever be lazy
I had a small "tell don't ask" violation in my code. This is not a huge problem
in itself; what is bad is the fact that I was aware of it, yet as it was slightly
easier to leave the violation (no need for additional tests), I did not do
anything about it. It was so easy to move the behaviour where it belonged that I
really had no excuse to leave the violation there (self-slap!).

Ciao
