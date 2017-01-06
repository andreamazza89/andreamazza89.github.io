---
layout: post
title: 8th Light apprenticeship - Day Twenty-six
categories: 8thLight apprenticeship
---

### Pills
- Moved the game state in my tictactoe from being stored in the session to being
encoded in the request/response cycle.
- Wondering if my higher-level tests cover enough functionality.

This morning started with a Zagaku by Enrique, who live-programmed some of the
Gilded Rose kata. It was pretty nice to see him work and his thinking behind the
various refactorings.

I then spent the rest of the day altering my tictactoe as follows: in the first
version, the server would use a session to store the latest state of the game.
On every request, it would retrieve that state (or create a new one if the client
is visiting for the first time), alter it if necessary, and then use it to render
a view of the latest state. My mentors asked me to remove the session and encode
the game state in the request/response cycle, effectively making the application
stateless. This is pretty cool, as it means that the client is using the server
very functionally: you could have many instances of the application running on
different machines and the client would be able to run a game unaware of which of
the many servers it is interacting with, as each transaction is self-sufficient.
So this is a big plus already, however it does have security repercussions, as
the state can now be altered by the client, who can effectively cheat. I think
the lesson to be learned here is that having stateless applications (or at least
part of them) is preferable *if possible*.
It was nice to see that moving the state management away from the session was not
too difficult, as this responsibility was isolated into its own module, so I
simply had to 're-route' it. The most challenging part was serialising the data and
then decoding it. I used a JSON library to do this, but unfortunately it did not
work out of the box for me, as it is unable to decode a struct back to its
original state, so had to add some code of my own to make it behave.

I also paired over this task with another apprentice, EA, which was good, as
having a pair of fresh eyes look over your code and having to justify your
decision is a great exercise. In particular we focused on how the views should
be tested in the integration tests. I am fairly certain that these tests should
not be exhaustive and only show major features, as they are expensive (slow to
run and fragile as they have so many dependencies, including the front end). I
wonder however how much should be tested, and is it ok to leave some of the
functionality untested? Writing any code without a failing test makes me feel a
little ill! For example, I have a 'play again' button in the application, which
simply brings you back to the initial page, where you choose a game to play.
This link is currently untested, as it is literally a hyperlink to another page,
so I am not going to test that! Although I did break it as I changed the naming
on the end points and only realised it was broken with a manual test. I still
feel this should be left untested though... Lots to learn and there never is one
right way to do things. I like it.

Ciao.
