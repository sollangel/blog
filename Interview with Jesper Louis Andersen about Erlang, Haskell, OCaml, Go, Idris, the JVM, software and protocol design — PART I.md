# Interview with Jesper Louis Andersen about Erlang, Haskell, OCaml, Go, Idris, the JVM, software and protocol design — PART I


In this occasion we interviewed Jesper Louis Andersen, a type theorist with lot of practical knowledge and experience. His [blog](https://medium.com/@jlouis666) (you should also check his old [blog](http://jlouisramblings.blogspot.com/), you will find a lot of good things over there) and code (specially things like [safetyvalve](https://github.com/jlouis/safetyvalve) and [fuse](https://github.com/jlouis/fuse)) have been a big inspiration to me. That is why I published the interview in two parts: I had too many questions for him. I hope you enjoy reading his answers as much as I did.

![](https://miro.medium.com/max/560/1*mIrYVuSZtaYe3WJkRwUqwQ.jpeg?q=20)


Reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen) if you have any comment or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial). **Stay tuned, tomorrow I will publish part II of this interview.**

PD: I just published part II of the interview. [Check it out!](https://medium.com/this-is-not-a-monad-tutorial/interview-with-jesper-louis-andersen-about-erlang-haskell-ocaml-go-idris-the-jvm-software-and-5628fe591295)

                                                    . . .

**Your reply to the picture that is below was:**

> *The typical applications I write in Erlang have 3–4 functions above it in the call stack*

**Modern comfortable programming language #java:**

![](https://miro.medium.com/max/904/0*Ee-kDajNT561ZgOd.)

**Why is it so? Why this is not the case in most OOP languages?**

It is not really an artifact of OOP-style languages as much as it is an artifact of how we develop software. In biology, it has been observed solutions are usually not rewriting code, but rather patching code. Imagine a world where we are more inclined to build on top of what we already have rather than go down and rewrite older parts. I think each new generation of programmers tend to add a layer on top of what we already have, mostly to put their mark on programming and to simplify and abstract the parts of the lower levels which are in common use while also hiding the rare parts. What you get is the deep call stack. Another similar view would be in geology where you can see older layers and go back in time to older periods. Much of the Java stack has this in it.

Erlang is no different, but from the outset, small-community languages are less susceptible to patching. Especially if it is easy to rewrite code, which means an itch can be scratched by writing something new, rather than building on top of what others did. What brings the call stack down to 3–4 frames, however, are processes. In an Erlang-system a typical web request is served by cooperation of 3–4 processes, each of those having a call stack. Add them all together, and you approach the above size, but in isolation, they are small. An eventual error will result in a crash report with 3 things in it: the backtrace, the state _before_ the process crashed and the incoming message which made it crash. The reason we have the original state is because of functional programming: every data structure is persistent, so we can version it. Usually this is enough to quickly isolate and figure out where the error is and what went wrong.

**You tweeted:**

> *“Lets talk agile: The ultimate agile language is one in which you can deliver working code quickly, which is also maintainable. Hence there are only a few agile languages in existence: Haskell, Ocaml and Erlang are 3. Go, Javascript, Python and the rest lacks the necessary abstractions to be regarded as agile. You end up having to recode. However, the **real** solution is to stop doing agile. The idea is bullshit in the first place.”*

**Why Haskell, OCaml and Erlang are the only agile languages?**

If the waterfall model risks not building the right product, then agile risks not building the product right. I’m very fond of Erlang creator Mike Williams point:` “If you don’t make experiments before starting a project, then your whole project will be an experiment”`. My hunch is what a lot of Agile process misses is that you need to experiment before you build.
If we instead ask about prototyping, then we need a programming language with certain traits. The team is usually small, so we need an expressive language, and we need to address the core kernel of the system in isolation, first. We don’t need a lot of interfacing to foreign systems and in general we won’t care too much if the system we build is fast. Also, we usually won’t need to operate the prototype in production, since it is simply a proof of concept.

From a perspective of rapid prototyping and proof-of-concept development, functional languages tend to have an edge over imperative ones. Their higher level and expressivity allows more to be said succinctly, in fewer lines of code. They also tend to describe data structures and algorithms in ways that are clearer, which helps understanding of the problem space.

In turn, since agile values the minimum viable product, moving fast and making experiments, then you need languages in which it is easy to experiment with the unknown ideas many agile projects face. In languages such as Haskell, Erlang and OCaml you can often iterate over far more designs in a limited time window. Thus you can carry out more experiments and this often leads to a better product, even if the final product is not even written in a functional language. In general I feel we value experimentation too little. Build it, throw it away, and rewrite with the new knowledge.

**What do you think about Clojure?**

Sadly, my experience with Clojure is limited to carrying out a set of Koans in the language, and I have not ever used it for anything serious work. I must admit I don’t find the language especially compelling, and in general I don’t find Lisp-dialects interesting. My language roots are closer to Standard ML, which may be the reason it did not catch my interest when I finally tried toying with Scheme and Common Lisp.

That said, people are doing lots of outright _amazing_ stuff in Clojure. I think the Datomic project is genuinely interesting as a database system. And the work Kyle Kingsbury has done with his “Jepsen” framework and his “Knossos” linearizability checker in Clojure is solid work. I’m also following David Nolen’s work with interest. But I’m pretty sure the language isn’t “for me”.


