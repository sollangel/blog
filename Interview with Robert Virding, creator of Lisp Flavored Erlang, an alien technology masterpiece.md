# Interview with Robert Virding, creator of Lisp Flavored Erlang, an alien technology masterpiece

![](https://miro.medium.com/max/360/1*0_N8_5MqdROjW5duRb6Dpw.png?q=20)

As you might know zombies, skeletons and momies are good friends of aliens

This time I interviewed [Robert Virding](https://twitter.com/rvirding), co-creator of Erlang and creator of [Lisp Flavored Erlang](http://lfe.io/) (LFE). I am an Erlang developer and Lisp fan — if you are learning Clojure check out my post [How to earn your Clojure white belt](https://medium.com/this-is-not-a-monad-tutorial/how-to-earn-your-clojure-white-belt-7e7db68a71e5#.dtmcog9gk) — so logically I am very excited about LFE.
Reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen) if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial/). **Stay tuned!**
                                                . . .
**Why did you create LFE?**

I discovered and learnt Lisp long before we started working with Erlang, and have always loved it. But I also like the Erlang language (I don’t have problems with the syntax :-)) and how it can build systems. My goal was to make a lisp which was a “real” lisp which builds systems in the Erlang way. Hence LFE.

**What is the LFE philosophy?**

LFE is a proper lisp based on the features and limitations of the Erlang VM, which coexists seamlessly with vanilla Erlang and OTP and runs on the standard Erlang VM.

**In your talk called** [**“About Language Design”**](https://www.youtube.com/watch?v=afLRmoSOnHA) **you said:  
_“_**_People complain about the Erlang libraries and once thing they complain very rightly about the Elang libraries is they’re inconsistent, the naming conventions is inconsistent, the argument ordering is inconsistent, everything is inconsistent about them and that is correct. They are right and people complain about that._**_”  
_At some point, will you create a new standard library in LFE?**

I would like to but there are some deep problems trying to do that. Basically you can’t really change any library module that is used by OTP without the effect propagating and going viral in OTP. Adding new libraries yes, modifying old modules not really. Elixir got around this by having their special module aliases which map onto module name ‘Elixir.XXX’ but then you end up with 2 module naming conventions. I preferred to keep the same names.

[**Common Lisp macros and functions**](https://github.com/rvirding/lfe/blob/dev-macro/src/cl.lfe) **have been added to** [**LFE**](https://github.com/rvirding/lfe/blob/dev-macro/src/cl.lfe)**.** [**Clojure macros and functions**](https://github.com/lfex/clj/issues/18) **are also available as a separate library. LFE follows more traditional LISPs like Common Lisp, Scheme or more modern Lisps like Clojure?**

LFE more has the feel of CL and Scheme, especially CL as it is a lisp-2 not a lisp-1 like Scheme. Clojure is definitely interesting but I felt that the way it does concurrency doesn’t really map well onto Erlang and the style of building systems feels different. `Clojure feels more like language with concurrency while Erlang feels more like a operating system with a language.`
