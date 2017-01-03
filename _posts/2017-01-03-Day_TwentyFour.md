---
layout: post
title: 8th Light apprenticeship - Day Twenty-four
categories: 8thLight apprenticeship
---

### Pills
- One more week of Elixir!
- Started working on Human vs Computer on my web TicTacToe.

First day of the year today. I had a freezing start, cycling in -1 C to then go
for a swim before going into the office. Neither was a good choice!

Today I had my IPM (iteration planning meeting), and found out that I will be
working with Elixir for another week, which is great news as I really like Elixir.
I definitely will be going back to it in the future, and maybe Erlang too.

I am now adding the option to play against the computer in the web version of the
tictactoe, which is making me scratch my head: as always new features make me
realise how adverse to change my application can be. The problem is that a
chosen move can now come from either the client or the browser, so it is impossible
to have a 'clean' process whereby 'someone' just asks the current player for a move,
regardless of the player type (polymorphysm). Currently my application only gets
moves from the client, but I need to be able to also get the minimax player to
pick a move. I think one way to go about it is maintain polymorphism, asking
whoever is the current player for the next move, but have the human player
return a value that is ignored, so the game is only updated server-side if the
player is MiniMax.

Ciao
