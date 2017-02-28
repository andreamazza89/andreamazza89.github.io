---
layout: post
title: 8th Light apprenticeship - Day Sixty-three
categories: 8thLight apprenticeship
---

Today I managed to get all the acceptance tests for the HTTP server to turn green!!

It was a nice moment; there is something deeply pleasing about turning a test from
red to green, especially after having some of them red for a while..

But, like someone once said, *how it is done is as important as getting it done*,
and I have a lot to refactor; most of the refactoring is straightforward, but
I will also be working on making my `Resource` class open/closed. This currently
adapts its behaviour switching on the request method; instead, I want to have
different types of `Resource`, which all conform to the same interface (`generateResponse`)
but implicitly know how to behave; there will be a `GetResource`, a `PostResource`
etc., which is selected by the `Resources` collection based on the request path and
method.

I am looking forward to receiving feedback on my work, and will write a retrospective
on the exercise afterwards. Building this server has been really fun so far and
got me to learn heaps, both about specifics of the HTTP 1.1 protocol and software
development.
