# Sonic: a minimalist alternative to Elasticsearch written in Rust

![](https://miro.medium.com/max/904/1*ur9rT3EUiunAzys52MePnQ.jpeg)|« Sonic » is the mascot of the Sonic project. Valerian drew it to look like a psychedelic hipster hedgehog.|
 

Database implementation sits in a nice spot between computer science and software engineering. There are lot of tradeoffs to consider. That is why nowadays we have a plethora of databases, each of them useful in a particular scenario. In the projects we work on at [LambdaClass](https://lambdaclass.com/) we always end up using the following: Redis, Elasticsearch, PostgreSQL, Kafka and Riak or Cassandra. It is difficult to keep up with the number of databases that are needed and it is even more difficult to learn about their internals.

I always end up using Elasticsearch to index documents, to generate autocompletes and for geolocation. [Sonic](https://github.com/valeriansaliou/sonic) doesn’t solve all three problems but it is a good tool to solve the first two. I have not yet used it in production, but it seems like a good lightweight alternative to Elasticsearch.

Since we love databases and we are trying to focus on Rust projects, [Amin Arria](http://twitter.com/nenearria) and I decided to interview Sonic’s creator, [Valerian Saliou,](https://github.com/valeriansaliou) who generously agreed. Also remember to check Sonic’s [repository](https://github.com/valeriansaliou/sonic).

                                                               . . . 
_I‘ve created a Not a Monad Tutorial newsletter so that you receive an email whenever we publish a new story._ [_Sign up_](https://mailchi.mp/9302d4f60de9/not-a-monad-tutorial)_!_

_Reach me via twitter at_ [@_federicocarrone_](https://twitter.com/federicocarrone/) _if you have any comments or interview request for_ [_This is not a Monad tutorial_](https://medium.com/this-is-not-a-monad-tutorial/)_._   

                                                               . . . 


