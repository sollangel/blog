# Efene: an Erlang VM language that embraces the Zen of Python


![](https://miro.medium.com/max/352/1*Y6aaUiw3qoh372nY7WNzJg.png?q=20)

In this ocasion we interviewed [Mariano Guerra](https://github.com/marianoguerra), creator of Efene. [Efene](http://efene.org/) is _“an alternative syntax for the Erlang Programming Language focusing on simplicity, consistency, ease of use and programmer UX”._ After reading the interview with Mariano Guerra, check [Efene Quick Introduction for the Busy/Lazy Programmer](http://efene.org/quick-efene-introduction-busy-programmer.html) for learning more about Efene.
Reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen) if you have any comment or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial). **Stay tuned!**

**Why did you create efene?**

I learn by doing, and a while ago I wanted to learn Erlang, it was the first functional programming language I wanted to learn coming from C, C++, ASM, Java and Python so I was looking for some toy project to learn it.

For a while I couldn’t find a project that was interesting to me and also matched the strengths of Erlang at some point I decided that I would do a small calculator in Erlang, you can see the first commit [here](https://github.com/marianoguerra/match/commit/cc048638b4cc99719ad5c28cea2f9e8163b9661c) and the full project [here](https://github.com/marianoguerra/match).

At first I was doing all the eval stuff myself but pretty quickly I added support to compile the expression to an Erlang [module with a function](https://github.com/marianoguerra/match/commit/6c726f641e5d651f6bb46b2ae04202e557ea022b). The next commit added function support and then I realized there was a programming language there, you can read the rest of the commits to see how it morphed into one.

At that point the only other beam language other than Erlang was [Reia](http://reia-lang.org/). I wasn’t planning anything in particular with my powerful calculator/language hybrid, but at some point people from Erlang Factory asked me if I wanted to give a talk about my language and of course I said yes, then I got a dose of impostor syndrome, so I started the project from scratch to do a proper programming language and I decided to support every feature that Erlang supports and not much more. At that point the project changed from a toy to an actual programming language.

After [my talk](http://www.erlang-factory.com/conference/London2010/speakers/MarianoGuerra) and some initial excitement things got quiet. I just had got my Engineers degree and had a new job so development stopped for a while, then Elixir appeared and got much more attention, so I thought “OK, someone got it right, I will just stop pushing efene” and some years passed. But then looking into Elixir I saw that the Elixir ideas weren’t exactly the ideas of efene and I decided to rewrite it to try to fill the niche of “just a different syntax for Erlang, reuse as much as possible from the Erlang ecosystem, unified tooling and documentation as the core of the project”. The language has been complete for a while now. I’m just working on documentation, rebar3 plugins andwaiting for some of the surrounding tools to mature to avoid having to redo the documentation (mainly [rebar3](https://github.com/rebar/rebar3) and [cowboy 2](https://www.youtube.com/watch?v=YGuAXS0Cy_8)).

**Why do you embrace the** [**Zen of Python**](https://www.python.org/dev/peps/pep-0020/)**?**

![](https://miro.medium.com/max/560/1*6_ijRzr0oB6Zckr8GTrq4A.png?q=20)Zen of Python
