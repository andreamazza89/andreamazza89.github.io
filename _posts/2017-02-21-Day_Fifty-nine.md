---
layout: post
title: 8th Light apprenticeship - Day Fifty-nine
categories: 8thLight apprenticeship
---

Another day working on the HTTP server. I spent a large part of today not adding
any features; instead I have been refactoring the system to make it simple, so
that it will (hopefully) be easy to add new functionality to it.

One interesting dilemma I had was on whether to use a null object or a java optional
in one case (I am going to avoid using details from my project, hopefully this makes things
easier).

Here is the situation: somewhere in your system, you are 'looking' at an object and
need to send a message to it; however, that object might not be there. For example,
let's say we have a `User` object. This is returned when a previously signed up
user enters the system. You might want to call `User.getName()` to eventually
display the user name on the page. However, if whoever connected did not previously
sign up, then the user is missing. The worse case scenario is where the expected
`User` object is instead a `null`, which will throw an exception when sent `.getName()`.
The easiest thing to do at this point is a null check: if the user does not exist,
then act accordingly; in our case, probably use something like `"unregistered user"`
instead of the user's name. However, this can easily get out of hand as the requirements
on the non-existing user grow and our logic is polluted by the null check.

The key is to realise that, as Sandi Metz says, *nothing is something*.

There can be such thing as a `MissingUser`, or `NullUser`. This would be an object
that follows the same interface as `User`, however, when sent `.getName()`, it
returns `"unregistered user"`, which is our desired behaviour. Notice that we
have to create an interface that both `User` and `MissingUser` implement. The part
of our system that was asking the user for its name no longer needs to check if
the user is there, it just needs an object that can respond to `.getName()`.

Order is restored. But not quite; a problem that can arise is that our `User` might
have a much larger interface that `MissingUser` needs, but because they need to
implement the same interface, the latter is forced to implement a lot of methods
that it does not expect to ever be called. This does not feel right.
Another option is to make `MissingUser` inherit from `User` and override the
methods we need. This removes the need to implement all the other methods that
have nothing to do with `MissingUser`. This is only a false sense of security though,
as `MissingUser` does inherit all methods from `User`. Mmm trade-offs.

There is another option: Java 8 introduced an `Optional` type; for example, an
`Optional<User>` represents a user or the lack of it. This encodes the problem
intrinsically, and makes it explicit. In addition, it provides an API that lets
you deal with both cases in a clean manner, so that our querying the user name
would look something like this: `user.getName().orElse("unregistered user")`.
