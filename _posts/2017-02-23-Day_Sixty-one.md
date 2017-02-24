---
layout: post
title: 8th Light apprenticeship - Day Sixty-one
categories: 8thLight apprenticeship
---

Today has helped me understand data encoding better.

Anything that goes through a computer will eventually become ones and zeros.
This is also true for data that is transferred between two computers. For example,
if an HTTP server wants to send the message "Hello Client!" back to the client,
it would first turn each character into ones and zeroes according to a protocol,
then send the raw data to the client down the wire. The Client will in turn receive this stream of bits, which will only make sense if it knows how to decode them.

The HTTP protocol describes both the anatomy of a message (status/request line,
headers, message body) and the encoding that should be used for each part.
This way, the receiving end knows how to decode the incoming bits and reconstruct
the original message. This mechanism allows the message to contain a mixture of data: it always has text to describe the convey information about the message, but it might also include an image, sound, etc..

Now with the above in mind, the problem I ran into today was due to the fact that
my server 'reasoned' in strings rather than bits. In other words, data would flow
through the server as strings, and then, just before being sent to the client, it
would be turned into ones and zeroes. This did not show any problems as long as
my messages only contained text. However, today I worked on sending an image, which
did not function, as my system forced me to turn that image into a string, then back
to bits, then send it. This process corrupts the file, so the image is broken
at the other end. This was a good learning experience, as it got me to understand
these concepts and finally change my system to have raw bytes running through
it rather than strings; JPEG peace was restored.
