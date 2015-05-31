---
layout: post
title:  "Elixir v Ruby Pt. 1"
date:   2015-05-30 09:35:42
categories: Elixir Ruby Functional-Programming Object-Oriented-Programming
---

<h1>Exploring Elixir Pipes in Ruby</h1>

Elixir has a Unix-like pipe operator to ferry method results through a series of transformations. It goes something like this

{% highlight elixir %}
   String.upcase("Metova")    ##"METOVA"
     |> String.reverse     ##"AVOTEM"
     |> String.split("")   ##["A","V","O","T","E","M",""]
{% endhighlight %}

In Ruby, we'd do something like this

{% highlight ruby %}
  "Metova".upcase.reverse.split('') ##["A", "V", "O", "T", "E", "M"]
{% endhighlight %}

These look similar at first glance, with Ruby being more sentence-like. There is a key difference, however.

The Ruby methods must be defined on the item being returned from each method call in the chain ( i.e. "String" objects ). Elixir is a functional language, so the data being passed ( the string 'Metova' ) along is simply data. It contains no internal properties other than the string of bytes necessary to spell 'Metova'. It must be passed as an argument to functions defined in the String module. This is worth stating another way. In Ruby method chains, the return value of the previous method is the <em>receiver</em> of the next message in the chain. In Elixir pipes, the return value of the previous function is the <em>argument</em> to the next function in the chain. This is illustrative of a fundamental difference between Functional Programming and Object Oriented Programming.

Let's take a closer look 

{% highlight elixir %}
 upcase_test.exs
  ...
   assert String.upcase("Metova") == "METOVA"
   ## passes
  ,,,
{% endhighlight %}
{% highlight ruby %}
 upcase_test.rb
  ...
   expect{"Metova.upcase"}.to eq "METOVA"
   ## also passes
  ...
{% endhighlight %}

The difference:
  Elixir String module receives the message 'Upcase' with the argument "Metova".
  Ruby string "Metova" receives the message 'upcase'.
  
We'll try to implement the Elixir pipe in Ruby (and see that it's kind of out of line with Ruby's normal Object Orientation).

We'll try to call it like this
{% highlight ruby %}
  "Metova".pipe(:upcase).pipe(:reverse).pipe(:split, "")

# Because we want to use it in any situation, we'll define pipe on Object
# No, I do not advocate monkey patching the Object Class, 
# this is for investigational purposes only

class Object
  def pipe(*args)
    #first argument will be the method we want to send
    #the rest will be any arguments to the method
    meth, otherArgs = args[0], args.drop(1)
    
    #remember, pipe is actually being called on the return value of the previous method call,
    #which we want to pass as the first argument to our quasi-functional method call.
    firstArg = self

    #Method is like the Send's quasi-functional brother. It's pretty cool and you should check it out.
    #http://ruby-doc.org/core-2.2.0/Method.html (It's listed under meth in the docs for some reason )
    otherArgs.empty? ? firstArg.method(meth).call : firstArg.method(meth).call(*otherArgs)
  end
end

p "Metova".pipe(:upcase).pipe(:reverse).pipe(:split, "")
# ["A", "V", "O", "T", "E", "M"]
{% endhighlight %}

In the end, we've gained basically nothing with this. After going around in a circle, we're still sending the same methods to the same returned object. The fact is, Object Orientation is baked into the Ruby language. Ruby core methods ( and other good Ruby ) will often return a value of the same class as the original method recipient, or even the receiver itself ( self ), perhaps transformed. Method chaining is an extremely useful result of this pattern. 

We've discovered that the hard-baked Object Orientation of the Ruby Core will not allow us to create a version of 'pipe' that is more useful than method chaining, at least for using the Ruby Core. Still though, we'll try again to create a version of pipe that doesn't amount a less readable version of method chaining. In part 2.