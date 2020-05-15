# **Interview with** Will Kurt on his latest book: Bayesian Statistics The Fun Way


![](https://miro.medium.com/max/544/1*LDJcJQMeyOPU9lqAs98JBQ.jpeg?q=20)


Like most devs, I have a diverse set of interests: functional programming, operating systems, type systems, distributed systems, and data science. That is why I was excited when I learned that [Will Kurt](https://twitter.com/willkurt), the author of [_Get Programming with Haskell,_](https://www.manning.com/books/get-programming-with-haskell)  wrote a a bayesian statistics book that is being published by No Starch Press. There aren’t many people that write books on different topics. I was sure that Will had something interesting to share in this new book. I wasn’t disappointed. The book is an excellent introduction, specially for those of us that have a rough time with advanced math but that want to advance in the data science field. I recommend reading the book after reading Think Stats, but before reading Bayesian Methods for Hackers, Bayesian Analysis with Python and Doing Bayesian Data Analysis.

If you like the interview I recommend that you also read the interviews we did with [Thomas Wiecki](https://notamonadtutorial.com/inteview-with-thomas-wiecki-about-probabilistic-programming-and-pymc-66a12b6f3f2e) and [Osvaldo Martin](https://notamonadtutorial.com/interview-with-osvaldo-martin-about-bayesian-analysis-with-python-a696b2bce3ba) about Bayesian analysis and probabilistic programming.

Finally I wanted to thank two members of my team (Pablo Amoroso and Juan Bono) for helping me with the interview.

                                                  . . .
Reach me via twitter at @federicocarrone if you have any comments or interview request for This is not a Monad tutorial.
If you have an idea, you are looking for a part time CTO, need a team of devs or have maintenance work ping us: LambdaClass.
                                                  . . .

**1. Why a new statistics book?**

Nearly all of the many excellent books on Bayesian statistics out now assume you are either familiar with statistics already or have a pretty solid foundation in programming. Because of this the current state of Bayesian statistics in often as an advanced alternative to classical (i.e. frequentist) statistics. So even though Bayesian statistics is gaining a lot of popularity, it’s mostly amount people who already have a quantitative background.

When someone wants to simply “learn statistics” they usually pick up an introduction based on frequentist statistics and end up half understanding a bunch of tests and rules, and feel very confused by the subject. I wanted to write a book on Bayesian statistics that really anyone could pick up and use to gain real intuitions for how to think statistically and solve real problems using statistics. For me there’s no reason why Bayesian statistics can’t be a beginners first introduction to statistics.

I would love it if, one day, when people said “statistics” it implied Bayesian statistics and frequentist statistics was just an academic niche. To get there we need more books that introduce statistics to a wide audience using Bayesian methods and assume this may be the readers first exposure to stats. I toyed with the idea of just calling this book “Statistics the Fun Way”, but I know I would probably get angry emails from people buying the book help with stats 101 and getting very confused! Hopefully this book will be a small step in getting “stat 101” to be taught from the Bayesian perspective, and statistics can make sense from the beginning.

**2. Who is your intended audience for the book? Could anyone without a math background pick it up?**

My goal with Bayesian Statistics the Fun Way was to create a book that basically anyone with a high school math background could pick up and read. Even if you only vaguely remember algebra, the book moves at a pace that should be easy to follow. Bayesian statistics does require just a little calculus and is a lot easier with a bit of code, so I’ve included two appendices that cover enough R to work as an advanced calculator and enough background in the ideas of calculus that when the book needs talk about integrals you can understand. But I promise that there is no solving of any calculus problems required.

While I worked hard to limit the mathematical prerequisites for the book, as you read through the book you should start picking up on mathematical ways of thinking. If you really understand the math your using, you can make better use of it. So I don’t try to shy away from any of the real math, but rather work up to it slowly so that all the math seems obvious as you develop your understanding. Like many people, I used to believe that math was confusing and difficult to work with. In time I really saw that when math is done right, it should be almost obvious. Confusion in mathematics is usually just the result of moving too quickly, or leaving out important steps in the reasoning.

**3. Why should software developers learn probability and statistics?**

I really believe everyone should learn some probability and statistics because it really does help to reason about the uncertain world which is our everyday life. For software developers in particular there are few common places that its useful to understand statistics. It’s pretty likely that at some point in your career in software, you’ll need to write code that makes some decision based on some uncertain measurement. Maybe it’s measuring the conversion rate on a web page, generating some random reward in a game, assigning users to groups randomly or even reading information from an uncertain sensor. In all these cases really understanding probability will be very helpful. In the software part of my career I’ve also found that probability can help a lot in troubleshooting bugs that are difficult to reproduce or to trace back to a complex problem. If a bug appears to be caused by insufficient memory, does adding more memory decrease the probability of the bug in a meaningful way? If there are two explanations for a complex bug, which should be investigated first? In all these cases probability can help. And of course with the rise of Machine Learning and Data Science, engineers are more and more likely to be working on software problems that involve working directly with probabilities.

**4. Could you give a brief summary of the difference between the frequentist and bayesian approaches to probability?**

Frequentists interpret probability as a statement about how frequently an event should occur in repeated trials. So if we toss a coin twice we should expect to get 1 head because the frequency of heads is 1/2. Bayesians interpret probability as a statement of our knowledge, basically as a continuous version of logic. The probability of getting heads in a coin toss is 0.5 because I don’t believe getting heads is any more likely than getting tails. For coin tosses both schools of thought work pretty well. But when you talk about things like the probability that your favorite football team will win the world cup, talking about degrees of belief makes a lot more sense. This additionally means that Bayesian statistics does not make statements about the world but about our understanding of the world. And since we each understand the world a bit differently, Bayesian statistics allows us to incorporate that difference into our analysis. Bayesian analysis is, in many ways, the science of changing your mind.

**5. Why did you choose to focus on the bayesian approach?**
