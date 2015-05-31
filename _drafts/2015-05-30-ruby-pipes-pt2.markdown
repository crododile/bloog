---
layout: post
title:  "Elixir v Ruby Pt. 2"
date:   2015-05-30 -3:15
categories: Elixir Ruby Functional-Programming Object-Oriented-Programming
---
 In [part 1]({% post_url 2015-05-30-ruby-pipes-pt1 %}) of this post, we saw that Object Oriented Programming is deeply embedded in the Ruby language. We saw that elixir style pipes and ruby method chaining are largely incompatible solutions to the same problem, and Ruby favors method chaining.
 
 Be not dismayed, I am determined to force this Functional peg into an Object Oriented Hole.
 
 The aim is to have the returned value from the prepipe function be passed as the argument to the post pipe function. Because all functions live as attached to Objects, we'll need to pass in the actual Objects that are ultimately doing the calling, as well as the method name.

In short, we want some syntactic sugar for this pattern:
{% highlight ruby %}
#turn this...
result1 = foo(result0)
result2 = bar(result1)
result3 = baz(result2)

#into this
result1.pipe2("foo").pipe2("bar").pipe2("baz")
{% endhighlight %}

The question that arises is, if the result1, and the subsequent return values are not the receiver of the function call (foo, bar, baz), who is? In Elixir, the arguments to 'pipe' calls are either fully qualified function names or ones shortened by the 'using' macro.

We are left with the task of defining some context to which our arguments to pipe2 will be bound. Who will be the receiver of these method calls? Candidates are:

1. The item itself; this results in 'shitty method chaining' [see part 1]({{ site.baseurl }}{% post_url 2015-05-30-ruby-pipes-pt1 %})
2. Allowing the argument to contain the context (a fully qualified function call)

The second item above is what Elixir does, so let's try to do that.

What are the consequences of defining the receiver of a Ruby method outside of a direct method call? The implication is that we will not require access to or mutate the data owned by the receiver, or else we would have invoked the method in the ordinary manner. 
