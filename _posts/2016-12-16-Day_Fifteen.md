---
layout: post
title: 8th Light apprenticeship - Day Fifteen-fun with processes
categories: 8thLight apprenticeship
---

I think processes in Elixir are pretty cool. Elixir compiles to a format that runs on the Erlang
virtual machine, so Elixir and Erlang processes are pretty much the same -I say, with two weeks 
experience on Elixir and 40 minutes on Erlang, so please take this
with a pinch of salt!-. This week I was trying to explain what they are to a fellow
apprentice at 8th Light and came up with an example that I liked, so I thought I
would write about this. I will be using Elixir's interactive shell. If you have
Elixir installed (it is easy to do if not!) you can access it by typing _iex_ in
your terminal. 

Here is my mental model of processes.

![elixir processes](/assets/elixir_processes_1.jpg)

I imagine the Erlang virtual machine as an initially desolate world, where processes can
come to life, generate other processes, and die. Not only does the virtual machine allow processes to come alive and stay 
that way, but it also acts as a communication bus between them. But what are processes?
A process is any self-contained piece of code running on the virtual machine. 
Let's make a super-simple example.

{% highlight elixir %}
  defmodule Foo do

    def add_one(number) do
      number + 1
    end

  end

  IO.puts add_one(5)
{% endhighlight %}

The code above defines a function that adds 1 to a number and then calls the 
function with 5 and prints the result.
If you were to save this in a file, say _foo.ex_ and then
run it with _elixir foo.ex_, Elixir would compile the file, then start the Erlang
virtual machine, spawn the process (this is jargon, read: _make the process come
alive_), which prints the number on the console and then dies. The virtual machine assigns a
unique ID to each process. This is so that processes can be unambiguously referred to, 
much like in an addressing system. If you know the address of a process you can 
send a message to it, as we will see later on. Each process knows its own address;
to prove this, add the following line at the end of _foo.ex_:

{% highlight elixir %}
  IO.inspect self()
{% endhighlight %}

This will print out the ID assigned to the process. Cool!

Next, as I mentioned, the Erlang virtual machine allows
processes to communicate with each other. Refer to my sketch above; each 
process can send as well as receive messages. Let's build an Echo server to 
explore message sending/receiving in the interactive shell.

First, a couple of interesting things. The interactive shell running is a process itself. We
can prove that using the function _self_, which returns the subject's process ID.
Also, we can check if the process is alive with the function _Process.alive?(ID)_

{% highlight elixir %}
  iex()> id = self()
    # => #PID<0.80.0>
  iex()> Process.alive?(id)
    # => true
{% endhighlight %}

Now let's save our Echo module in a file called _echo.ex_:

{% highlight elixir %}
  defmodule Echo do

    def start do
      receive do
        {sender, message} -> send(sender, "Reply from echo: " <> message)
      end
    end

  end
{% endhighlight %}

A process is always spawned from a function; in this case, we define _start_ as 
the function that we will use for spawning the process. You can see that inside 
_start_ we have a _receive_ block, where we defined what to do with an incoming 
message, namely prepending "Reply from echo: " to the message and sending it back
to the sender.

OK, let's try this. First import the module into the iex session. 

{% highlight elixir %}
  iex()> c "echo.ex"
    # => [Echo]
{% endhighlight %}

Then we want to spawn an echo process. Let's spawn two for good measure!

{% highlight elixir %}
  iex()> echo_one = spawn(%Echo.start/0)
    # => #PID<0.90.0>
  iex()> echo_two = spawn(%Echo.start/0)
    # => #PID<0.133.0>
{% endhighlight %}

Now let's send a message to each echo process. The _send_ function returns the 
message sent, which in our case is the shell's process ID and the actual message.

