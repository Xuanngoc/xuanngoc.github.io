---
layout: post
title:  "[JS] Hidden threads in Javascript"
date:   2022-03-20 09:00 +0700
categories: js multithreaded
---
### Is Javascript really single-thread language ?

While your JavaScript code may run, at least by default, in a single-threaded environ‐
ment, that doesn’t mean the process running your code is single-threaded. In fact,
many threads might be used to have that code running smoothly and efficiently. It’s a
common misconception that Node.js is a single-threaded process.

Modern JavaScript engines like **V8** use separate threads to handle garbage collection
and other features that don’t need to happen in line with JavaScript execution. In
addition, the platform runtimes themselves may use additional threads to provide
other features.

In Node.js, **libuv** is used as an OS-independent asynchronous I/O interface, and
since not all system-provided I/O interfaces are asynchronous, it uses a pool of
worker threads to avoid blocking program code when using otherwise-blocking APIs,
such as filesystem APIs. By default, four of these threads are spawned, though this
number is configurable via the **UV_THREADPOOL_SIZE** environment variable, and can
be up to 1,024.

On Linux systems, you can see these extra threads by using top -H on a given pro‐
cess. In *Example*, a simple Node.js web server was started, and the PID was noted
and passed to top. You can see the various V8 and libuv threads add up to seven
threads, including the one that the JavaScript code runs in. You can try this with your
own Node.js programs, and even try changing the **UV_THREADPOOL_SIZE** environment
variable to see the number of threads change.

<img src="{{ "/assets/images/top-show-threads.png" | prepend: site.baseurl | prepend: site.url}}" alt="zigzag" />


*Output from top, showing the threads in a Node.js process*

It’s important to think about these extra threads when going through a resourceplanning exercise for your application. You should never assume that just because JavaScript is single-threaded that only one thread will be used by your JavaScript
application.

---
References:
- Multithreaded JavaScript-Thomas Hunter, Bryan English [9-10]
