---
layout: post
title: 8th Light apprenticeship - Day Sixty-four
categories: 8thLight apprenticeship
---

The idea behind the Rack gem (ruby) is simple: a request object (in Rack they
call this `env`) is generated somewhere, then handed to a stack of middleware layers.
Each layer needs to conform to a very simple interface: it has to have a `call`
function, which will be called with the `env` (request) object and needs to
return a response object, consisting of a status, headers and optional body.

This model maps extremely well with the request/response cycle of HTTP: a request
is received, then processed by a number of layers, until a response is created and
sent back to the client. The beauty of such a simple contract (the interface
described above) is that middleware layers can be added or reshuffled in place,
as they all conform to the same interface.
This is be an example stack of middleware layers:
- A Request object (`env`) is formed from an incoming message.
- The Request is handed over to a logging layer by calling `call(env)` on it.
This logs the request in one way or another and then calls `call(env)` on the next layer of the stack,
which was injected into the logger at initialisation.
- The next layer is the authenticating one. It validates the incoming request (`env`)
and either: a) returns a `401` response or b) passes the request further down the
middleware stack. Again, a reference to the next layer was passed at initialisation.
- At this point, we reach the core of our application, which is the router.
This is handed the request, complying to the `call(env)` interface, and
returns a response, which goes via the authenticator, then back to the logger, finally
dispatched to the client.

What is really elegant about this design is that none of the middleware layers
sitting between the client's original request and the core of our application
(the router) need to know about each other, they only know that they should
pass the Request (which they might have altered or not) down to another layer. For
instance, in the example above, we could add a mailer that notifies the system
administrator every time a request is made for the `/secret_recipe` route. This
could sit before or after the logger. The only thing we would need to change is
how the stack of middleware layers is created.

Pretty cool no? I like to think of this whole idea as a pipeline that processes
an entity (the initial request) one after the other, until the result (a response)
is ready to be returned.

A drawing of the example above would look a bit like this:

```
                                       A
                                       u
                                       t
                      L        M       h       R
                      o        a       e       o
        Request -----> -------> ------> ------>  ---
                      g        i       n       u   |
                      g        l       t       t   |
        Response <---- <------- <------ <-----  <---
                      e        e       i       e
                      r        r       c       r
                                       a
                                       t
                                       o
                                       r
```
