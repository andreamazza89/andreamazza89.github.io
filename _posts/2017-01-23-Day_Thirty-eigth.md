---
layout: post
title: 8th Light apprenticeship - Day Thirty-eight
categories: 8thLight apprenticeship
---

### Pills
- Nice retro on standups.
- Data types make our programs more consistent.

Today one of the crafters organised a retrospective on our daily standups. This
was really nice and we discussed what we like about a standup, what should be changed,
but most importantly why we do it. What I like is the attitude of not taking what we do
for granted just because we have always been doing it. This goes in line with
continuous learning and a mindset that keeps us reassessing what we do and how
we do it.

I have been working on my contact manager applcation the rest of the day. In the
code, I have a Contact class, which holds all contact information as properties.
Up until now, all these fields where simply strings or integers, however today
I created a custom type for age and telephone number. Why? To have a type that
truly represents the data I am dealing with. For instance, by using an integer
to represent age, I am allowing any integer, including negatives. Instead of
scattering rules about age all around the codebase, we can simply create an Age
class, which, for instance, does not allow a negative age to exist, by definition.
Notice that it does not (nor should) matter how we represent and keep track of
the age, as this is encapsulated in the Age class.
