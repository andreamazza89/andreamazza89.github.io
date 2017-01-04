---
layout: post
title: 8th Light apprenticeship - Day Twenty-five
categories: 8thLight apprenticeship
---

### Pills
- Implemented Human vs Computer in my web tictactoe.
- HTML forms made me suffer a bit.

Today was a good day. I integrated the minimax computer player from my core
tictactoe logic with the rest of the web application. One thing I would like to
improve upon is my process in adding a new feature to an application. In
particular, there is an initial phase in which I start considering how I could
implement the feature and the implications this will have on the rest of the
system. My head spins as I try to go through all the details and repercussions
of the change. This is not optimal as lots of time is spent deliberating vague theories
in my head.
I think this could be improved as follows: first, diagram the system as it
is now. Then consider how to add the new part, or modify the existing one. This
might help bring to light any problem with the design and make it clear what
needs refactoring before the new feature can be added. Then have a test to drive
development of the new feature.
I think diagramming is a very powerful tool for thinking about design but also
to consolidate learning, so I should do this more!

As I was nearly finished with the feature, I spent what felt like too long on an HTML
form. The form is in the initial game page, where the user can select a game mode.
The form posts the selection to the server, which initiates the game and
redirects the user to the 'playing' page. Initially, I assumed that the query
parameters would be included in the query string, as is the case for a GET
request, however this is not the case for POST, where the default browser behaviour
is to include the parameters in the body of the request. This made it less
straightforward to implement the logic that deals with the form but it did also
mean that I learnt something, which is great!
In general, I would like to go the next level with my knowledge of the HTTP
protocol, but I guess this will come with the much dreaded Java server, which I
will have to write as part of the apprenticeship.

Ciao
