---
layout: post
title: 8th Light apprenticeship - Day Forty-eight
categories: 8thLight apprenticeship
---

### Pills
- Getting to grips with Java ServerSockets

I am still working on the Chat Server, which is quite fun, though it has some big
gotchas. By the afternoon today I had a fully working system (server side), unit tested, and
manually used to chat between my computer and Fabien's. I opened a pull
request for my work, but then decided to add a high-level test, to replicate what
I manually did with Fabien: spin up the server, connect with multiple clients
and check that sending a message from one is received by all other users.

Sounded simple enough, especially as I knew this worked already. However, I ran
into a problem that I am yet to crack and I am going to try explain:
the `ServerSocket` has a function, `accept`, which blocks execution, as it waits
for any incoming clients. This is inside an infinite loop, so that as soon as a
new connection is established, the system goes back into the 'listening' mode.
In order to test the system, the `accept` function needs to be in a separate thread to
the client sockets connecting, otherwise the test will hang on the `accept` function.
Now, creating a connection involves creating a remote socket (the ones in the test)
and a server-side representation. However, this process is not synchronous, and it
takes longer on the server side, so that by the time the second socket tries to
connect, the server is not ready for it. The annoying thing is that rather than
rejecting it, the client socket is created, but as soon as it tries to communicate,
the connection drops. I have tried a few things, next one will be to catch the
exception and trying to reconnect.
