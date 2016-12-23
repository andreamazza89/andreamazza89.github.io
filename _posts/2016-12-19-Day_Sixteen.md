---
layout: post
title: 8th Light apprenticeship - Day Sixteen
categories: 8thLight apprenticeship
---

### Pills
- My tictactoe now supports a 4x4 board (don't use MiniMax though!)
- Web tictactoe is next.

Today I have been working on allowing a 4x4 board to be used in my tictactoe 
written in Elixir. It was fun, and I was happy to see there were not too many places
in my code that required the board to be 3x3, so that after an hour or so I 
could play a 4x4 game, and after another two this was patched into the overall game, so
that the initial setup menu lets you select a board size to play with. Nice.
Just to stress a point already made 
in [this post](http://andreamazzarella.com/8thlight/apprenticeship/2016/12/14/Day_Thirteen.html), 
I like how my design has been evolutionary, driven by the needs of the client 
(my mentors). In this instance, I had a few places that made assumptions about the 
board size. Had I worried about this before knowing the requirements I could have
wasted a lot of time: what if the requirement for a larger board never came? Or 
what if rather than a larger board a differently shaped one came to be needed?
This way, I only changed the design because a requirement was made. No premature optimisation.
An interesting thing to notice is now that I allow a larger board to be used, 
the computer player using the minimax can take (almost) forever to figure out the
next move. The current minimax goes through every single possible game state to 
evaluate what the most optimal move is. The problem is, with a 16x16 board, at the
start of the game there are 16! (20,922,789,890,000) different ways the game could 
pan out. Now, I have measured my minimax on my laptop and it takes about 8ms to 
work out the best move when it has 5 to choose from. This means for the full 16 it
would take 8ms * 6 * 7 ... * 16, which is just over 44 years! I don't have that long.
I wonder if alpha-beta pruning can make this bearable, but I suspect a different 
approach might be required.

Anyway, I might not have to worry about it at this stage as next I am going to
try and reuse the core logic of my tictactoe to have one that works online...Exciting!

Ciao
