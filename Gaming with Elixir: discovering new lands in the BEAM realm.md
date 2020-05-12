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
Also, we had the following goals:

1.  High level of fault tolerance  
    a. Because game teams write “scripts” we did not want them to be able to bring the node down because they wrote bad code  
    b. If the game server goes down this is VERY bad for game developers. Users will get mad quickly and just download a different game with a very slim chance they may come back  
    c. User Acquisition (UA) is expensive. To “buy” a user (this is done via Ads or Promotions, etc) you can spend up to $30 per person, and if your game server is crashed that money is gone.

2. Easy to scale horizontally

3. Easy to upgrade servers with hot patches without taking the system down

With those three goals BEAM is one of the best tools for the job.

**Lua is a language that is pretty common in the game development community. The Erlang VM has a Lua based language called Luerl. Why did you choose to implement Playground in Elixir instead of Luerl?**

We actually started with Luerl in early 2014 as our scripting language. The problem with Luerl at the time (I have not reviewed it in some years) was its basic architecture didn’t fit our needs. Luerl is designed for you to pass it a bunch of data, process that data in Lua, then get the results back. However if you don’t know what the Lua code will be doing you need to pass in ALL possible data in one big map (and this was pre-Erlang maps so it was a custom struct the Luerl lib created). This meant we had to spend a lot of time modifying the actual Luerl package to extend its basic APIs so the game logic could “ask” our Erlang code for needed data, etc. Around mid/late 2014 Elixir had matured and we felt that the language was easy enough that most folks could pick up the basic syntax without a lot of work. In the end we are really happy that we moved off Luerl. Now our System <-> scripting environment has a near zero overhead (we don’t have to serialize/deserialize struct, etc). In fact we liked Elixir so much we ended up porting all of our Erlang system to it and now we are pretty much an Elixir shop.

Also Elixir made it easy to “Walk” the code AST where we can check it for operations we don’t allow. For example, when a team writes Elixir business logic we don’t allow them to use OTP, or even spawn processes, etc.

**What is the biggest difference between implementing a backend for a video game and a HTTP REST JSON api?**

It all boils down to two things: latency and state.

**State  
**Games are very state-full, and if you had to send all the data needed to finish a REST-based transaction your JSON would be HUGE both upstream and downstream. Games also tend to react to events that the user did not generate so bi-directional communication is a very strong requirement. For example: If player 1 attacks player 2, player 2 will need to be notified of this event and different actions may be triggered if player 2 is online or not. Also, this state may need to be presented to other players, so in the last example player 3 may see that players 1 and 2 are in combat so he/she may jump in and help player 2.

**Latency  
**For a typically ecommerce site a 500ms latency may be completely unnoticeable. However, in a game, 500 ms could be the difference between winning and losing. So we are constantly concerned with latency. This includes things like time spent serializing/deserializing your communications, how much data you are sending up and down the wire, how long you spent accessing the DB, etc, etc.

**What are the main difference between implementing actual games and game development tools?**

It is the same as developing any tools/lib vs a product that goes directly to consumers. With tools you need to think about not just what the tool does but also you have to think about how others will use it. Does it have the flexibility to work in N different use cases, etc. With a game you know exactly how your product should behave and can quickly tell when things work or they don’t. With a tool it may work great with the designed use case, but some other creative engineer decided to repurpose it in a completely different way. As a tools/lib developer your challenge is to have the flexibility to support both.

**What was your experience using** [**Riak Core**](https://github.com/basho/riak_core)**? What did you like and what did you not?**

Riak Core is a very cool library but really you have to look at it from two angles.

1.  Do I buy into the whole Consistency Hashing thing?
2.  Do I want to implement my own solution for this?

About two years ago we said yes to the first question, however getting Riak Core to work in an elixir environment was just too painful so we rolled our own solution. It worked well and was actually a bit faster than Riak Core, however it was not nearly as robust and did not handle node failures and handoffs well. Recently the Phoenix and Basho got a Hex package that actually works in Elixir and we jumped at the chance to try it out. After some evaluation we decided that the little bit of performance gains we got from our system was outweighed by the expanded functionality that Riak Core provided.

**Liked**

-   Very robust software that handles a lot of error cases for us.
-   Built in gossip protocol
-   Handles data handoffs when new nodes are brought online

**Dislike**

-   Dependencies that don’t work because Basho bases all their libs/tools on Erlang R15
-   Very heavy weight
-   Difficult to track down errors deep in the system code

**Is there anything you miss from implementing actual videogames?**

Working on videogames is a truly unique experience. Not only are you working with very smart engineers, you also get to work closely with Artists, Game Designers, Musicians, and sometimes even Actors. That is a very fun and exciting environment to be in.

[No rights reserved by the author.](http://creativecommons.org/publicdomain/zero/1.0/)

   . . . 
-   [Erlang](https://notamonadtutorial.com/tagged/erlang)
-   [Elixir](https://notamonadtutorial.com/tagged/elixir)
-   [Game Development](https://notamonadtutorial.com/tagged/game-development)
