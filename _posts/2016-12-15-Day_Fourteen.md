---
layout: post
title: 8th Light apprenticeship - Day Fourteen
categories: 8thLight apprenticeship
---

### Pills
- Short post today!

This morning I run a Zagaku on regular expressions. I hope it was somehow 
useful to the people listening; I enjoyed doing this, I like the idea of everyone
sharing what they know and being invested in other people's learning. 

I then paired with EA for the rest of the morning, which was fun. She has just 
started, and like me she is starting her apprenticeship with Elixir first, so I gave her an overview of 
my current tictactoe design, which led to discussing several nuances of Elixir,
including processes, which I really enjoyed. It also got me to strengthen my 
understanding of processes, as I was demonstrating the concept of message passing 
between them and made a tiny echo process in the REPL. I am always worried that I
talk too much and do not leave enough space to breathe. It's just that I am too 
excited about these things! I am going to try and keep it on a slightly less 
verbose note, but I guess enthusiasm is not a bad thing.

Finally, I spent the rest of the day working on a repeat functionality for my 
tictactoe and then started refactoring the human player so that the input parsing
is delegated to the user interface. I wish I had done this earlier as it is now 
a bit painful, but that's OK and I have learned a lesson about keeping concerns 
together (I already had a user_interface module, but still decided to handle input
parsing for a new user move from a different module).

Ciao.
