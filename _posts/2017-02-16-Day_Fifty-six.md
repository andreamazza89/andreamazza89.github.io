---
layout: post
title: 8th Light apprenticeship - Day Fifty-six
categories: 8thLight apprenticeship
---

First full day on the HTTP server. Today I skimmed through the HTTP/1.1 specification.
A few quick thoughts on something that caught my interest follow.

The first line of any HTTP response message includes three tokens: the HTTP version,
status code, and reason phrase. The status code is one of a number of codes that
describes the outcome of the request; for example, if everything went well and the
response includes whatever resource was asked for in the request, you get a nice
round 200. On the contrary, if the resource the user-agent asked for does not
exist, you get a bitter 404. The last field in the first line, as mentioned, is the
reason phrase. The reason phrase is a human-readable description of what the status
code means. For instance, 200 is `OK`, and 404 is `Not Found`. There is a list of
standard codes and what their reason phrase should be, but the phrase can be modified
since it is intended for human consumption. What I am not too sure about is why
the duplication of information exists: receiving a message with `200 OK` could be compared
to getting `Yes Si` in human language: two ways to convey the same information. Having two
sources of truth is a dangerous thing, so there must be a valid reason why this
rule was broken in the HTTP protocol. What I am thinking is that even if you are looking at a
raw HTTP message response, then you must know about the standard, and if you do,
you can just have a reference sheet that matches status codes to what they mean;
so why the extra characters and duplication? One reason I can think of is perhaps
the fact that status codes can be extended, so that a human looking at an HTTP
response with a code that is not part of the standard set can get immediate
information on what that code means for the origin server; it is also true that
two organisations might use the same custom code for different reasons, so that,
thanks to the reason phrase, a user of these two organisations will be able to
discern between the two messages with equal code but different meaning.

Another interesting fact is that the protocol encourages clients to pipeline
requests that are idempotent. That's a mouthful! An idempotent request is similar to a
pure function: the result of such request never changes for a given input, though
it might have side effects, which will always be the same. In simple terms, this
means that a client does not need to send requests one at a time, waiting for
each one to be satisfied before sending a new one. Instead (again, only if the
transactions are idempotent), it can fire all requests in a row, and the server
will respond (if all goes well!) to them at some point in the future. This can
reduce the total time taken to get all responses back. Nice; however, one thing
I do not get is why the specification calls for the server to respond to the
requests _in the order they came in_ (section 8.1.2.2). This seems unnecessary and possibly
taxing: if the requests are idempotent, then why does the order in which they
get processed matter? In terms of efficiency, I can imagine a scenario where the
server could respond to request no.2 right away but for no.1 it has to wait a while.
Having to queue the responses makes the total response time longer than not queuing.
I cannot work out why there would be a reason for this, but I am sure one exists;
I will try to find out!
