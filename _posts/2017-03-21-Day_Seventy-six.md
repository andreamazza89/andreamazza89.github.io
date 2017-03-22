---
layout: post
title: 8th Light apprenticeship - Day Seventy-six
categories: 8thLight apprenticeship
---

Travelling through time in the browser.

These last few days in the apprentices project we have been working on a task
that deals with browser history.

Here is what the dictionary on my laptop has to say about history:

_a continuous, typically chronological, record of important or public events_

For the browser, history is a chronological succession of discrete events. These
are recorded as history so that one can easily go backwards and forwards in time;
useful.

Now, what are these discrete events we are keeping track of so we can navigate
through them? Glad you asked.
(Most) browsers only treat new page loads as new events; this means that a new entry
in the 'History book' is made only if the browser makes a request to a server and
a new view is rendered as a result. It makes some kind of sense: applications
can be very different between each other, but we are using the same browser to
navigate through them, so the chosen common denominator is fresh new-page loads. Anything
that might occur between page loads does not make it to the history book.

Here comes the complication; with the advent of Javascript and client-side
manipulation of page content, we have the power to update some of the page
without reloading a full view. This looks and feels good: think modals,
think page content changing without the header/footer momentarily flashing due
to a full page reload. Behold client-side rendering!
The problem is, as mentioned above, that any events happening between full page
loads do not make it to the history book. If we are using Javascript to update the
view within the same page, it might look and feel as though we are navigating to
different pages, but in fact we have been looking at the same page all along, so
that if we hit the browser 'back' button, ooops, we have gone back to whatever
page we were at before visiting this site. An extreme example of this phenomenon
are so-called single page apps. With these there is only one page load and all
further navigation is handled client-side.

This is a problem. Our users are losing their minds trying to figure out why they
just spent 20 minutes browsing through our single page app, then pressed back and
rather than going back to the previous view they were at, they are back to their
favourite search engine!

A solution to this problem was introduced into the WebApi (a set of functionalities
that a browser _should_ offer), with a function that
lets Javascript feed history events to the browser.

In the last few days we have been getting our heads around the topic and adding
ad-hoc injections into the history using the WebApi, since the application we are
working with uses a mixture of server-side and client-side routing.
