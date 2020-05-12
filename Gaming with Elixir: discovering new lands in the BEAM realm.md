# Gaming with Elixir: discovering new lands in the BEAM real

In this opportunity I interviewed somebody I don’t normally interview: a client, [Chris Jimison](https://www.linkedin.com/in/chrisjimison/), CTO of Merigo. One of my clients. After working for almost a year with Merigo, I appreciate and understand the differences between using a BEAM language to develop a typical REST JSON system and using it to develop a videogame backend. I am not aware of many companies using Elixir to develop backends. I have only watch a talk by Jamie Winsor called “[Building And Releasing A Massively Multiplayer Online Game”](https://youtu.be/_i6n-eWiVn4). I hope to be able to interview Jamie in the following weeks.
To sum up I did this interview because I wanted to share my experience of using Elixir for developing a different kind of beast. I am also writting a post about what I like and dislike about Elixir after developing in Erlang for quite a few years.

Reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen) if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial/). I am about to post another group of interviews about [Lasp,](https://github.com/lasp-lang/lasp) PostgreSQL and Rust. **Stay tuned!**

_Discuss and vote at_ [_lobsters_](https://lobste.rs/s/f1wxuc/gaming_with_elixir_discovering_new_lands)_,_ [_reddit_](https://www.reddit.com/r/programming/comments/66hnpk/gaming_with_elixir_discovering_new_lands_in_the/) _and_ [_hn_](https://news.ycombinator.com/item?id=14156379)_._

![](https://miro.medium.com/max/560/1*3HjK0CX5813XXtDwfKIhWw.jpeg?q=20)

**Could you describe your experience in the software industry?**
I started in the software industry about 15 years ago. My career has been focused in the game development industry, specializing in networking problems. This includes games that have a peer-to-peer deterministic game play model, all the way to big MMORPG titles. In the last 7 years I have been working on mobile titles where the game networking requirements are more of a hybrid of REST and MMORPG-style technologies. Some companies I have worked for in the past include EA, Sony R&D, NGMOCO and DeNA.

**What is** [**Merigo**](http://merigo.co/)**?**

About 3 and a half years ago my cofounders and I decided we wanted to start our own company to help game developers be more successful in the market. One of the big issues that has come around in the last 4–5 years is single player games just don’t make that much money in the market place (outside of the occasional indie hit). As mobile game developers move into the “Online Gaming” space, the costs of creating these applications are increasing exponentially. When the iPhone 3G first launched (this was the first model that had the App Store) many game teams consisted of one or two engineers — a “large” game having 3–5 engineers. We are now seeing team sizes of anywhere between 10–20 and have even heard of some titles consisting of over 30 engineers. This means costs of titles are skyrocketing and the need to “fail fast” is more critical. “Fail fast” gets to the heart of game development. A game designer comes up with an idea they “think” will be fun. However you don’t truly know until you have it in your hands to play. Some ideas work, some don’t. With such a high development cost, many game teams must choose to implement a feature and hope that it works because they don’t have the budget to try a second or third time. Merigo wants teams spending more time “focusing on the fun” and less time worrying about how to make it all work.

**What is SDE?**

The Merigo SDE (Server Development Environment) is a system that allows developers to build out their server logic quickly without having to worry about all the pain of how to scale out and manage their stack. We provide many common game services such as authentication, purchase verification, leaderboards, player persistence, etc. However the SDE is not a bunch of black box REST APIs. The SDE enables developers to write custom “scripts” in Elixir that can be loaded/updated at runtime, allowing them to create custom APIs and behaviors needed for their application. Since we have provided the basic framework of how the scripts are managed and when they are called we can now “scale” this logic out across a distributed server cluster. Our main philosophy with the SDE is “servers are cheap, people are expensive”. With the SDE we wanted to provide a system that allows for fast development and rapid iteration with the knowledge that you can always spin up new servers to balance the load, even if your business logic is not optimized for high performance. Each team can then decide where they want to invest their time once the game goes “live.” They can decide where to best invest their time: building new features or optimizing their code to reduce their server costs. Basically turning development into a simple Return on Investment (ROI) equation.

**What languages/platforms did you consider or try before settling on BEAM/Elixir?**

Java, Scala, Go and for one crazy day I even debated just writing the whole stack in C++.

**What made you choose the Erlang VM (BEAM) instead of the JVM?**

I hate writing Java… The real reason we didn’t go with a JVM environment is tuning it is kind of a black art and something I have not done in years (since Java 1.4).
