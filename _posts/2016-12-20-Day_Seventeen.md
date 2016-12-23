---
layout: post
title: 8th Light apprenticeship - Day Seventeen
categories: 8thLight apprenticeship
---

### Pills
- Started using Plug.
- Supervisors in Elixir.

Today I have been travelling home (to Italy) for Christmas, so I was working from a 
plane first and then a train, which was challenging as neither had Wi-Fi!
I did some more refactoring on my tictactoe and then started looking into Plug, 
which is a library for lightweight web applications, using cowboy, an HTTP server
for Elixir. You don't have to use Cowboy, but this is currently
recommended by Plug.

Plug is cool. It all revolves around a connection struct. This is an immutable 
data structure that comes to life whenever an incoming HTTP request hits your server.
A Plug module should define two callback functions: one, _init_, is invoked when 
the module is started and whatever it returns is then passed as one of the 
parameters to the other function, _conn_, which is also given a connection.
The most common thing to do is take the incoming connection, do a few 
transformations and finally send a response. This is a side effect, as the function
itself is supposed to return the transformed connection, which should now have
the ':state' field set as ':sent'.
Of course anyone wanting to set up a web app will need some kind of routing, so that
different routes generate different responses. Plug provides a Router module which 
deals with this, allowing you to match a specific incoming http method on a 
specific path. Many other modules are available, including an html rendering engine
similar to Ruby's erb.

I started putting together a fizzbuzz app using core logic I already had. This is 
working sending input from the client as query parameters. I did not have a chance to figure out 
how to redirect as I had no Internet, but hopefully I should have a nice app by 
tomorrow afternoon, with input validation and html forms.

Finally, another cool related thing is Supervisors in Elixir (this is really an
Erlang/OTP thing, which Elixir 'inherits'). Elixir applications normally consist of a number
of processes (see [my blog from last week](http://andreamazzarella.com/8thlight/apprenticeship/2016/12/16/Day_Fifteen.html) 
about processes). If you imagine all these processes running in a tree-like 
structure, with 'parent' and 'children' processes, you have supervisors at the 
roots of the trees. These are responsible for spawning processes and monitor them
so that they can restart them if they crash, and do any other necessary actions 
(logging the crash in some way or the other?). Of course using supervisors is not 
mandatory and one could start processes manually, but this way you would have no
control over any of the processes and would need to restart them manually if they
crashed.

I currently know very little about Supervisors and distributed systems in 
Erlang/Elixir but I would like to read more about them, so maybe I will write an
in-depth post on the topic soon!

Ciao
