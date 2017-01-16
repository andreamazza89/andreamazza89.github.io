---
layout: post
title: 8th Light apprenticeship - Day Thirty-three
categories: 8thLight apprenticeship
---

For any program to be of any use to a human being, a user interface must exist.
Let's use a very simple example: a calculator. Say we have built
a calculator application, which is running in a computer. This can take numbers
as input and perform operations on them. However, for this to deliver any value,
we need a way for a human to both communicate the required operation/inputs and
somehow receive the output. These are the main Inputs/Outputs to the system, and
are what we refer to collectively as user interface. A simple way to interact
with the calculator could be to use the command line; this way, the keyboard would
be our Input, and text printed onto the computer screen our output. Now these are just two
of the many ways we might use to take input from and communicate output to a human; we could,
for instance replace the keyboard with a graphical user interface where the user
picks numbers/operations with mouse clicks, and we might want to make the
output available to blind people, using a braille display. Imagine our simple
calculator did have to support both text output onto the command line and a braille
display; hopefully we should not need to rewrite the entire application. After all, the
abstract meaning of an operation, say `5 + 5` should be independent from how it
is then communicated to a human.

We have identified two major areas of concern: the core logic of a program and the
user interface used to actually make use of the functionalities the system offers.
These two are often referred to as Model and View and should be as decoupled from
each other as possible. Of course, we do need some degree of dependency between
the two, otherwise they would be completely useless: a user interface with no
functionality is as useless as that functionality without a user interface to access
it. A link between the two must be made, with the View dependent on the Model, as
I have hopefully demonstrated above (a different way to gather input or display
output should not affect the underlying logic).

This delicate relationship between View and Model is most often abstracted and
there are a number of patterns to describe this, collectively referred to as MV\*
(Model/View/something_that_glues_them_together).

The most basic functions performed by this abstraction —the glue between Model and
View— are to adapt the user input to whatever form the Model requires and also adapt the
Model's output to a format suitable for the View.
In the calculator example this would mean transforming a string or mouse click over
a certain graphical element into a call to a specific function (say `addition`)
using integer. On the output side, it would transform the given integer result back
to a string or into a command that renders the number onto the braille display.

User interfaces can be much more complex than this, with logic and a state of
their own, which determines what is currently displayed, the state of buttons,
prompts, etc. It should be clear that this additional state/logic should not
live in the model, although it deserves a Model of its own. This is called the
ViewModel in the Model-View-ViewModel (MVVM) pattern.
The ViewModel  abstraction provides isolation between the View and the Model,
removing all logic from the View, making testing easier and faster.
