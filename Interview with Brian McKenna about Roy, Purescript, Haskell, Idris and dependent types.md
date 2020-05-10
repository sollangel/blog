# Interview with Brian McKenna about Roy, Purescript, Haskell, Idris and dependent types
[![unbalancedparentheses](https://miro.medium.com/fit/c/96/96/2*p2NbnNI4sEc75QvzOZ1gaA.jpeg)](https://notamonadtutorial.com/@federicocarrone?source=post_page-----63bb1289ea3d----------------------)
[unbalancedparentheses](https://notamonadtutorial.com/@federicocarrone?source=post_page-----63bb1289ea3d----------------------)


[Aug 26, 2015](https://notamonadtutorial.com/interview-with-brian-mckenna-about-roy-purescript-haskell-idris-and-dependent-types-63bb1289ea3d?source=post_page-----63bb1289ea3d----------------------) · 6 min read

As promised in our [last interview](https://medium.com/this-is-not-a-monad-tutorial/eric-merritt-erlang-and-distributed-systems-expert-gives-his-views-on-beam-languages-hindley-a09b15f53a2f) for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial) we interviewed [Brian McKenna](https://github.com/puffnfresh), creator of the [Roy](http://roy.brianmckenna.org/) programming language. In this interview Brian talks about Roy, its implementation, how it compares with Purescript, and also about dependent types and other interesting technologies like Morte and Unison. I highly recommend that you check [Brian’s blog](http://brianmckenna.org/blog/) after you finish reading this interview.

In the following weeks we will be talking with the creators of [Red programming language](http://www.red-lang.org/), Robert Virding — Erlang co-inventor and [Lisp Flavored Erlang](http://lfe.io/) creator — and with [MirageOS unikernel](https://mirage.io/) dev team. Stay tuned! Reach me via twitter at [@unbalancedparen](https://twitter.com/unbalancedparen), if you have any comment or request.

![](https://miro.medium.com/max/500/1*vYf7TCGE19Gni5ssKXQaHA.png)

**What are Roy’s main features?**

Roy featured things common to languages which are well suited to typed  
functional programming:

-   Types and type inference (with row polymorphism)
-   Algebraic data types and pattern matching
-   Monadic syntax

**Why did you create it?**

I created Roy after doing years of JavaScript work. I learned Haskell  
and it made a huge amount of sense to me. Functional programming with  
types seemed like the ideal way for me to work on software and so I  
wanted to bring it to web programming.

**I am tempted to say that Roy has some big similarities with** [**Purescript**](http://www.purescript.org/)**. What are the main differences between the two languages?**

Roy was first!

PureScript had almost the exact same goals as Roy but had a much  
better implementation. Quoting the PureScript wiki:

>Roy is probably the most similar language on the list, and was a  
>large influence on the development of PureScript.

I stopped working on Roy once PureScript got around equal in  
functionality. PureScript’s implementation means it’s a lot easier to  
work on and has much fewer bugs.

Originally the PureScript FFI started off being very different (i.e.  
it used inline JS instead of an externs file) but recently changed to  
being similar to the original design Roy had. That was probably the  
biggest difference the projects had.

**You implemented the lexer and the parser. Did you do so to learn/play or because you thought that the available tools were not good enough?**

I implemented a whitespace sensitive lexer which was passed into the  
Jison parser generator. It was the same approach which CoffeeScript  
took.

A custom lexer was necessary because the [Jison](https://medium.com/this-is-not-a-monad-tutorial/interview-with-brian-mckenna-about-roy-purescript-haskell-idris-and-dependent-types-63bb1289ea3d) lexer generator was not  
capable of stateful scanning of whitespace.

I prefer working with parser combinators such as Parsec and Trifecta.  
They’re easier to work with and allow you to write reusable  
abstractions.

**Why did you use a whitespace sensitive grammar a la Python/Haskell in Roy?**

Just to look like Haskell. I hate thinking about syntax — I copy  
things as much as I can. I’m also annoyed that we’re still writing  
programs using text. They’re trees, not lists of characters!

**Why is it useful to have monadic sugar in a language that has unrestricted side effects?**

Roy was not designed to allow side-effects, but you could do anything  
with the FFI and so it was possible.

Scala has (pretty limited) monadic sugar and also unrestricted  
side-effects. People use monadic sugar for things as straight-forward  
as Option:

    for {  
      name <- maybeFirstName  
      user <- lookup(name)  
      avatar <- getAvatar(user)  
    } yield avatar

Which can be easier to type than:

    maybeFirstName.flatMap { name =>  
        lookup(name).flatMap { user =>  
           getAvatar(user)  
        }  
     }


It doesn’t have anything to do with side-effects.

**From what we could read in the Damas-Hindley-Milner type inference algorithm source comments it is based on Robert Smallshire’s Python code. Do you have any recommendation for those who want to implement it?**

I recommend reading “Generalizing Hindley-Milner Type Inference  
Algorithms” which turns the algorithm into explicit separate  
constraint collection and constraint solving problems, which is a bit  
easier to work with than plain Algorithm W. It’s then possible to do  
tree annotation via the cofree comonad, [which gives a pretty nice  
implementation](http://brianmckenna.org/blog/type_annotation_cofree).

**What reading material do you recommend for implementing your first programming language?**

Source code of other languages are great resources. I think the  
PureScript and Idris compilers are somewhat easy to play around with.

Hackage has a lot of code to look at, such as reference  
implementations of Algorithm W:

[https://hackage.haskell.org/packages/](https://hackage.haskell.org/packages/)

**What would you recommend us to read to learn about different type systems?**

[Types and Programming Languages](https://www.cis.upenn.edu/~bcpierce/tapl/) by Benjamin Pierce is a brilliant  
resource for understanding both. It’s surprised me by being a very  
good introduction to types and having pretty good reference  
implementations in OCaml for each of the discussed type-systems. Very  
highly recommended.

**Have you tried** [**Elm**](http://elm-lang.org/)**? What do you think of** [**FRP**](https://en.wikipedia.org/wiki/Functional_reactive_programming)**?**

I think Functional Reactive Programming could be generalised a bit to  
just general stream combinator libraries and we could make it as  
simple as libraries such as pipes, machines and scalaz-stream. I’d  
like to implement one of those libraries in PureScript and try to  
write combinators to fill in the gaps for developing user interfaces  
to see how close we get to current FRP libraries.

**You recently gave a talk about** [**Idris**](http://www.idris-lang.org/)**, a language with** [**dependent types**](https://en.wikipedia.org/wiki/Dependent_type)**. Could you explain what are dependent types?**

>**["Idris: Practical Dependent Types with Practical Examples" by Brian McKenna](https://www.youtube.com/watch?v=4i7KrG1Afbk&feature=youtu.be).**
In non-dependently typed languages we have a way of making values  
depend upon other values: functions! In dependent typed languages we  
have those functions, but also functions which can return types.

Haskell has two completely separate languages: the language of values  
and the language of types. Recent versions of GHC give a way of  
promoting certain values up to types, but if you want to write a  
function over those values, you’ll have to write a version at the  
value level and separately at the type level.

Dependent-types remove the separation. If you write a function, you  
can reuse it for terms at any level.

**What are the practical benefits of using a language with dependent types?**

Since you can use values in your types, you can specify a lot more.  
For example, I can specify that list reversal is an involution, i.e.  
reversing twice is the same as not reversing at all:

   reverseInvolution : (xs : List a) -> reverse (reverse xs) = r


