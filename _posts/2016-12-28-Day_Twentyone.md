---
layout: post
title: 8th Light apprenticeship - Day Twenty-one | Demystifying Elixir macros
categories: 8thLight apprenticeship
---

A disclaimer before we begin: I am not advocating the use of macros. Quite the
opposite in fact, as it will hopefully be clear throughout the article. Deciding 
to use macros in your code should be given serious consideration and justified
by significant benefits.

I do not like magic tricks in programming. If something is mysteriously happening, I
get this uncomfortable feeling that I am not really in control of my code: what
if the magical thing stops working? How will I fix it if I have no clue on how it
operates?

I am not saying that I want to understand everything that goes on in a program,
down to electrons flowing inside the processor, the same way a car driver does
not need to know how the engine and transmission system work.
This is called information hiding and
it is a great thing. What I am not happy with is magic that happens at the same
level of abstraction that I am in. For example, the Elixir Plug library has a
Router module that allows you -amongst other things- to define what to do when a
request comes in, like so:

{% highlight elixir %}
  get "/hello" do
    send_resp(conn, 200, "world")
  end
{% endhighlight %}

Now, that is nice and simple, however, there is a slightly terrifying detail:
that `conn` variable has entered our scope and was assigned a value outside of
our control. The value is a Plug connection struct, which represents an incoming
request and is passed around the application in a pipeline fashion to be
transformed into a response that is then sent back.

Since `get` is a macro, we need to understand macros before we can find out how
that `conn` variable magically made it into our code.

The word macro can mean a number of things, but in Elixir it is the mechanism
that allows meta-programming. Meta-programming is when you write code whose
purpose is to create other code, rather than executing. If you feel uncomfortable
with this, you are having the right reaction: meta-programming is a very sharp
tool that should only be used as a last resort, the main reason being how much
harder this makes your code to understand.

The compilation process in Elixir (or at least my very limited understanding of
it!) involves generating an Abstract Syntax Tree (AST) from the source code,
which is then fed to the compiler to generate bytecode for the virtual machine. The
AST is simply a 'translation' of our source code into a format that is understood
by the compiler. Interestingly, the reason why all these languages exist is
because we cannot speak machine-language and the machine cannot speak human
language (the source code), so the same 'content' (the program) is translated
from human (the source code) all the way down to something that controls the
electrons flowing through the processor.

When we write a macro, we directly manipulate the AST. The major building block
for macros is `quote` (which is itself a macro!); this converts any expression
into its AST equivalent. This is a tuple with three elements; let's have a look:

{% highlight elixir %}
  iex()> quote do: sum(1,2,3)
    # => {:sum, [], [1, 2, 3]}

  iex()> quote do: variable
    # => {:variable, [], Elixir}

  iex()> quote do: (1 + 2) + 3
    # => {:+, [context: Elixir, import: Kernel], [{:+, [context: Elixir, import: Kernel], [1, 2]}, 3]}
{% endhighlight %}

The structure has a few nuances, but in general, the first element is an atom
identifying either the function or variable name, the second is metadata, and the
third is the argument/s to the function or context in which a variable exists (in
the second example above the variable `variable` exists in the global scope).
Notice how the second `quote` above does not raise an error, even though the variable
does not exist. This is because `quote` simply translates the given block into
its AST representation, regardless of whether the variable we are using was
declared or not. Finally, you can see why humans prefer using source code to
writing ASTs: a simple expression with two additions turns into this ugly nested
expression, which is unreasonably hard to decode.

The second building block for macros is `unquote`. This is a similar mechanism to
string interpolation, which allows us to inject a value, expression, variable,
etc. into a string. Here is a simple example of `unquote`, which can only
be used inside a `quote` block:

{% highlight elixir %}
  iex()> a = 2
  iex()> quote do: 40 + a
    # => {:+, [context: Elixir, import: Kernel], [40, {:a, [], Elixir}]}

  iex()> quote do: 40 + unquote(a)
    # => {:+, [context: Elixir, import: Kernel], [40, 2]}
{% endhighlight %}

Refer to the arguments in the resulting AST tuple: the first expression means
'forty plus whatever the variable a in the global scope is going to be',
whereas the second one means 'forty plus the current value of a', which is why I
had to declare and initialise a, otherwise its evaluation would have failed.
In short, `unquote` evaluates the input expression and injects it into the `quote`
block.

Onto macros; macros are defined inside a module and its parameters (if any) are
given to it in quoted form. A macro needs to return a quoted expression, as this is
then used to manipulate the AST.

Here is an example, where we create a macro (`evaluate`) that takes an expression,
prints out a message describing the expression and its result, then finally
returns the result.

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

You can play with the above by pasting it into an iex session, then requiring the
VerboseEvaluation module, like so:


{% highlight elixir %}
  # ... paste the module

  iex()> require VerboseEvaluation
    # => VerboseEvaluation

  iex()> VerboseEvaluation.evaluate(4 + 38)
    # The expression 4 + 38 is 42
    # => 42
{% endhighlight %}

This has been a very basic introduction to macros, as I am still getting to
grips with them myself. Please do refer to the [Elixir documentation](http://elixir-lang.org/getting-started/meta/macros.html)
as well as this nice series of [blog posts](http://www.theerlangelist.com/article/macros_1)
for much more in-depth explanations.

Finally, I mentioned how Plug magically injects a variable (`conn`) into your code, with
a value already assigned to it. Let's look into the mechanism that allows this to happen.
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

  #...paste the above
  iex()> require MagicVariable
    # => MagicVariable

  iex()> secret_variable
    # => ** (CompileError) iex:3: undefined function secret_variable/0

  iex()> MagicVariable.inject_variable
    # => "secret_variable now initialised"

  iex()> secret_variable
    # => 42
{% endhighlight %}

Magic! The `inject_variable` initialises and assigns 42 to a variable named *secret_variable*
, in whatever scope it is called from! The code in the Plug
library is of course much more complex, but this is to give you an idea of how
the result can be achieved.

I hope this has been an interesting and informative introduction to Macros in
Elixir. There is so much more to macros and I am still learning myself, but
hopefully I have tickled your curiosity, so that next time magic things occur,
you might want to look into the source code and figure out what is really 
happening under the hood.

Ciao.
