# Interview with Brad Chamberlain about a productive parallel programming language called Chapel

As you might know, I am a big fan of concurrency, parallelism and distribution but I know almost nothing about high performance computing (HPC) so I decided to get out from my comfort area. This time I’ve interviewed Brad Chamberlain about [Chapel](https://github.com/chapel-lang/chapel), a productive parallel programming language.

                                             . . .
                                             
 I 've created a Not a Monad Tutorial newsletter so that you receive an email whenever we publish a new story. [Sign up!](https://mailchi.mp/9302d4f60de9/not-a-monad-tutorial)

Reach me via twitter at [@unbalancedparen](https://twitter.com/unbalancedparen) if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial/).

_Discuss and vote at_ [_lobsters_](https://lobste.rs/s/zmqo84/interview_with_brad_chamberlain_about)_,_ [_reddit_](https://www.reddit.com/r/programming/comments/7x2jhp/interview_with_brad_chamberlain_about_a/) _and_ [_hn_](https://news.ycombinator.com/item?id=16360381)_._

![](https://miro.medium.com/max/260/1*Vbk8JH0pBz1gSHSbxV799A.png?q=20)

**What problems does Chapel solve? Who is the ideal user of Chapel?**

Chapel supports scalable parallel programming in a portable way: programs developed on a user’s multicore laptop can be run on commodity clusters, the cloud, and supercomputers from [Cray](https://www.cray.com/) or other vendors. Chapel is also designed to vastly improve the productivity of performance-oriented programming, whether serial or parallel. As such, it supports programs with Python-like clarity while retaining the performance of lower-level approaches to programming like C, C++, Fortran, MPI, and OpenMP (the _de facto_ standards for high-performance parallel programming).

Ideal Chapel users include Python programmers who are interested in parallel or performance-oriented programming as well as parallel programmers who are fed up with conventional approaches.

**Were there any other previous languages that tried to solve the same issue?**

Definitely. There’s a long list of failed attempts at developing scalable parallel programming languages, and skeptics like to remind you of them all the time: “If all of these languages have failed, how will you ever succeed?” In designing Chapel, we spent a lot of time reviewing other parallel languages to learn from their mistakes. Historically, I’d say that most parallel languages have failed for one of two reasons: Many were too high-level, limiting what an expert programmer could explicitly control while requiring a lot from their compilers. Others were lower-level, but as a result didn’t provide sufficient appeal over existing approaches like MPI.

Our response to this tension was to design a language using what we refer to as a _multiresolution_ _philosophy_: Chapel supports higher-level features like parallel loops and distributed arrays for productivity and ease-of-use. Yet, it also supports lower-level features that give programmers more explicit control over the system. Notably, the high-level features are implemented in terms of the lower-level features within Chapel itself. This provides programmers with the ability to extend the language by creating their own abstractions. For example, an advanced Chapel user can implement new work-scheduling policies for their parallel loops, or new distributions or memory layouts for their arrays.



