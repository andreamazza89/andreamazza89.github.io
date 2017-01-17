---
layout: post
title: 8th Light apprenticeship - Day Thirty-three
categories: 8thLight apprenticeship
---

### Pills
- Started playing with Java FX.

Today I started getting acquainted with Java FX, a graphical user interface
framework that I am going to use for my contact manager application.
It is nice to see that the way graphical elements are declared and handled is not
too dissimilar to a web page: you can use a markup language to define a tree-like
structure much like HTML. You can also define styling for these elements as a
separate asset (this actually supports CSS) and finally you can define logic that
handles user interaction, things like the click of a button, the arrow position
and pressing the enter key. This last element is defined in Java, though the
library does also support Javascript, Clojure and other JVM-able languages.

Referring back to my post on MV\* from yesterday, Java FX should only be concerned
with the View and the star.

To familiarise myself, I first looked at the most basic tutorials to understand
the building blocks and gotchas of the framework, putting together a Hello World
app that prints some text on pressing a button. This was effectively a Model-less
app, so I then went ahead and made one that uses my Roman Numerals kata to
convert numbers. Nice.

Finally, I worked on porting the existing functionality on my contact manager to
JavaFX. I had a major blocker, as I decided to use a TableView class to display
search results but initially failed to fully understand how to use this. I simply
assumed it would work in a similar way to HTML tabs and initialy got frustrated by
not being able to even display a constant string. After some research I got to
grips with how this class works and managed to make it work. A TableView is a
fairly sophisticated class, made of TableColumns, each to be pointed to a collection
of observable objects. Say for instance that you have a list of Contact objects.
These can be given to a TableColumn and then the table column is 'told' what
property on the Contact object it should be looking at; so, for instance, you
would tell the `nameColumn` to point at the `name` property on the `Contact` object.
This makes for a verbose contract with the framework, but it does give powerful
features in exchange, including data binding (if a Contact is updated in the
background, the table display is refreshed as the column was observing the object).
Observable objects do need to follow a specific interface convention, so I opted
to not enable data binding, so my TableColumns load a property of the Object/s
they have on them but then do not observe the object for changes.

I am looking forward to adding more features and seeing the architecture of this
app take shape. I also should look into what to do about acceptance tests.

Ciao.
