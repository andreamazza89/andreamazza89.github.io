---
layout: post
title: 8th Light apprenticeship - Day Sixty-five
categories: 8thLight apprenticeship
---

A small thought from a conversation I had with one of my mentors today: in
object-oriented programming, whenever I create a new class, I should keep an eye
on its fields to see if any of them might be an object. For example, in my HTTP
server I have a `Request` class; this has a field that contains headers. To access
a header you call `request.getHeader("NameOfHeader")`. The client is forced to
_know_ that headers are stored in a Map, using strings as keys. Not only does this
break encapsulation, but also forced me to store all headers the same way (as a
string), whereas it would be more useful to have different types depending on the
header; for instance, the content length header should probably be represented
internally as an integer and return one rather than a string. I am now refactoring
my code for better encapsulation and appropriate typing.

So in summary I should develop a smell-radar for lack of object-orientation:
if I am using a native type to represent data, is it well encapsulated, or is
there a hidden object in there? A simple example of this is using integers to
represent age; does this make sense? No, there is no such thing as a negative age
so you should create an `Age` type.
What I failed to spot in my server was the object hidden inside another object!

The London Software Craftsmanship Community I went to a few months ago on
[object calisthenics](http://williamdurand.fr/2013/06/03/object-calisthenics/) is
now making more sense than it did at the time, when I had never programmed in a
static language.
