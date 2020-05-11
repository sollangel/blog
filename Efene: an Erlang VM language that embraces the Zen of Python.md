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

![](https://miro.medium.com/max/560/1*6_ijRzr0oB6Zckr8GTrq4A.png?q=20)
Zen of Python

Python was the first language I enjoyed coding in before I coded in C, C++, ASM and Java, but just because it was what I knew or they provided something I needed. With Python it was the first time I said “I’m a Python programmer” and not “I’m a programmer”, also the Python Argentina community helped a lot witht hat.

Python has this attitude of simplicity and community that I like and instead of coming up with an “ad hoc, informally-specified, bug-ridden, slow implementation of half of the python zen” I decided just to copy it.

David Nolen summarized it well the other day:

That’s why efene is a mixture of what I like about the languages, communities and philosophies of Python, Javascript and Erlang, don’t expect a lot of novelty in efene, just a remix of what’s there :).

>## marianoguerra/ioriofn
>[marianoguerra/ioriofn](https://github.com/marianoguerra/ioriofn/)
>### ioriofn - ioriodb client and tests in efene
>#### github.com

If your reaction is “I understand this and this is boring”, then I would be happy :), of course knowing some Erlang will help the understanding since efene semantics and patterns are the same as Erlang’s.

**Which are the biggest advantages of coding in a language that runs on top of the Erlang VM (BEAM)?**

The semantics of the VM are really thought out and really simple to learn.

The stability and scalability of the platform is great and there’s a lot of people that have worked on really hard problems for a long time on top of the Erlang VM, this means you can get really good advice and help from them.

One thing I really like and I don’t think is mentioned that much is the level of runtime introspection and visibility the VM has, and the tooling that is build and can be built around it is great.

**What difficulties did you find in implementing efene?**

Learning the limits of the parser and what syntax is valid an unambiguous, learning to avoid introducing crazy ideas into the language because syntax and semantics are always tricky and you don’t want to have a “WAT” language.

Also learning about Erlang and its VM while doing it.

But to sum it up, it ended up not being as difficult as I thought it will be, it just requires persistence and some hammock-driven development ;)

**Do you have any recommendation for those of us that did not implemented any language yet?**

Learn about lexing and parsing, then build a calculator using S-Expressions (Lisp-like) or reverse-polish syntax (Forth-like).

Start it as an interpreter, copy the semantics from a simple language you already know, coming up with good semantics is hard, don’t try to invent them the first time.

Then ride on top of a language you know, either transpile to that language or compile to bytecode or some intermediate representation.

Try to reuse as much of the tooling from the other language as possible ([AST from Erlang](http://www.erlang.org/doc/apps/erts/absform.html), [AST from Python](https://docs.python.org/3/library/ast.html) or similar), this will allow you to reuse all the tooling and code built around those representations.

Read about Lisps and Forth. Implement a simple Lisp (Scheme) or Forth.

Once you learn to lex and parse you can think syntax for languages and try to parse them.

**What is the match expression and why did you introduce it?**

At the core of efene rewrite was the concept of “Everything revolves around 4 main things, pattern matching, functions, guards and data”, pattern matching is done when using the equal sign (=), on the argument list of a function definition and on other Erlang expressions. I wanted to unify the pattern matching under a single syntax and reuse it everywhere, that’s where the “case clauses” came to be.

If you haven’t look at efene yet, the shape of efene expressions is something like:

    <keyword> [<expr-args>]  
        <case-clauses>  
        [else: <body>]  
    end

A case-clause has this shape:

    case <case-args> [when <guards>]: <body>

For example try/catch:

    try  
       <body>  
    catch  
       <case-clauses>  
       [else: <body>]  
    [after <body>]  
    end
  Receive:

    receive  
         <case-clauses>  
         [else: <body>]  
    [after <after-expr>: <body>]  
    end

Functions:

    fn [<name>]  
        <case-clauses>  
        [else: <body>]  
    end
You should see a pattern there, since the case keyword was already taken and it’s what Erlang use for what “match” does in efene I had to look for a new keyword.

One thing I like about python is this concept of “executable pseudocode”, I like the fact that if you read Python code aloud it sounds like what it does, so I thought “what am I doing here”, “I’m matching and expression against cases”, in imperative it would be “match A [against] case B, case C, else … end” and that’s how I ended up with match.

**Why did you introduce a _for_ expression?**

The initial idea for efene was to be familiar for people coming from “algol-like” or “mainstream” languages, so they can focus on learning what’s interesting about Erlang which are the semantics and the abstractions and avoid learning a new syntax on the way to epiphany.

Since list comprehensions aren’t available in many of those languages but “for” is, I decided to implement list comprehensions as a more familiar construct but in fact it does the same.

**What is the arrow operator and why did you add it?**

First a quick introduction for people unfamiliar with efene or the arrow operator.

There’s this thing in Erlang where if you want to apply a sequence of operations to a list you have to create a new binding for each intermediate result:

    MyList = create_list(),  
    MyList1 = op1(MyList),  
    MyList2 = op2(MyList1),  
    MyList3 = op3(MyList2),  
    MyList4 = op4(MyList3).
