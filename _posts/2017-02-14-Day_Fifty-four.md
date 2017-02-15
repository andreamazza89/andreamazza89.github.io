---
layout: post
title: 8th Light apprenticeship - Day Fifty-four
categories: 8thLight apprenticeship
---

Today I added a new feature to the chat application: message history. Any new
client who joins the conversation is sent all the previous messages (if any).
Currently the message history is stored in memory, and the chat room sends a
newly subscribed client all the history, one message at a time. Neither is a
sustainable solution, but they prove the concept and can be swapped in place
by alternatives. Also, the in-memory version of the repository will be used for
testing.

Once the above was all working, I started looking into creating a persisting
replacement for message history. Java defines a series of interfaces around data
persistence, one of them being `DataSource`. What is really cool about this is
that Java only defines the interface, but the implementation is left to the
vendor. This is the ultimate contract. So I added the PostgreSQL library as a
dependency and started playing with it in the afternoon; after some time I had
my chat room write messages received to the database!
I am not sure I will be able to integrate this version of the message repository
in time for when my current iteration ends, but it has been really fun to tinker
with it anyway.
