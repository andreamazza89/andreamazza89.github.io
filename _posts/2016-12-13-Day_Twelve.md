---
layout: post
title: 8th Light apprenticeship - Day Twelve
categories: 8thLight apprenticeship
---

### Pills
- When meeting with stakeholders to plan the next iteration, I should take my time
to think through a story and ask for as much information as possible.
- The MiniMax algorithm for the computer player took longer than I expected.

I had my IPM (iteration planning meeting) with my mentors this morning. I really
liked discussing various aspects of my latest tictactoe implementation: they 
both gave me ideas on how to improve the design and I am looking forward to rearrange
the code in a (hopefully) better way.
One thing I should get better at is taking the necessary time to thoroughly 
think about a story before estimating it: for example, amongst other things, I was
asked to integrate the unbeatable player with the rest of the game. I did not give
this too much though and simply responded with my estimate. Later on, as soon as 
I got started with the task, I realised I was missing some specific information 
that I could have asked my mentors had I thought a bit harder about the story.
This was not a problem today as I simply asked the question, but had that been a 
client, I would not have made a good impression. Food for thought.

Today I spent much longer than I though I would working on the MiniMax algorithm
for the computer player. This is for two reasons. One, I had a silly mistake that 
gave false positives and took SO long to spot. Two, I then tried to simplify the 
algorithm so that it would only use the board and the player rather than game board
and player, only to realise that the algorithm needs to know whose turn it is, which
is the Game's responsibility :(
For number one, I think the biggest takeaway is that I should have built the 
algorithm incrementally, like one of my mentors suggested: you could slowly build
it up starting with a test for the base case, then have a scenario that is one 
step away from base case and so on.
For the latter, I think I should give a refactor more consideration before starting,
especially when it affects a number of modules and functions. Had I done this, I 
might have realised that it was not worth the effort overall. However, I guess it
really depends as sometimes it is easier to do the refactor and look at any ripple
effects, then assess. I think the key is time-boxing.

Ciao
