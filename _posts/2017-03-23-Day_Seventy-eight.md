---
layout: post
title: 8th Light apprenticeship - Day Seventy-eight
categories: 8thLight apprenticeship
---

Today we had an interesting problem working on the apprentices pairing project.

Normally, when you write code, you write it in a specific version of a certain
language. Knowing what version of the language you are using means knowing that
your code will run on any machine, as long as the same language version is being used.

Let me state the obvious: if you run your code in a machine using a different version
of the language than the one you used, then some of the features you rely on might
not be available.

This is seldom a problem, as we have control over what language
is used in any given machine we run our code in.

The above is sadly not true for one ubiquitous language that we all rely upon
(knowingly or not) everyday: Javascript. There are two main environments in which
the language can be run: using Node or using a browser. Each different browser
has its own version of the language and we have no control over what version of
Javascript this is, so that our code written in version X might be run on several
other versions.

This was the nature of our problem: the function we were using would not work in
one of the environments in which we were running the code! Luckily, we found another
function with a different name but same outcome, which works across all the platforms
we had at our disposal.
