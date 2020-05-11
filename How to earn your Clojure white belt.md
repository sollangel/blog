# How to earn your Clojure white belt


![](https://miro.medium.com/max/560/1*uFlthCHz1YbmOvzhdVBrFw.jpeg?q=20)

I tend to think that if I do not know how to implement something then I do not undestand completely how it works. That is why I want to learn so many things: new frameworks, programming languages, garbage collectors, type systems, compilers, editors, operating systems, protocols, kernel development. The road to enlightenment is long and difficult. To make things worse, you need to invest time on keeping up to date with technologies you thought you already knew. I usually feel like Mario in one of those levels where he needs to jump between moving clouds to reach the finish line.
That is why one of the best skills to get is being able to separate the wheat from the chaff. We have finite time and distractions are infinite. If you are going to invest time in learning a new language then you should consider Lisp which is definitely wheat of the best quality. If you don’t know why, then go read [Beating the Averages](http://www.paulgraham.com/avg.html) article. Clojure is a member of the Lisp family. If you have used a Lisp dialect such as [Scheme](http://www.schemers.org/), [Common Lisp](https://common-lisp.net/), [Emacs Lisp](https://en.wikipedia.org/wiki/Emacs_Lisp), or even really alien and amazing creatures such [Shen](http://www.shenlanguage.org/), [LFE](http://lfe.io/), [Racket](http://racket-lang.org/), [Pixie](https://github.com/pixie-lang/pixie) you might ask yourself why you should be learning Clojure. This is my opinionated point of view:

-   The language’s creator: Rich Hickey has a holistic understanding of this era development issues. You should read/watch his talks called [Are We There Yet?](https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/AreWeThereYet.md), [Simple Made Easy](https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/SimpleMadeEasy.md), [The Language of the System](https://youtu.be/ROor6_NGIWU). I see Clojure as a step in the right direction for tackling this era’s development issues
-   Great functional programming support: First class functions, persistent, immutable data structures and lazy [sequences](http://www.braveclojure.com/core-functions-in-depth/#2__The_Sequence_Abstraction). It has great support of polimorphism thanks to [multimethods and protocols](http://clojure-doc.org/articles/language/polymorphism.html)
-   Concurrency support from its inception thanks to [atoms, agents, refs, a software transactional memory](http://clojure-doc.org/articles/language/concurrency_and_parallelism.html) system and asynchronous programming using channels a la [CSP](https://en.wikipedia.org/wiki/Communicating_sequential_processes) with [core.async](http://clojure.com/blog/2013/06/28/clojure-core-async-channels.html) library
-   Batteries included: good core libraries and [transducers](https://www.youtube.com/watch?v=6mTbuzafcII) support
-   Clojurescript: Clojure that targets JavaScript with great libraries such as [reagent](https://github.com/reagent-project/reagent), an interface to React.js that is really simple and useful. Check David Nolen’s talks called ClojureScript: [Lisp’s Revenge](https://youtu.be/MTawgp3SKy8) and [Introduction to ClojureScript](https://youtu.be/-I5ldi2aJTI)
-   JVM’s power (available almost everyhwere, optimized garbage collector, JIT compilation, Java interoperability), but also some issues such as not being able to properly support tail call optimization and awful stacktraces. If you are a Java developer you should check part [I](https://www.youtube.com/watch?v=P76Vbsk_3J0) and [II](https://www.youtube.com/watch?v=hb3rurFxrZ8) from Rich Hickey’s Clojure for Java Programmers talk
# Ready, steady, go!

Install [Leiningen](http://leiningen.org/#install) and configure your editor of choice.

-   Emacs with [Cider](https://github.com/clojure-emacs/cider) [Clojure mode](https://github.com/clojure-emacs/clojure-mode) and [Company mode](http://company-mode.github.io)
-   Vim with [vim-fireplace](https://github.com/tpope/vim-fireplace), [im-sexp-mappings-for-regular-people](https://github.com/tpope/vim-sexp-mappings-for-regular-people), [vim-salve](https://github.com/tpope/vim-salve), [rainbow](https://github.com/luochen1990/rainbow)
-   [Light Table](http://lighttable.com/)
-   [Nightcode](https://sekao.net/nightcode/)
-   Sublime with [paredit](https://github.com/odyssomay/paredit), [Lispindent](https://github.com/odyssomay/sublime-lispindent) and [SublimeREPL](https://www.google.com.ar/search?q=SublimeREPL)
-   [Cursive](https://cursiveclojure.com/) (IntelliJ)
-   [Counterclockwise](http://doc.ccw-ide.org/documentation.html#install-as-standalone-product) (Eclipse)

## Good tools that deserve a mention

-   Linter [Eastwood](https://github.com/jonase/eastwood)
-   Idiomatic Clojure checker [kibit](https://github.com/jonase/kibit)
-   Clojure formatter [cljfmt](https://github.com/weavejester/cljfmt)
-   Check simple issues in your code [bikeshed](https://github.com/dakrone/lein-bikeshed)
