---
layout: post
title: 8th Light apprenticeship - Day Nineteen
categories: 8thLight apprenticeship
---

### Pills
- I have a first working implementation.
- Outside-in can pay dividends.
- I want to learn more about macros.

By the end of today I could update a game stored in the session and show whose
turn it is. The user interface does not stop the game when this is over yet, but
that's next. I first want to make the board more appealing visually (currently a 
row of letters and buttons) and, most importantly, review and refactor what I have
done so far.

I experienced first hand how inside-out development can wast your time:
before I wrote any high-level test, I figured I work on the one thing I was
almost sure I would need, a state 'manager' in the server to keep the game state.
However, as I wrote my first two high-level tests and started putting the thing
together, I realised that there was no need for a separate process tracking the
game state as I can simply store it in the cookie! This is one thing that I like
in outside-in development; it makes you focus on only the things you do need to
make the app work rather than the ones you think you might need. At least in principle.

One final thought: as I was working on building my own plug (Elixir Plug), trying
to understand the underlying workings of these, I went to look into the Plug source
code, but since this makes heavy use of macros, I could not fully grasp what was
happening, so I plan to learn macros in depth.

Ciao
