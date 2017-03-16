---
layout: post
title: 8th Light apprenticeship - Day Seventy-three
categories: 8thLight apprenticeship
---

These past few days I have been working on a project with other apprentices,
where we are adding functionality to an existing internal application.
The task we have been working on so far is to do with navigation, so I will try
to explain my understanding of it here.

The back end for this application uses Ruby on Rails. This lets you define routes
that will be served and associate controllers to generate a response for them.
For example, you can specify `restaurants` as a resource, and Rails will respond
to a request on `your_host_name/restaurants` using a `Restaurant` controller, which you
need to create. This controller will likely use a template to either return html
(a view that the browser will display) or raw data, in JSON format.

This is the main routing mechanism in our application: if a request is made for
an existing resource, Rails responds with an HTML view, which might contain links
to other pages available.

So far so good; we can navigate through our application going to routes that are defined
in Rails' configuration.

Let's introduce some complication. Navigating between routes and re-rendering a
new view every time we change page is relatively slow and inefficient. We also
re-render elements that might not have changed, such as the header and footer.
This is why so-called single page applications exist. These simply load an HTML
view when we first connect to the application, then leave all further routing and
view management to be handled in the front end. Any additional data required is
fetched by the client with an asynchronous request, normally expecting a JSON
payload. This makes the front end more fluid, only updating parts of the view that
have changed and much faster and smoother transitions between pages. This approach
does however create a problem: while we navigate through the application, the
browser 'thinks' that we have only visited one page, the first that was loaded.
Most front end frameworks and libraries deal with this issue by keeping track of
user navigation and tricking the browser into thinking that the user did navigate
to new pages.

Now back to our application; as discussed above, our site follows the former type
(back-end based) of routing, managed within Rails. However, some of the pages
do have a degree of further front-end based navigation. Whenever we request a new
page from the back end, an HTML view is returned along with a bundled Javascript
file, which sets up event listeners. The listeners can catch a click over a link and
update the view accordingly, without reloading the entire page.
This client-side rendering leads to the problem mentioned above: while we have
moved to a new view, the browser still 'thinks' that we are looking at the same
page.

We have been working with the browser's `history` object to fix this and simulate
browser navigation.
