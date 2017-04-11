---
layout: post
title: 8th Light apprenticeship - Day Eighty-nine
categories: 8thLight apprenticeship
---

In any given system, it is good to limit how much each component knows about the
other ones. This increases decoupling, which is generally a good thing.
For example, let's say you have a (very realistic!) farm system, with a dog and a
shepherd. One thing the shepherd might need the dog to do is run around the sheep and bark
at them. Here are three examples of how this might take place from
the shepherd's point of view (pseudocode):

```
1__
  (30).times do:
    dog.move_legs
    dog.open_mouth
    dog.make_noise
    dog.close_mouth

2__
  dog.run_around_sheep
  dog.bark

3__
  dog.get_sheep_back_to_cote
```

All three versions above achieve the same goal of getting the sheep back into their
shelter. However it should be clear that the shepherd's knowledge of how a dog works
is much higher in example two than three, and even higher in number one.
Not only is the last example much easier to read because it is more declarative
than the others, but it will also withstand any change in how a dog operates; for
instance, with the first example, if the dog changes to use a horn rather than its
mouth to make noise, the shepherd has to adapt, whereas the last example only cares
about getting the sheep back trusting that the dog will somehow do it.
Notice that this is not aimed to reduce how much code there is, but how it is
arranged: with the third example the dog still needs to move and bark to push the
sheep, however the specifics of how these actions are performed reside within the
dog, and the shepherd only cares about what it needs from it (tell, don't ask!).

Now, hopefully the above makes sense and it is easy to see how it can be applied
in object-oriented programming; what about functional programming then?

Should we discard any principle of good object-oriented programming in our functional
programming as the two need to stay far away from each other or else they will
explode? Sounds very little scientific; discarding it on these grounds seems bad.

Instead, if decreased decoupling is a good thing, we should give it a chance. In
functional programming there is a strong emphasis on data structures and functions
that operate on it. For instance, in a tictactoe application we have a data
structure that represents the state (this can and should be immutable) of the board
and functions that operate on it, such as `add_move` and `full?`. Now there are
several ways one could represent the board as a data structure: a string, array,
map, a combination of the two: go wild. The question is: should users of the board
__know__ about its underlying representation? It would be better if they could
avoid it; take for instance `add_move`: if a client knew that the board is represented
as a map that associates a position with a mark, they could simply write a mark
directly into the board. This however entangles the board with its clients more
than needed and will force them to change when/if the board changes internally.
Instead, the clients of the board should know as little about it
as possible; with `add_move` there is no need to know _how_ the move is added to the
board as we trust the board to do it for us.

By operating on abstractions (the __what__) rather than concretions (the __how__) we make our systems more
flexible thus easier to change.
