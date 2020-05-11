# Interview with Jay Kreps about Apache Kafka


![](https://miro.medium.com/max/444/1*lRRJqrarJFi5TPtnBm_8hA.jpeg?q=20)

This time we interviewed [Jay Kreps](https://twitter.com/jaykreps), one of the creators of Apache Kafka. Kafka is an open source messaging system with a few design choices that make it particulary useful for high throughput and low latency scenarios.

_“This experience lead me to focus on building Kafka to combine what we had seen in messaging systems with the log concept popular in databases and distributed system internals. We wanted something to act as a central pipeline first for all activity data, and eventually for many other uses, including data deployment out of Hadoop, monitoring data, etc.” — Jay Kreps_

Kafka is built around the concept of a distributed database commit log. If you have no idea what that is, then I highly recommend that after you finish reading the interview you check the links I have pasted at the end. I learnt a lot by reading them.
In the following weeks I am going to publish an interview with [Martin Kleppmann](https://twitter.com/martinkl), one of the authors of Samza, about his book Data Intensive Applications and realtime stream processing systems vs batch processing systems.

Reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen) if you have any comments or interview request for [**This is not a Monad tutorial**](https://medium.com/this-is-not-a-monad-tutorial). Stay tuned!

                                        ...
       
**What problem does Kafka solve?**
Kafka is a distributed storage system for data streams. It allows you to publish streams of data and subscribe to them. It is built around the concept of a persistent log that is appended to — publishers of data append to this log and consumers subscribe to changes. Perhaps most importantly, it scales really well so it can function as a central hub for these data streams even in a company with a lot of data like LinkedIn or Netflix or Uber.
