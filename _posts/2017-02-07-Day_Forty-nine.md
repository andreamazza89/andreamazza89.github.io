---
layout: post
title: 8th Light apprenticeship - Day Forty-nine
categories: 8thLight apprenticeship
---

### Pills
- Java's CountDownLatch.

Today I learned about the `CountDownLatch` class. This is a tool to achieve
synchronisation between two or more threads. The use case is: you have multiple
threads working on separate tasks (call them A and B), at their own speed;
At some point A relies on B to have finished a certain operation before it can proceed.
This is in direct contrast with what threads are, as A and B do not know about
each other and are running in their own thread. They could be running in separate
machines as far as they are concerned. So how can we let A know when a certain
operation was completed in B? Enter the `CountDownLatch`. Imagine this as a portal
between A and B; it is injected in both A and B. The `CounDownLatch` is instantiated
with a number to countdown from, and has a blocking function, `await`, which blocks
a process until the countdown reaches zero. So, back to A and B, we call the `await`
function in A just before we get to the point where we have to wait for B, whereas
in B we hit the `countDown` (from 1 to 0 in this example) once the function we were waiting for has finished.
Nice, now we have the synchronisation we required!
