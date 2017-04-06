---
layout: post
title: 8th Light apprenticeship - Day Eighty-seven
categories: 8thLight apprenticeship
---

Lazy evaluation.

Lazy evaluation is not something strictly exclusive to functional programming but
it seems to be much more common in functional than object-oriented languages.

The name is a pretty good descriptor: lazy evaluation is lazy because it only
does the work if and when needed. This sounds good: with lazy evaluation we
only use precious processor cycles if and when needed.

Let's talk about two examples: dealing with infinite sequences and optimising
expensive operations.

Dealing with infinity is a pretty cool concept philosophically: how can such a
finite thing as a computer hold something infinite?
Anyway, the trick is that we never aim for infinity; infinite sequences exist
because sometimes we do not know how many items we are going to need from them
but we know how to work out the next item, so we only lazy-evaluate the ones we
need when we know how many we need.

Here is an example taken from [this book](http://www.braveclojure.com/core-functions-in-depth/):

{% highlight clojure %}

(concat (take 8 (repeat "na")) ["Batman!"])
; => ("na" "na" "na" "na" "na" "na" "na" "na" "Batman!")

{% endhighlight %}

You see, `repeat` is a lazy function and it only evaluates when required (in this
case we are asking for the first 8 items in the sequence). Had it not
been lazy we would be waiting for `repeat` to infinitely evaluate before we could grab
the first 8 items in the sequence. Yarn.

On to the second point, to do with efficiency: even if we are dealing
with a finite collection, imagine one where going through each item is expensive
(there is always some time associated with going through a collection of items;
by expensive I mean it takes long enough to quickly become noticeable to a human).
For instance, say we need to take a database table and look for an item, but in
order to find the item we have to perform some operation on it. Here is a simple
example: say we have a table of a million people and we are looking for one person
with golden hair. However, hair colour is stored as a string, and before we can
search we have to convert the string to a unique colour id (a number).
Now, without lazy evaluation, we would have to go through each entry in the table
and do the string->integer conversion before searching. If each conversion took 1second, it would
take at least 1 million seconds (and a bit) to find our golden haired person.
Compare this with lazy evaluation, where there would only be as many string->integer
conversions as necessary to get to the first golden haired person in the table.
Of course there would be no gain if the only match was at the end of the table,
but that is just the worst case scenario!
