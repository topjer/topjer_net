+++
date = '2025-09-17T20:45:48+02:00'
draft = true
title = 'Python garbage collection does not work like you think!'
+++

At least that was the case for me.

While I was learning about profiling of Python applications - make sure to subscribe if you want to read about that - I ended up looking into memory profiling as well.

Having the tools at hand, the following thougt came to mind: "Wouldn't it be cool if I would disable garbage collection and could see a constant increase in memory usage?" What happened next, surprised me.

But, let us start at the beginning.

# What is garbage collection

When it comes to Memory Management (which involves allocation, utilization and deallocation), there are different strategies programming languages employ.

Languages like C and C++ put all responsibility in the hands of the developers. Memory must be allocated and deallocated manually. While this gives fine grained control over memory usage, this can also esily lead to memory leaks or dangling pointers, i.e. references to a memory location that has already been freed.

Other languages like Java or Python take an automated approach. The so-called garbage collector is responsible for identifying and freeing memory once no longer in use.
This makes the life of the developer easier, because they no longer have to concern themselves with manual memory management, but it can lead to unexpected pauses in program execution.

A unique approach is used in Rust. Where a strict set of rules ensures memory safety at compile time.



# Notes

- introduction
- quick look at what garbage collection is in Python but omit details for the surprise
- show code of function in question
- establish gc package
