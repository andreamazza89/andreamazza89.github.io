---
layout: post
title: 8th Light apprenticeship - Day Forty-three
categories: 8thLight apprenticeship
---

### Pills
- Optional objects in Java.

Penultimate day working on my contact manager app in Java today. As part of the
few tasks left to do, I was asked to make one of the contact fields (post code) optional, i.e.
not required for a contact to be valid.
This does not mean that the user can leave the input field blank and the system,
while storing an empty string for the post code, will not complain about it. Instead,
it should be possible to create a Contact object with a missing post code.
This idea is represented in Java using the relatively recent class `Optional`.
This is used to wrap an object and allow it to exist or not. In other words, if
you were to wrap a String into an Optional, then you would have a new type, which could
either be a String, or nothing. So my Contact class now takes an optional post
code input and as a result, returns an optional post code from its getter.
I like this, it makes my code more 'honest': using an optional type lets any client
know that the value might be there or not, so that they have to deal with the
possibility of an empty optional as well as the 'happy path'.

This:

{% highlight java %}
  updatePostcodeShown(contact.getPostcode().orElse(""));
{% endhighlight %}

Is much nicer than this:

{% highlight java %}
  if (contact.getPostcode() != null) {
    updatePostcodeShown(contact.getPostcode());
  } else {
    updatePostcodeShown("");
  }
{% endhighlight %}
