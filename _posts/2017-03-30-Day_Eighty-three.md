---
layout: post
title: 8th Light apprenticeship - Day Eighty-three
categories: 8thLight apprenticeship
---

My favourite keyword in Javascript: this.

>This? What do you mean??

No, it's not like that. There's this keyword in Javascript whose name is __this__.
The word __this__ is the keyword. Not sure if there is any other word than "__this__" in
the dictionary to make talking about it so difficult.

You see, at least when I am writing about this keyword, I can differentiate between
the normal this and this __this__ by switching to bold, or cursive etc. But what do
I do if I am talking about __this__ with another reasonable human being? Do you say
this normally and wink when you mean to say __this__? Or maybe you could say __this__
a lot louder and slower than the normal this. I mean, it should be easy from the
context to deduce which this you are referring to, but it is good to be clear, especially
when you are getting to grips with this keyword (__this__).

OK, so the name is a bit confusing, could have thought a bit more about it, but I
am sure what it is (or does) is nice and straightforward...

So, what is __this__? (Remember to say any __this__ in bold louder if you are
reading this to your children as a bedtime story so they do not get confused about
this...).

__This__ is a placeholder: much like a variable, it has a value associated to it.
Again, like a variable, it normally lives inside a function.
Unlike a variable, the associated value cannot be inferred by _where_ in your code
__this__ is defined, but instead it changes depending on _where_ the function is
called from. The above are referred to as _lexical_ and _dynamic_ scope. Lexical
scope is nice and simple: the value of a variable with lexical scope can easily
be deduced by looking at where it is defined; that's it, which is why this is also
called _static_ scope. With dynamic scope, well, it depends; it depends on where
the function containing __this__ is invoked, which is why __this__ is also referred
to as the _context_. OK, time to see some code and make some sense of this.

First, let's define a `Horn` object, which has a horn sound that it can print
out to the console, and a function to tell us what __this__ is.

{% highlight javascript %}

  let Horn = function() {
      this.sound = "BEEEEEEP";
      this.playSound = function() { console.log(this.sound) }
      this.whatIsThis = function() { console.log(this) }
  }

{% endhighlight %}

Now let's use the horn to make some noise.

{% highlight javascript %}

  let myHorn = new Horn();
  myHorn.playSound(); // => BEEEEEEP

{% endhighlight %}

OK, so far so good. Now let's say we need to pass `playSound` as a callback. Maybe
you need to perform some other actions before blasting out, maybe you just want to
build the suspense; in either case, you need to pass the `playSound` function to
something that performs those actions or builds the suspense. We are going to
build suspense by using `setTimeout`, which takes a callback function and the
amount of milliseconds to wait before invoking the function.

{% highlight javascript %}

  let playHornCallBack = myHorn.playSound;
  setTimeout(playHornCallBack, 500); // => undefined

{% endhighlight %}

Say whaaat? Why is it undefined? I thought the sound was `BEEEEEEEP`, what happened
to that??
Well, as mentioned above, what __this__ is depends on where the function it
lives in is invoked. When we invoke `playSound` inside the `Horn` object, we can
see that __this__ includes a sound:

{% highlight javascript %}

  myHorn.whatIsThis() // => Horn {sound: "BEEEEEEP", ...}

{% endhighlight %}

However, when we invoke `playSound` within `setTimeout`, __this__ is something else,
and we can see that `sound` is not defined in it:

{% highlight javascript %}

  setTimeout(function() {
                          console.log(this);
                          console.log(this.sound);
                          playHornCallBack();
                        }, 500); // => Window {...}, undefined

{% endhighlight %}

Interesting, so when we use `setTimeout` we are invoking `playHornCallBack` with
`Window` set as __this__. OK if this is true, then we can set a sound inside the
Window object and we should get some noise.

{% highlight javascript %}

  this.sound = "THIS IS LOCO";
  setTimeout(playHornCallBack, 500); // => THIS IS LOCO

{% endhighlight %}

Wow, this __this__ really is crazy!
Now, writing directly into the window object like we did above is evil, do not do
it. There is a way to override what the context (__this__) is to a function, by
using the function `bind`. Let's do that.

{% highlight javascript %}

  let boundPlayHornCallBack = myHorn.playSound.bind(myHorn);
  setTimeout(boundPlayHornCallBack, 500); // => BEEEEEEP

{% endhighlight %}

Cool, I think this is it. __This__ is it.

Notice that there are more quirks to determine how __this__ is deduces, but I went
through the most common.

I hope you now love __this__.
This is the end to this post about __this__.

This

