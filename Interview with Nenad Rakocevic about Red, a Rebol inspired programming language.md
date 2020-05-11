# Interview with Nenad Rakocevic about Red, a Rebol inspired programming language

[![unbalancedparentheses](https://miro.medium.com/fit/c/96/96/2*p2NbnNI4sEc75QvzOZ1gaA.jpeg)](https://notamonadtutorial.com/@federicocarrone?source=post_page-----681133e3fd1c----------------------)
[unbalancedparentheses](https://notamonadtutorial.com/@federicocarrone?source=post_page-----681133e3fd1c----------------------)
[Aug 28, 2015](https://notamonadtutorial.com/interview-with-nenad-rakocevic-about-red-a-rebol-inspired-programming-language-681133e3fd1c?source=post_page-----681133e3fd1c----------------------) · 9 min read

After our [last interview with Brian McKenna](https://medium.com/this-is-not-a-monad-tutorial/interview-with-brian-mckenna-about-roy-purescript-haskell-idris-and-dependent-types-63bb1289ea3d) for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial) we interviewed [Nenad Rakocevic](https://github.com/dockimbel), creator of the [Red](http://www.red-lang.org/) programming language.

![](https://miro.medium.com/max/220/1*C0sMUPxpbBs40YIBX848jQ.jpeg?q=220)

From my completely subjective point of view Red and Rebol are quite strange creatures! But don’t get me wrong, that doesn’t mean something bad. For example, I am not aware of many high-level languages which features an embedded DSL for general-purpose low-level programming or that have 50 native types. You should check it out, you might find some interesting ideas inside Red development.

Reach me via twitter at @unbalancedparen if you have any comment or interview request for This is not a Monad tutorial. Stay tuned!

Please tell us a little bit about Red’s inception. Why was it created?

![](https://miro.medium.com/max/130/1*869fRKUVs2sTdVDYrqblMQ.png)
I started programming micro-computers, an Amiga in my case, in my teens. I have now been programming for more than 30 years. After my early experiences, I was unhappy with existing programming languages and tools. This was mostly because I found them not productive or friendly enough for my taste. So, when I stumbled across the Rebol language, in 1999, it was an eye-opener on what was wrong with so called “modern” computing practice. (Nowadays it’s even worse). Fighting complexity on all software fronts became the logical course of action.
In 2010, Rebol was closed source. I deeply felt that the approach had a lot more to offer but Rebol was barely evolving. This was the trigger for me to start work on an open source relative to Rebol with much higher ambitions and a broader field of usage.

**What are the main selling points of Red?**

-   First fullstack programming solution: combines in one tool, the ability to write high-level code (GUI apps, scripting and DSL) and fast low-level code (writing device drivers, operating systems, native interfacing, etc). Moreover, Red is also a _both-sided_ technology (client & server).
-   Cross-platform native code compiler: from any platform the toolchain runs on, you can compile to about 15 other platforms, with a simple command-line option (-t Windows, -t Linux, -t Darwin, -t RPi, …).
-   Extremely lightweight: Red is a 1MB, single-file, no install, no setup, toolchain. It takes typically a few seconds to download and you can immediatly start writing and running code, there’s *nothing* to setup (it’s just terrible that this is the exception instead of being the norm…).
-   Batteries-included solution: it comes with a very rich runtime library, despite its tiny size, covering pretty much anything you need for common tasks.
-   DSL-oriented environment: Red comes with many embedded DSL addressing important needs (like GUI or system-programming). DSL are a very powerful way to reduce complexity arising from frameworks or API, while drastically increasing productivity. Red includes a DSL (called Parse) for constructing DSLs.
-   Red (like Rebol) is a Lisp derivative, but with a human-friendly syntax (no parenthesis hell). Red is its own data format. All code is treated as data until you evaluate it, code/data serialization comes for free. The minimal punctuation makes it easy on the eye.
-   The underlying philosophy of Red (as was that of Rebol) is to make the simple easy and the difficult possible.
**What made Rebol the main inspiration for Red?**

Rebol is one of the most innovative programming language created in the last 20 years. Sadly, it remained under the radar being closed source at a time when open-source languages like Perl, Python and Ruby hit the streets. Rebol’s approach shakes the foundation of what programmers consider “simple” or “efficient” in programming. Typically, API which other languages would call “simple”, look uselessly complicated when you are used to wear Rebol glasses. ;-) Here are a few one-liners as examples (using the Rebol2 REPL):

Create a GUI window with a button printing, on click, Hello in the console:

    view layout [button “Click Me” [print “Hello”]]
Dump the content of a web page in console:

    print read [http://rebol.com](http://rebol.com/)
 Extract the title of a web page:

    parse read [http://rebol.com](http://rebol.com/) [thru <title> copy text to </title> (print text)]
  Send the list of files in current folder by email:

    send [user@domain.com](mailto:user@domain.com) mold read %.
 
 Retrieve records from a MySQL database and print them in console:

    foreach row read/custom mysql://root@localhost/books [“SELECT * FROM authors”] [print row]

Notice that even if you never looked at Rebol code before, you can nonetheless read it and guess what most of the code is doing.
