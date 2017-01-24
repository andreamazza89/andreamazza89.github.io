---
layout: post
title: 8th Light apprenticeship - Day Thirty-nine
categories: 8thLight apprenticeship
---

### Pills
- Self-reminder to always keep the refactoring mind on.
- Exceptions in Java.

This afternoon I paired over my contact manager app with Enrique, which was very
useful. The biggest takeaway is that I should constantly have my eyes open to look
for potential refactorings to make the code simpler and easier to understand. This is
not the first time I have to remind myself of this, so I have now setup a daily
reminder of this that I will turn off when refactoring/simplifying becomes second nature.

Today I also started looking into exceptions in Java, so here is a
stream-of-consciousness-like report of what I learned:

- Any exception is an object.
- This object gets thrown by a method when something goes badly.
- The client of the method that raised the exception was waiting for a nice return
value, but instead it gets slapped in the face with this burning thing (the
exception).
- Being an object, an exception *should* include useful information on what went
wrong.
- There are two main types of exceptions: checked and unchecked.
- A checked exception makes the exception a compiling problem; a method that throws
a checked exception needs to say so in its signature, and any of its users need to
either try/catch it or throw it again in their signature.
- An unchecked exception, as the name suggests, does not require declaring the
potential for failure in the method signature.
- As a general rule, one should only use a checked expression if the client might
be able to recover from the exception.
