---
layout: post
title: 8th Light apprenticeship - Day Twenty-one | Demystifying Elixir macros
categories: 8thLight apprenticeship
---

A disclaimer before we begin: I am not advocating the use of macros.
The decision to use macros in your code should be given serious consideration
and be justified on a case by case basis.

I do not like magic tricks in programming. If something is mysteriously happening,
I get an uncomfortable feeling that I am not in control of the way my code
functions: what if the magic stops? How will I fix it if I do not know how it operates?
I am not saying that I want to understand everything that goes on in a program,
down to electrons flowing inside the processor, much like a mechanic does not
need to understand the science behind internal combustion engines in order to
repair a car. Being able to make use of a system without fully understanding how
it works at all levels is called information hiding, which can be very useful,
as it allows people to specialise in one area of a system rather than the whole.
What I am not happy with is magic that happens at the same level of abstraction
that I am in. For example, the Elixir Plug library has a Router module that
allows you to define how to handle an HTTP request, like so:

{% highlight elixir %}
  get "/hello" do
    send_resp(conn, 200, "world")
  end
{% endhighlight %}

The above defines a route, “/hello”, which responds to a GET request with the
text “world” and HTTP status of 200. The code also contains some magic: `conn` entered the scope
of `get` and was assigned a value outside of our control. `conn` is a Plug connection
struct, which is the data structure used to represent an HTTP request.

`get` is a macro, so we need to understand macros before we can find out how
`conn` magically made it into our code.

In Elixir, a macro is a mechanism that allows meta-programming. Meta-programming
is a technique whereby  you write code whose purpose is to create or modify other
code, rather than executing. If you feel uncomfortable with this, you are having
a reasonable reaction: meta-programming can be a very dangerous tool that should
only be used as a last resort, the main reason being how much harder it makes
your code to understand.

The compilation process in Elixir involves generating an Abstract Syntax Tree
(AST) from the source code, which is then fed to the compiler to generate
bytecode for the virtual machine. The AST is simply a ‘translation’ of our source
code into a format that is understood by the compiler. When we write a macro,
we directly manipulate the AST.

The major building block for macros is `quote` (which is itself a macro!);
this converts any expression into its AST equivalent, which is represented as a
tuple; let’s have a look:

{% highlight elixir %}
  iex()> quote do: sum(1,2,3)
    # => {:sum, [], [1, 2, 3]}
{% endhighlight %}

In the resulting tuple, the first element is an atom identifying the function,
the second is metadata (none in this case), and the third is the function's
list of arguments.

Passing a variable instead of a function to `quote` yields a slightly different
result:

{% highlight elixir %}
  iex()> quote do: foo
    # => {:foo, [], Elixir}
{% endhighlight %}

The first element is an atom identifying the variable, the second
metadata (none given in this case), and the third the context in which the variable
exists ("Elixir" being the global scope). Notice how the above does not raise an
error, even though the variable `foo` has not been previously declared.
This is because `quote` simply translates the given block into its AST
representation, regardless of variable declaration.

Finally, you can see why humans prefer writing source code to ASTs: a
simple expression with two additions turns into the following ugly nested expression,
which is unreasonably hard to decode:

{% highlight elixir %}
  iex()> quote do: (1 + 2) + 3
    # => {:+, [context: Elixir, import: Kernel], [{:+, [context: Elixir, import: Kernel], [1, 2]}, 3]}
{% endhighlight %}

The second building block for macros is `unquote`. This is a similar mechanism to
string interpolation, which allows us to inject a value, expression, variable,
etc. into a string. Here is a simple example of `unquote`, which can only
be used inside a `quote` block:

First we define a variable, `a`, in the current scope and assign it a value of `2`.

{% highlight elixir %}
  iex()> a = 2
{% endhighlight %}

Then we create a simple AST that uses `a`.

{% highlight elixir %}
  iex()> quote do: 40 + a
    # => {:+, [context: Elixir, import: Kernel], [40, {:a, [], Elixir}]}
{% endhighlight %}

The resulting tuple describes addition between the value `40` and the variable
`a`. This is fine, but what if we wanted to use the current value of `a`, which
we have set to `2`?

What we need is `unquote`, which interpolates the current value of `a` inside the
`quote` expression:

{% highlight elixir %}
  iex()> quote do: 40 + unquote(a)
    # => {:+, [context: Elixir, import: Kernel], [40, 2]}
{% endhighlight %}

Macros are defined inside a module declaration, and their parameters —if any— are
given to it in quoted form. A macro needs to return a quoted expression, as this is
then used by the compiler to manipulate the AST.

Here is an example where we create a macro named `evaluate` that takes an expression,
prints out a message describing the expression and its result, then finally
returns the result. The function `Macro.to_string` simply takes a quoted expression
and returns a string representation of it.

{% highlight elixir %}
  defmodule VerboseEvaluation do

    defmacro evaluate(expression) do
      string_representation = Macro.to_string(expression)

      quote do
        result = unquote(expression)
        IO.puts "The expression " <> unquote(string_representation) <>
                " is " <> (inspect result)
        result
      end
    end

  end
{% endhighlight %}

You can play with the above by pasting it into the Elixir REPL, then requiring the
`VerboseEvaluation` module, like so:

{% highlight elixir %}
  # ... paste the module

  iex()> require VerboseEvaluation
    # => VerboseEvaluation

  iex()> VerboseEvaluation.evaluate(4 + 38)
    # The expression 4 + 38 is 42
    # => 42
{% endhighlight %}

With a basic understanding of how macros are constructed, let's look again at
Plug's `conn` struct.

I mentioned how Plug magically injects a variable —`conn`—
into your code, with a value already assigned to it.
Macros are, by default, hygienic, meaning that any variable created inside the
macro is not allowed to leak into the scope the macro is used in. There are
however ways to override this, like using the macro `var!`; here is a simple
example:

{% highlight elixir %}
  # paste this into iex
  ################################
  defmodule MagicVariable do

    defmacro inject_variable do
      quote do
        var!(secret_variable) = 42
        "secret_variable now initialised"
      end
    end

  end
  ################################

  #...after pasting the above snippet, type in the following:
  iex()> require MagicVariable
    # => MagicVariable

  iex()> secret_variable
    # => ** (CompileError) iex:3: undefined function secret_variable/0

  iex()> MagicVariable.inject_variable
    # => "secret_variable now initialised"

  iex()> secret_variable
    # => 42
{% endhighlight %}

Magic! The `inject_variable` initialises and assigns `42` to a variable named
*secret_variable*, in whatever scope it is called from! The code in the Plug
library is of course much more complex, but this is to give you an idea of how
the result can be achieved.

There is much more to macros than I have covered here, but hopefully I have tickled your curiosity,
so that next time magic things occur, you might want to look through the source code
and figure out what is really happening under the hood.
