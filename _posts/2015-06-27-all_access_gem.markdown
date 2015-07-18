---
layout: post
title:  "all_access gem"
date:   2015-06-27 -3:15
categories: Ruby Gems Object-Oriented-Programming
---
In vanilla Rails, if you're being lazy about using decorators or helper methods ( or think that they will actually add complexity instead of remove it ), you can end up with a controller that looks like this

~~~ ruby
  def show
    @thing1 = object.thing1
    @thing2 = object.thing2
    @thing3 = anotherObject.thing3
    ...
  end
~~~

It looks like crap and it means you have a view logic in your controller ( your alternative is the template... ). Your ready for some sort of decorator/bonafide View.

Problem is, you really just want to instantiate all those instance variables! It works just fine!

Solution: Make class does exactly what the controller method does, but at least its in another file
~~~ ruby
  def show
    @decorator = MyDecorator.new
  end
  #in my_decorator.rb
  class MyDecorator
    attr_reader :thing1, :thing2, :thing3 ...
  
    def initialize
      @thing1 = object.thing1
      @thing1 = object.thing1
      @thing2 = object.thing2
      @thing3 = anotherObject.thing3
      ...
    end
  end
~~~

This looks sort of like an ActiveModel::Serializer file.

Another problem, this is more code, and I had to type attr_reader which makes my fake, anemic class look like it is a real class, and its just not true, plus I had to type more.

Goal: get rid of necessisity of attr_reader call.

Okay so maybe this isn't a real problem, but I've always enjoyed being able to access properties of javascript objects without thought, so, with a little metaprogramming, I want to make those instance variables accessible without having to make my pathetic little class look like a real class.

Here's the code:
~~~ ruby

module AllAccess
  module Initializer
    def initialize
      super
      create_readers
      create_writers
    end
  
    def ivar_strings_and_symbols
      instance_variables.map do |iv|
        [iv.to_s.gsub("@",'').to_sym, iv.to_s]
      end
    end
    
    def create_readers
      ivar_strings_and_symbols.each do |iv|
        define_singleton_method iv[0].to_sym, lambda { eval(iv[1]) }
      end
    end

    def create_writers
      ivar_strings_and_symbols.each do |iv|
        define_singleton_method "#{iv[0]}=".to_sym, lambda {|assignment|
          if assignment.class == String
           eval "#{iv[1]} = '#{assignment}'"
          else
           eval "#{iv[1]} = #{assignment}"
          end
          }
      end
    end
  end
  
  def self.included(klass)
    klass.send :prepend, Initializer
  end
end
~~~
A couple of interesting things came out of this basically pointless endeavour. Probably the most likeley to be useful was the last few lines, gained from this [StackOverflow Answer](http://stackoverflow.com/a/17498039/3504261) in which we see how to hook into include call of a class that includes your module, as well as to monkeypatch the initialize method. The argument to included is the "includer" and self is the "includee". We tell the "includer" to #prepend the internal Initializer model, which means that the "includer"'s methods get defined first and won't overwrite the "includees" methods. So the module monkey patches the "includer" rather than the more common case of the "includer" monkey patching some method from the module.

Also interesting was the actual creating of the readers and writers. the create_readers method is pretty nice and fairly self explanatory. We get all instance variables from Object#instance_variables, and crate a tuple with the symbol version of the ivar and also the "@#{name}" version.
We pair those up using define_singleton_method and are pretty much doing the same thing attr_reader does without explicitly naming all of the instance variables. The writer version is the same with a slight hiccup for assigning a string because of some twists in how #eval unquotes its arguments.

In reflection, I am certainly all for explicitness, especially in [frameworks](http://www.phoenixframework.org/). However, it can be misleading. Sometimes something just isn't very important, and its better to be casual like JavaScript. This class is not trying to say anything at all about its attributes, has no pretenses about privacy. Its just a bag to hold things, and I want in.

the gem is [here](https://github.com/crododile/AllAccess)
