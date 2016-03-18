---
layout: post
title:  "looping in Elixir"
date:   2016-03-17 -3:15
categories: Elixir Functional-Programming Recursion Beginners
---

Elixir is a functional langauge, "for" and "while" loops don't really exist.

Examine a for loop in JavaScript:

~~~javascript
function lameLoopingCountdown (times) {
  for (var i = times; i > 0; i = i - 1 ){
    console.log(i)
  }
  console.log("boom")
}
~~~

This loop makes two assumptions about mutability:

1. i is mutable
    The loop ends when it is no longer true that i > 0.
    This assumes that i is currently greater than 0, and it will not be greater than 0 at some point in the future.
    Otherwise, the loop would go on forever.

2. i is mutable
    i is being decremented by 1 every time around the loop.
    This is of course only possible if i is mutable.

Okay it really assumed one thing, but in two ways.
   
How then shall we loop? recursion

~~~elixir
defmodule BeginnersRecursion do
  def radRecursiveCountdown(times) do
    if times <= 0 do                   #base case, or middle chunk of for loop head, end of loop
      IO.inspect "boom"
    else
      IO.inspect(times)                # operation, or body of for loop
      radRecursiveCountdown(times - 1) # the mechanism of looping
    end
  end
end
~~~

JavaScript can do the above, but Elixir offers a
nice way to clean up recursion with multiple function heads.

~~~elixir
defmodule BeginnersRecursion do
  def radRecursiveCountdown(0) do
    IO.inspect "boom"                   # base case, end of loop
  end

  def radRecursiveCountdown(times) do
    IO.inspect(times)                   # operation, or body of for loop
    radRecursiveCountdown(times - 1)    # the mechanism of looping
  end
end
~~~

TLDR, To transform a loop into a multi-headed recursive Elixir function:

  1. extract termination condition of loop into a function head that executes your base case ( i.e. what happens when i is no longer > 0 )
  2. create another function head to perform the body of the loop
  3. end the body of the loop with a recursive call, passing the result of the incrementing operation ( i.e. i - 1 ) as the argument.