{% highlight elixir %}
  iex()> send(echo_one, {self(), "ciao"}
    # => {#PID<0.80.0>, "ciao"}
  iex()> send(echo_one, {self(), "hola"}
    # => {#PID<0.80.0>, "hola"}
{% endhighlight %}

Notice that nothing happened. This is because the echo process sent the message
back to our shell session, but we have not defined what to do when a message is received. 
Luckily processes store all incoming messages for future consumption. 
You can check a process' message queue with _Process.info(PID, :messages)_. Let's
do that.

{% highlight elixir %}
  iex()> Process.info(self(), :messages)
    # => {:messages, ["Reply from echo: ciao", "Reply from echo: hola"]} 
{% endhighlight %}

Nice, so we can see the two echo processes did respond with an echo. Let's define
what to do with these messages now. 

{% highlight elixir %}
  iex()> receive do
  iex()>   message -> IO.puts message
  iex()> end
    # Reply from echo: ciao
    # => :ok
{% endhighlight %}

If you do that again, the second message gets printed, and the message queue is 
now clear.

{% highlight elixir %}
  iex()> Process.info(self(), :messages)
    # => {:messages, []}
{% endhighlight %}

OK, pretty cool so far. See below a sketch of what we just did.

![elixir processes](/assets/elixir_processes_2.jpg)

Now, if you wanted to do this again, you couldn't, as both
echo processes are dead. You can still send the message, but nothing would happen
as the destination process is not there to receive it. Let's check.

{% highlight elixir %}
  iex()> Process.alive?(echo_one)
    # => false
  iex()> Process.alive?(echo_two)
    # => false
{% endhighlight %}

Why is that? Recall the _start_ function in the Echo server. This simply defines
what to do when a message is received. When you start the process, the _receive_
block waits for a message to arrive, then sends a response, and finally dies as 
it has nothing else to do. 

If we wanted to keep the process alive, we could simply call the _start_ 
function again to repeat...Hello recursion! This pattern is very common in Erlang/Elixir. A process
is set on an infinite loop, which pauses at the _receive_ block and waits for a 
message. Upon receiving a message it deals with it and then repeats. 
If you are thinking an infinite loop is bad, do not worry: processes 
can be killed from other processes.
Let's update the Echo module in _echo.ex_, so it stays alive.

{% highlight elixir %}
  defmodule Echo do

    def start do
      receive do
        {sender, message} -> 
          send(sender, "Reply from echo: " <> message)
          start()
      end
    end

  end
{% endhighlight %}

Let's see if that worked:

{% highlight elixir %}
  iex()> c "echo.ex"
    # => [Echo]
  iex()> echo = spawn(&Echo.start/0)
    # => #PID<0.117.0>
  iex()> send echo, {self(), "one"}
    # => {#PID<0.80.0>, "one"}
  iex()> send echo, {self(), "two"}
    # => {#PID<0.80.0>, "two"}
  iex()> send echo, {self(), "three"}
    # => {#PID<0.80.0>, "three"}
  iex()> Process.info(self(),:messages)
    # => {:messages, ["Reply from echo: one", "Reply from echo: two",                                                                   │
    #                 "Reply from echo: three"]}
  iex()> Process.alive?(echo)
    # => true
{% endhighlight %}

Hurray, it did work!!

Now, explaining why processes are useful is beyond the scope of this post, however here are a couple of
thoughts. The first one is fault-tolerance. An application might consist of a plethora
of processes. These are fully independent and, should one crash, it will not affect
the rest of the application. For example, compare a monolithic web app, where an 
exception thrown in the mailer might take the whole thing down, as opposed to one built with 
processes, where a crashed mailing process does not affect the rest of the app. 

Another cool thing is scalability: this is possible both vertically and horizontally.
With a rising requirement in processes handled, one can upgrade the computer on
which the Erlang virtual machine is running. This is vertical scaling.
In alternative, the Erlang virtual machine allows you to send messages between
processes over a network, so that an application can run across multiple computers
with each process unaware of where it is running, as it does not matter where a 
process is, all it matters is knowing its ID and the Erlang virtual machine will
handle delivery. This is comparable to a data network: you ping your own computer in
the same way you ping another computer, as you do not worry about *how* the message
gets there, only *who* the message is for. This is horizontal scaling.

Ciao.
