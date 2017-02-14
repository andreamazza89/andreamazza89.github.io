---
layout: post
title: 8th Light apprenticeship - Day Fifty-three
categories: 8thLight apprenticeship
---

### Pills
- Legacy code retreat.

Yesteday (Sunday) I was at the office all day to attend a legacy code retreat,
which was organised by Enrique, Georgina and Daisy. Big thanks to them as it was
a super-fun day and very instructional. We worked in 45 minutes slots, where we paired
up (changing pair every slot) and worked on a specific technique that was
introduced and discussed at the beginning of the slot. The exercises were all on
a small codebase that was written for running a code retreat! Here is
[a link](https://github.com/jbrains/trivia) to it.

There is something about refactoring code that I find very appealing, and –though
I am not a gardener myself– could be resembled to gardening; you start with a
garden that has weeds in it and has grown past its initial shape. With a sequence
of small alterations, you bring the garden back to its original glory. OK, maybe
the metaphor is not a perfect fit, as with code you do not simply go back to the
initial form (we have version control for that!) but instead make sure the newly
introduced elements sit within the whole system in harmony. I guess the point is
that we care for the thing we are changing (garden or codebase) and, via small
incremental steps, transform it into a better shape.

Aside from the specific techniques that we deliberately practised, I liked that at
the end it was pointed out how to go about refactoring legacy code: you do not
take the whole application and work on every corner of it until it is spotless.
This is not pragmatic: if any of the system does not need change and currently works,
why touch it?? You might say 'so that when it comes to it, it will be easier to
change'. Well, I agree, but wait until the requirement to change it arrives, as
it might never do, or it might be different from what you expect today. This is
similar to Uncle Bob's boyscout rule: we always try to leave the code we deal with
in a better state than we found it. We do not go opening all files to try refactor
for the sake of it; it is just the areas we are concerned with that we act on.
