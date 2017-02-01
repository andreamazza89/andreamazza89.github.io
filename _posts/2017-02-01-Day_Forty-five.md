---
layout: post
title: 8th Light apprenticeship - Day Forty-five
categories: 8thLight apprenticeship
---

### Pills
- Playing with Java sockets.

Today it was a spiking day. I have been learning about Java sockets and concurrency.
I first made an echo server, then moved this into a separate thread so I could
test it. At this stage I could start the server, connect to it using NetCat as a
client, type text and see it sent back. Afterwards, I added the capability to accept
multiple connections and forward any messages from any client to all clients,
effectively creating a system where any user gets messages from all other users.
I also had a test for this: start the server, create two sockets connected to it,
send a message from one of them and assert the message is received on the other
one. This is an integration rather than unit test, but getting it to work helped
me get my head around threads, sockets and the system in general, so tomorrow I
shall throw away all the code I wrote and start over, with more modular code and
unit tests.

In an attempt to consolidate my mental model of what I learned, I am going to
describe how the system works.

At the very core of the chat server are streams; input streams and output streams.
Any client writes messages onto an output stream and receives messages in an
input stream. This is a simple two-way communication: one side to send, one to
receive. If you have two nodes connected to each other, then that is a closed
system where they can only ever speak between the two of them. However, if each
node connects to a central server (if these were telephones, then that would be the
exchange), different configurations can be created, including one where everyone
communicates to everyone else (broadcast), or subgroups (multicast, or chat rooms).

Ok, so I am imagining streams as the lines that join each node in this communication
system. Now, for the system to be of any use, we must be able to 'extend' these
streams beyond the confines of one machine, or else we will just be talking to
ourselves! One way to do this is to use a networking protocol called Transmission
Control Protocol (TCP), which is ubiquitous and at a lower level than HTTP is; in
fact, HTTP makes use of TCP. Anyway, Java has a utility for TCP communications
called Socket. You can create a socket, connect it to another (remote) socket and
start communicating via its input and output streams. Pretty neat; Java takes care
of all the low level details of the protocol, leaving us with a simple API.
With Java sockets, a client socket needs to connect to a server one, which can
be created and set to listen for incoming connections on a certain port. So far
so good; we create a `ServerSocket`, set it to a port and start listening for an
incoming connection. Notice that, similar to reading input from the command line,
this is a blocking function, that is, the program will pause execution until a
connection is made. Once a client connects, a socket is created, so the server can
communicate with it. But once a connection is made, how do we start listening
for another client while dealing with the one already connected? Sounds like we
are doing more than one thing at one time, sounds like...Concurrency!

This is where multi threading comes into play. In short, we need to listen for
a connection, and as soon as client connects, we hand the client socket to a
separate process and start listening again. Pretty cool right?
Java has a utility to manage multiple processes rather than do it manually, called
`ThreadPool`; you create a `ThreadPool`, then `submit` Runnable or Callable objects
to it, which need to implement `run`, invoked by `ThreadPool` to start them.

Nice.
