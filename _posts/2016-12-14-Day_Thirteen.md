---
layout: post
title: 8th Light apprenticeship - Day Thirteen
categories: 8thLight apprenticeship
---

### Pills 
- Evolutionary design.
- I am really engrossed by what I am doing, but I have to pause! 
- I am going to give my first Zagaku tomorrow.

I am really enjoying what I am doing today. Well, that's the case every day, but
today I am not able to stop at a solid 'checkpoint' (I left a test in red to remind me 
what I am doing when I resume), so I really had to fight myself when I realised
that I needed to stop to write this post. Two thoughts on this: the main 
one is that if possible I should plan my day so as to be able to achieve whatever
I am doing by the end of the day (even if this is part of a story). Of course this
is not always possible, and if it does happen, I just need to be able to let go
and simply resume the next day. This is often actually a good thing, as stepping
away from a problem and sleeping on it can be lead to a much quicker solution.  
 
Evolutionary design. This is a reflection on my tictactoe implementation so far.
In the past, I have definitely been guilty of over-designing my application before
even getting started. This might be fine for small applications, but for larger 
ones, it is just better to start somewhere and then slowly morph the code into a
better design, which will evolve with user requirements. If the initial blob of 
code just works for the requirements and there are never any changes, then that is
probably the best solution: fast and cheap. No time was wasted in worrying about
extensibility and trying to prepare for any change. If there are changes, then a
good developer will look into them, 'prepare the system' to easily make the changes
and update it **when** the changes are required. This way, change drives refactoring, improving the overall design,
making it tailored to the user needs. This is so much better than the utopian 
perfect design, which burns a lot more resources and delays delivery of a 
working system to the user, before the realisation that a perfect design is most
likely impossible. By the same logic, refactoring everything for the sake of it 
can be wasteful; I think that a good rule of thumb would be to prioritise refactoring
areas that are most likely to change, or definitely going to change. By refactoring
I am not referring to what should be done at the end of each red-green-refactor
cycle, where we look back at the code and make it more readable, simpler, etc, 
I am referring to major refactors, which affect a large part of the application.

In building the tictactoe, I started with a blob of badly written code, then 
slowly morphed this into much better shape. At the end of the first week, the code
was looking so much better than the original solution. As I keep adding new 
features, (and thanks to insightful conversations with my mentors) I first make 
sure the code is 'ready' to accept the new change then simply 'slide' the new 
feature in. 

I like this; there is no perfect design for all solutions. There is design that
is perfectly suited to the needs of the users, which delivers value quickly and 
adds value every time it grows.

Tomorrow I am going to do a Zagaku on regular expressions, my favourite thing (I
mean, not quite, but I do like them).

Ciao
