# A Pythonist finds a new home at Clojure land

Welcome back to another interview of [Not a Monad Tutorial](https://notamonadtutorial.com/). In this opportunity I decided to interview Facundo Olano, a friend, a teammate, and a developer with a [diverse experience](https://github.com/facundoolano). We talked about Python, Node.js, Clojure, Common Lisp and software development in general. Something worth mentioning is that this is not the first time Python and Clojure get mentioned together at Not a Monad Tutorial. [Timothy Baldridge implemented Pixie](https://notamonadtutorial.com/indie-languages-interview-pixie-and-timothy-baldridge-cadbc36418dc), a Lisp language inspired by Clojure, in Python.
I hope you like the interview with Facundo as much as I did. If you are looking for a battle proven developers like Facundo reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen). Also contact me if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial/). **Stay tuned!**

Vote and discuss at [lobsters](https://lobste.rs/s/nz28ef/pythonist_finds_new_home_at_clojure_land), [reddit](https://www.reddit.com/r/programming/comments/65ct5j/a_pythonist_finds_a_new_home_at_clojure_land/), [hn](https://news.ycombinator.com/item?id=14114624).

|![](https://miro.medium.com/max/1160/1*4XmtTGqxc82DyQwcpmr9pw.jpeg?q=20)|*A wallpaper I created for my OpenBSD laptop. Lisp, BSD, Erlang, Rust. We live in the best of all possible worlds.*|
|--|--|

**You are quite in love with Python. Why is that?**

Heh, love is a big word, isn’t it? But, yes, I guess for a while I had pretty strong feelings for Python. I still have, to a degree. If you’re going with the love metaphor let’s say I had a series of shitty girlfriends that didn’t treat me all that well and I didn’t even noticed it, then I meet this great woman, this caring and interesting woman that listens to what I have to say and doesn’t, um, force me to wrap everything in class.

Seriously speaking, I learnt to program in college, arguably at an old age, and there they taught me Pascal (because some folks are convinced that’s still the best way to teach good programming manners), then a fair amount of C++, and lots of Java. Everybody was in love with Java over there and they kind of gave you the impression that that was it: this is as good as programming gets, this is what will get you a job and it’s so much better than C++, which I already knew wasn’t pleasant to work with. So I got a job programming Java and I loved it, I read all the books, _Refactoring_ and the Design Patterns stuff. But then I had to quit because I was getting behind in College, I got some free time back and I was curious about Python because some former coworkers and other students talked heavens about it.

I got this book _Learning Python_ and went through it top to bottom one summer. I guess I felt scammed, in a way: how come nobody told me about this? I felt I had been wasting my time writing XMLs and type hierarchies, while there was a simpler way to do about everything: dynamic typing gave me polymorphism for free, the data structures were built-in and had literals, string manipulation was just amazingly easy, you could have standalone functions and pass them like values, a lot of the Java design patterns were reduced to one-liners… I guess one of the take-aways of the experience was that sometimes your peers know better than your teachers (or your bosses).

But besides it feeling better than Java, what’s still unique for me about Python is the set of principles that are best expressed in the _Zen of Python_. It felt like every bit of the language followed those principles and it encouraged you to do the same with your own code; when in doubt about how to tackle something, the Zen of Python would tell you the right way to go. I still follow most of those ideas when I program, regardless of the language.

**Before learning Clojure, your** [**first experience with a Lisp**](https://facundoolano.wordpress.com/2012/01/31/first-impressions-on-common-lisp/) **was with Common Lisp. From what I have talked with you about learning Common Lisp I could sum up that it was a bittersweet experience. Why didn’t you like Common Lisp?**

I got interested in Lisp after reading Paul Graham essays. Then there was that bit of Eric Raymond about the religious experience of _getting_ Lisp. After what I went through with Java and Python I made a point about always looking for chances to learn new languages.

But it just didn’t feel right, in a lot of ways it was the opposite of what I loved about Python: the code was very difficult to read (not because parens or prefix notation, just because it was filled with symbols), the operator set was huge, with really weird names and not dynamic at all, I remember it having like six equality operators for different types. Add to that it’s an old language with a small community and zero chances of getting a job using it, it made no sense to invest much time in it, since it wasn’t fun either. But fortunately there are many Lisps and I already knew right then I’d give a chance to Clojure eventually.

**Why was the experience with** [**Clojure different**](https://facundoolano.wordpress.com/2016/03/20/first-impressions-on-clojure/)**? As** [**stesch said in reddit**](https://www.reddit.com/r/programming/comments/65ct5j/a_pythonist_finds_a_new_home_at_clojure_land/dg97mao/)**, Clojure land is in the Java sea. Those are strange waters for a pythonist. What did you like about it?**

For starters, it addresses everything that put me off about Common Lisp. It’s more readable. The operator set is big, but very consistent and polymorphic: all core functions work as expected with every data type. It has a strong focus on immutability and functional programming, which CL hadn’t. But it’s also a very pragmatic language: it doesn’t ask you to learn category theory or to know what a functor is in order to get why you would benefit from it. That’s the approach to functional programming that I like. I’m not saying theory isn’t important, just that I’m the kind of programmer whose interest you won’t catch with theory detached from practice. Also, the community is very active and welcoming, and the fact that it’s a JVM language makes it get a lot of attention, considering it’s a Lisp dialect.

Another thing that made me feel at home about Clojure is that it has a strong philosophy and you can tell its design has been driven by it. The philosophy is not the same as the one in Python, and now I understand that that doesn’t really matter. I want consistently opinionated languages rather than bags of features you can bend to program in ten different ways; _what_ the philosophy is is secondary to the fact that _there is_ a philosophy.

The talks by Rich Hickey are enlightening, and every time I watch one I feel like I improved my understanding of the language, even if the talk isn’t about Clojure itself. Kind of like a Zen of Clojure.

**What do you think about the JVM and its community?**

I’ve been working to suppress the strong feelings I used to have against Java. I still dislike it as a language and the way of programming it encourages, though. I revisited it a couple of years ago to write some Android apps and nope, still not my cup of tea. Everything just feels convoluted and over engineered, I’d constantly think how much simpler this is in Python or Perl or Ruby or JavaScript. It’s a very programmer-centric view, I’m aware, mostly based in what I enjoy in the day to day, which doesn’t necessarily make sense from a business perspective. There’s a reason why big companies go to Java; I recognize there’s a lot of top of the class software written in it, and it’s fast. I don’t have the background to make a serious assessment of the JVM but I’m pretty sure it’s an amazing piece of software.

The good thing is the JVM can now host other languages, and something as weird as a functional Lisp dialect is fairly close to being popular and you can even get a job with it. That’s a lot to say.

**What do you miss from Python?**

Sometimes I say, half-joking, that programming is a branch of literature. In that sense I value elegance and succinctness, I think there can be beauty in code, but I also know that beauty is completely subjective and I can’t argue about one style being better than another. I’m not talking about readability, that’s important and it’s more or less measureable; I’m talking about aesthetics. I think Python had that kind of beauty for me, which is harder to get in Clojure. Clojure can be more expressive and it’s more powerful but can easily get ugly if you don’t have a lot of discipline. Again: all subjective stuff.

Truth be told, I don’t think I miss Python all that much, at least not while I’m doing Clojure. JavaScript is a different story, JavaScript is a mess. But still, if you bend it in the right directions, it can be a fairly decent functional language. I noticed that Python can’t, there’s stuff that just doesn’t work that way (lambdas come to mind), and that probably would annoy me nowadays. So I guess I’m not married to any language anymore. That’s a good thing, right?

**Your most known projects in Github are written in JavaScript. Do you like coding in JavaScript?**

JavaScript is Frankenstein. As Douglas Crockford showed in _The Good Parts_ years ago, there’s a lot of awful bits and you _have_ to subset the language. That’s gotten worse, because they keep adding stuff to it (some of it really cool, some of it to make it look like Java), and they can’t remove the old stuff, so it has become one of those things you have to agree upfront on which parts you’re going to use and which parts are banned, kind of like C++.

But, as said, if you pick the right subset it can be good. [eslint](http://eslint.org/) helps. If you ban _this_ and _new_ and treat objects like maps, most problems go away. If you’re bold you can even pick a set of eslint rules to force immutability. And then there’s [Ramda](http://ramdajs.com). That’s the secret sauce; it makes data manipulation a joy, in some spots I even like its functions better than their Clojure counterparts. The async stuff is weird. I still can’t make my mind if Promises are better than callbacks, but I got used to them. It’s a lousy way of hiding concurrency, though.

I have a fair amount of Node.js projects in GitHub, yes. That’s the killer thing about Node.js: NPM, its ecosystem, this philosophy of small unix-y modules. You have an idea, you write up a file and you’re two commands away from publishing it and getting feedback. No other language I’ve tried reduces the boilerplate to share your work that much. That’s a real boost for Open Source and Collaboration. I recognize it has some bad side effects (mixed module quality, left-pad, etc.), but those are much less than the advantages. I remember Python dependency and publishing story being way more cumbersome.

**In the last few months you started using Emacs. What do you think about it?**

I’ve been putting off a blog post about that. I know I’m glad that I decided not to learn Emacs at the same time that I was learning Clojure. That could have caused me to drop both things. I think there’s this idea that you can’t learn a Lisp without Emacs, and that may have been true before, but now that there’s [parinfer](https://shaunlebron.github.io/parinfer/) you can safely hack Lisp on most modern editors. Hardcore lispers may not notice how amazing of a contribution to the community this is, it makes it dramatically easier to get started with Lisp and reachable to people that wouldn’t even consider using something like Emacs.

I pleasantly used Clojure with Sublime and parinfer for almost a year. Then I started reading _Coders at work_ and saw all these amazing hackers mentioning Emacs again and again. It felt like I was missing out on something, so I decided I should give it a try, as a weekend project.

And it was a hell of weekend. The first lesson I learned was that I was a shitty typist, but that got better as the days went by. Fortunately it was a quiet week at work because I could barely get anything done for a while. One interesting note is that I went through this process with JavaScript, so I could make Emacs my daily editor at work. It wasn’t until I felt comfortable with the editor that I tried it with Clojure. And then I realised my Clojure experience wasn’t complete before. It probably goes the other way around: you get the best out of Emacs when you’re doing Lisp.

I’m not sure I’d recommend it to every programmer, though, specially if they work on a single language that already has a killer editor. Learning Emacs is a really fun and enriching experience, but you have to be prepared to spend a lot of time tuning the editor at first and acknowledge the fact that _your Emacs config will be the project of your life_, as I read somewhere.

I know I love those times when you realise you are doing something repeatedly, that could be done by a command, and you write it (or, better, you find that someone else wrote it already). That’s when it pays off.

**Is there any particular Clojure library that you would specially recommend?**

[kibit](https://github.com/jonase/kibit) I find amazing because it’s not only a useful linter (like [eastwood](https://github.com/jonase/eastwood/) also is), but it teaches you the standard library as you go, which can be a little bit overwhelming when you are starting out. [Figwheel](https://github.com/bhauman/lein-figwheel) is another obvious one that made a big difference for me when I started to play with ClojureScript.

The [Macchiato framework](https://macchiato-framework.github.io/) is doing a great job of bringing Node.js to ClojureScript.  I noticed some people don’t take Node.js very seriously, but being Clojure a language that makes such a strong point of leveraging the host platform, the world of possibilities available in NPM can’t be disregarded. There’s another often overlooked fact: you’re much more likely to find a functional programming enthusiast nowadays doing Node.js than in the Java world, so shortening that gap can only bring good to the community.

[Joker](https://github.com/candid82/joker) is another great one. It’s a Clojure interpreter written in Go, but what I’ve found incredibly useful is how it works as a linter, which you can easily hook to an editor. It really improved my workflow, early catching typos and bugs while I program.

Finally, in the testing related work I’ve been doing recently I’ve found [lein-test-refresh](https://github.com/jakemcc/lein-test-refresh) and [eftest](https://github.com/weavejester/eftest) to be useful and insightful about how clojure.test and leiningen internals work.

**On your github profile you have many pretty well known open source projects. What motivates you to invest time in them?**

I just love programming. I have lots of hobbies, but definitely programming is one of the things that I enjoy the most, what gives me the most pleasure. It’s like an itch: I get this idea or I encounter a need that isn’t addressed by an existing library or maybe I’m just playing to learn a new technology. I can’t help it but sitting down and building something, until is see something working on the screen and the itch goes away. I learned not to be lazy, polishing it a bit and uploading it to GitHub.

Open source is ideal, because you get to work on your own time, you do the stuff you’re interested in and drop the project whenever it bores you. I wish someone would pay me to do that. With a real job it’s harder: sometimes it gets boring, sometimes there’s nothing to do or you depend on someone else to move forward. Most often than not you can’t share your work.

And I always liked this process of conceiving a project, executing it and sharing it with others. Not just in software; the same goes for my fiction writing or when I recorded music as a teenager. When I was in high school I didn’t know how to program but I was obsessed with making games, I spent most of my afternoons fiddling with these game maker programs, RPG Maker and such, sharing my creations with friends. Unfortunately internet access was limited at home and they didn’t teach programming in my school, so it wasn’t until much later that I was able to do serious stuff on my own.

During college I did little projects and games but the languages I knew and skills I had weren’t enough to get me very far at the beginning. There was no GitHub, so it was harder to share and find interesting projects to work on. Later I had to juggle between a full-time job and finishing college. It wasn’t until these last couple of years that I got the freedom to work on whatever I want. And I know in a couple of years I’ll have kids and other stuff going on in my life, so I’m trying to enjoy this programming spring as much as I can.

And then there’s marketing factor. I’m not great at some types of job interviews and my resume may not tell you that much on its own, but I have a portfolio of open source projects that shows I’m a serious coder, a published Android game that proves I can take a personal project to production and a blog that tells you that I reason about my craft and I’m constantly trying to improve it. If you care enough to read it.

**What are you working on lately?**

I work on a Node.js shop, but the team is full of functional enthusiasts. We can’t convince our boss to let us write a production microservice in Clojure or Elixir, or a user interface in Elm. But the boss doesn’t care that much about our integration tests (as long as there _are_ tests). So when someone complained for the zillionth time about how painful it is to write our API integration tests in JavaScript, I proposed migrating them to Clojure. To my surprise they agreed. So I hacked together this little library to easily do all that we were already doing in our Node.js API tests. And I liked how it turned out, so I’ve [published it in GitHub](https://github.com/facundoolano/restpect). Nothing too fancy but it was a good excuse to escape from JavaScript, and it got three of my coworkers learning Clojure, which is great news.

Other than that, I keep slowly growing my [advenjure engine](https://github.com/facundoolano/advenjure); just [published a full game](https://facundoolano.github.io/house-taken) with it, based on a story by Julio Cortázar. Also been doing some [experiments with Macchiato](https://cljsbin-bkhgroqzwe.now.sh/), to write Clojure web apps that run on Node.js. I have this Machine Learning book I’m about to start, see where that takes me. Node.js microservices still pay my bills. For now, I guess. At some point I hope to be able to get paid for writing Clojure.

[No rights reserved by the author. ](http://creativecommons.org/publicdomain/zero/1.0/)

   . . .
-   [Clojure](https://notamonadtutorial.com/tagged/clojure)
-   [Programming](https://notamonadtutorial.com/tagged/programming)
-   [Python](https://notamonadtutorial.com/tagged/python)
-   [Nodejs](https://notamonadtutorial.com/tagged/nodejs)
