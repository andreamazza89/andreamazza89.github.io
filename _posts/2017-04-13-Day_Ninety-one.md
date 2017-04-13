---
layout: post
title: 8th Light apprenticeship - Day Ninety-one
categories: 8thLight apprenticeship
---

Today, among other things, I have been pondering about dependency injection in
functional programming.

First, what is dependency injection? I am going to use objects to describe this,
using an example.

A good principle in software design is that of single responsibility: systems that
are made out of tiny components with a clear and concise scope are generally better
than ones made of one or few large components with many responsibilities, as these are difficult to reason
about and change.

It is also good to have individual components know as little about
the rest of the system as possible. This way, any change in the system affects
the smallest number of components. The extreme of this principle (components knowing
nothing about each other) is not good: we need
components to know about each other, so they can interact and form an organic
system.

So, we want tiny components that have a clear/concise scope and know about
each other, but only as little as possible.

When a component knows about another component, it _depends_ on it: it needs it to
perform its own tasks. Enough abstraction, let's look at an example.
Let's say we have a class `AirportControlCentre`, which authorises plane take-off
solely based on weather. Now, since we like designing modular systems with tiny
components that have a clear/concise scope, we have a separate component to figure
out what the weather is, and we are going to call it `WeatherStation`.
OK, so the `AirportControlCentre` needs to depend on `WeatherStation` for plane
dispatch; we could summarise this dependency as

```
AirportControlCentre --depends_on--> WeatherStation
```

Now let's implement both classes; to make our life easier, let's pretend that
there are always planes in the airport's queue (only three shown in the example
below).

{% highlight ruby %}

class AirportControlCentre

  def initialize
    @plane_queue = ["plane3", "plane2", "plane1"]
    @weather_station = WeatherStation.new
  end

  def take_off_first_plane_in_queue
    if @weather_provider.ok_to_take_off?
      @plane_queue.pop
      puts "first plane in the queue took off!"
    else
      puts "could not take off due to bad weather :("
    end
  end

end

class WeatherStation

  def ok_to_take_off?
    rand() > 0.3 #this is maybe not the best way to answer the question but it works for my example!
  end

end

{% endhighlight %}

In the code above, when someone 'asks' the airport to take-off the first plane in
the queue, the airport checks with the weather station and only proceeds if it is
safe to fly. The airport's dependency on the weather station is two-fold:

- it knows how to ask the question (`ok_to_take_off?`)
- it knows the exact name of the class (`WeatherStation`)

Now, we do need to know how to ask the question, but could we avoid knowing about
the exact class we ask that question to? It would be good if we could simply ask
the question without knowing whom exactly we are asking. This would be a generic
weather provider, and all the airport cares about is that the weather provider can
answer the question `ok_to_take_off?`.
This is where dependency injection comes into play. Rather than having a direct
dependency between `AirportControlCentre` and `WeatherStation`, we could inject
a generic weather provider into the airport at creation. Let's see that in practice:

{% highlight ruby %}

class AirportControlCentre

  def initialize(weather_provider)
    @plane_queue = ["plane3", "plane2", "plane1"]
    @weather_provider = weather_provider
  end

  def take_off_first_plane_in_queue
    if @weather_provider.ok_to_take_off?
      @plane_queue.pop
      puts "first plane in the queue took off!"
    else
      puts "could not take off due to bad weather :("
    end
  end

end

 #notice: the above method #take_off_first_plane_in_queue includes direct user
 #        interaction, which should ideally be separated from this method, but
 #        I am including it here to make the example concise.

{% endhighlight %}

Cool, suddenly airport's dependency looks like this:

```
AirportControlCentre --depends_on--> any weather provider (an object that responds to ok_to_take_off?)
```

And this:

- it knows how to ask the question (`ok_to_take_off?`)
- ~~it knows the exact name of the class (`WeatherStation`)~~

Right, that's cool in principle, as our `AirportControlCentre` now knows a lot
less about the component it depends on, but let's look at how this can be helpful
in practice.
Now that the weather provider is injected into the airport, we can have different
versions of it, like so:

{% highlight ruby %}

class FairWeatherStation

  def ok_to_take_off?
    rand() > 0.3 #this is maybe not the best way to answer the question but it works for my example!
  end

end

class OverlyOptimisticWeatherStation

  def ok_to_take_off?
    true
  end

end

class GrumpyWeatherStation

  def ok_to_take_off?
    false
  end

end


optimistic_weather_station = OverlyOptimisticWeatherStation.new
grumpy_weather_station = GrumpyWeatherStation.new

my_optimistic_airport = AirportControlCentre.new(optimistic_weather_station)
my_grumpy_airport     = AirportControlCentre.new(grumpy_weather_station)

my_optimistic_airport.take_off_first_plane_in_queue()
 # => "first plane in the queue took off!"  | (always allows taking off)
my_grumpy_airport.take_off_first_plane_in_queue()
 # => "could not take off due to bad weather :("  | (always stops taking off)

{% endhighlight %}

Pretty cool eh? So here are two uses of the newly found power of injecting a weather
provider:

- We have the flexibility to choose a different provider but use the same airport;
for instance, imagine our airport application became so popular that it was used
throughout the world: we would need to use different weather providers depending on location. Without
dependency injection, we would have to rewrite the `AirportControlCentre` class
for each new weather provider, whereas with dependency injection, we always use
the same `AirportControlCentre` but inject different weather providers, depending
on where in the world our application is being used.

- Being able to inject a dependency makes testing much more flexible. Imagine
writing a test for our `AirportControlCentre` without dependency injection: in
order to test the two scenarios (_ok to take off_ and __not__ _ok to take off_) we
would have to somehow manipulate the `rand` function so that it behaves predictably
for each test scenario. In contrast, with dependency injection, we can pass in our
optimistic weather when testing the _ok to take off_ scenarios and the grumpy one
for testing the __not__ _ok to take off_.

So, in summary, dependency injection is good if we are going to use different
components that can answer the same question (in our example we use multiple weather
station that can answer `ok_to_take_off?`). However, even if we are only ever
going to use the one version of our dependency, injecting it can still help with
testing, especially if the component acts unpredictably like in our example, or
is 'expensive' (takes a long time to run) to use.

Notice that I am not advocating using dependency injection everywhere: like all
tools, it is important to understand its value and when it is appropriate to use.

OK, I have been going on for a while now, so I will move onto dependency injection
in functional programming in my next post.

