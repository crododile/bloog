---
layout: post
title:  "K&R C Review"
date:   2015-06-15 -3:15
categories: C Classics Book-Review
---
<h2>Thoughts on K and R C</h2>
This book was recommended by Ned Ruggeri of App Academy as a good foundational book to continue with after the course. I've been wanting to get my hands on this one for about a year, and finally bought it after I have finished reading Metaprogramming Elixir by Chris McCord. I'm a little trepidatious to write a review of such an old and well known book. Keep in mind that this is coming from my perspective, and could be valuable for someone of a similar perspective as myself. I'm sure much has been said of this book that I could not dispute, but this is the thoughts of a Rails developer in 2015 who has not had cause to write any actual C.

I don't feel like this was necessarily the highest and best use of my time. It was not a very pleasurable read, and many of the concepts are so fundamental that there isn't much to discuss. This probably shouldn't have come as a surprise, as higher level languages were created for a reason. The code examples of various sorting algorithms were kind of interesting, but the book did not "blow my mind" at any point with an elegant solution to a problem I'd seen before ( such as the Fibonacci's sequence in Programming Elixir ) or give me much that I can use in day to day. I read this book as a preparation for Ruby Under a Microscope because I thought I would get more out of that book if I understood C. I have not started back into Ruby under a Microscope, so perhaps this review will be revisited with much happiness when I've had a chance to apply the knowledge in this book.

As might be expected, pointers and memory management were the most interesting and challenging parts of this book, and I did find C's handling of arrays and array indexing pretty elegant. Basically, when referring to a C array at all, are actually referring to the memory location of the first element in the array. Indexing is done by adding to the pointer the index of the array multiplied by the size of each item in the array. The array in this case must be kept in a continuous block of memory.   
