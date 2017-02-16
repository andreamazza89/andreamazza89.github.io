---
layout: post
title: 8th Light apprenticeship - Day Fifty-five
categories: 8thLight apprenticeship
---

Last day working on the chat application today. I managed to replace the in-memory
repository with a PostgreSQL one. It is fairly simple but it was really cool to
see my program write to a database running on my machine and then retrieving the
data in subsequent runs. Aside from learning about java's `DataSource` interface
and its siblings, I was also happy to see that the tests I wrote for the in-memory
version of the repository worked for the SQL-based one. In fact, using JUnit's
`@Parameterized` flag, I could run the same tests for the two different classes.
Aside from the lack of duplication, this reveals that the tests I wrote are asserting
on the interface rather than a specific implementation of it, which is good.

Moving onto my next assignment, I will be working on implementing an HTTP server
in Java, which is a milestone of the apprenticeship program: every 8thLight employee that
goes through the apprenticeship (that is, every one) will have done this at some
point. What I am going to try build has to comply to the http 1.1 specification.
This is very exciting: a suite of acceptance tests were written to verify that
the server is compliant, so development will be driven by these and I will slowly
turn all the red bars into green bars, while adding unit tests. Pretty cool I think.
I am also looking forward to learning about the http protocol in depth:
this is the kind of thing that one might never find the time or expedient to do,
but for next week or two, this is my job! How cool.
