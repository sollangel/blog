# Lasp: a little further down the Erlang rabbithole

A few years ago I found [Lasp](https://lasp-lang.readme.io/docs): _“a suite of libraries aimed at providing a comprehensive programming system for planetary scale Elixir and Erlang applications”_. At this point it should come as no surprise for you to learn that here at Not a Monad Tutorial we are are interested in distributed systems and Erlang. After playing a little bit with Lasp I watched a few talks by its creator: [Christopher Meiklejohn](https://twitter.com/cmeik). After watching his talk [“Distributed, Eventually Consistent Computations”](https://www.youtube.com/watch?v=lsKaNDj4TrE) I decided it was time to interview Christopher.
Reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen) if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial/). **Stay tuned!**

_Discuss and vote at_ [_lobsters_](https://lobste.rs/s/3gvmyl/lasp_little_further_down_erlang)_,_ [_reddit_](https://www.reddit.com/r/programming/comments/6a620o/lasp_a_little_further_down_the_erlang_rabbithole/) _and_ [_hn_](https://news.ycombinator.com/item?id=14300763)_._

![](https://miro.medium.com/max/560/1*SKlcy2D4QxhBrMdNYNCIHA.jpeg?q=20)

**What is Lasp?**
Originally, Lasp was a programming model designed for deterministic distributed computing with weak synchronization. Lasp’s programming model appears function, in that you write applications that look like functional programs, but under arbitrary distribution, these applications are guaranteed to return the correct result, with minimal coordination, under network anomalies such as network partitions or node failures. Lasp achieves this by building upon the design philosophy of Conflict-free Replicated Data Types, or CRDTs: data structures that are designed to achieve convergence without requiring locking or other synchronization primitives.

During the evaluation phase of Lasp, we were tasked with trying to scale a prototype implementation written in Erlang to 10,000 nodes; we got as far as we could in the allotted time we had, which was 1,024 nodes running on Amazon EC2. In the process of trying to achieve that scale, we had to develop a number of libraries in Erlang to provide supporting infrastructure: new distribution algorithms, optimized implementations of the data types, deployment and operations tooling, etc. So, I’d say at this point Lasp, in the academic sense refers to the original programming model, but Lasp in the industrial sense, refers to the entire suite of libraries that deliver the programming system and programming model at large scale.

**What are CRDTs? What problems do CRDT solve?**

CRDTs, or Conflict-free Replicated Data Types, are data types that are designed for use in distributed systems: think regular sequential abstract data types, but with a predefined, deterministic merge function for any two possible values.

One of the big challenges in distributed computing is related to consistency. When a network partition occurs, any system that is managing replicated data must make a choice: do they allow operations to proceed, remaining available-under-partition, or do they prohibit operations to proceed, remaining consistent-under-partition.  
Consistent-under-partition systems, or CP systems, provide strong consistency which makes application development easier, whereas available-under-partition systems, or AP systems, allow the developer to both exploit available concurrency in the system for performance, and keep servicing requests when network partitions inevitably occur. Available-under-partition systems are ideal for applications that are geo-distributed, because they allow users to read and write locally to their geographically close replica and don’t incur a synchronization penalty for write operations (which, in some cases can be > 100ms an operation).

However, one of the challenges in using available-under-partition systems is the potential for write conflicts: two writes happen to the same object concurrently at two replicas. When the network ultimately convergences, the different replicas have to come to an agreement over which value wins. When using a database with opaque value registers that you `set` or `get` values on, this choice can be arbitrary: either choice may be valid, and systems like Cassandra resort to solving this by using the user provided timestamp to pick a value. Approaches like this, while deterministic, are problematic, however, because arbitrarily picking a value based on time and dropping the other write operation may fail to capture developer intent.

CRDTs say, rather than have opaque registers, why not store actual data types in the database, and then have a conflict resolution policy that is compatible with the semantics of the data type. One example of a trivial CRDT is the grow-only set: if you can never remove elements from the set, it’s always safe to merge copies of a set, that is independently modified, using the set union operation. Similar designs exist for sets where you can add and remove elements arbitrarily, graphs, dictionaries, counters, and booleans.

**What other alternatives exists apart from CRDTs to solve the same type of issue?**

Operational transformation is an alternative approach, which predates CRDTs, and was used to build both Google Docs and Apache Wave. Operational transformation relies on “transforming” edit operations based on concurrent operations so they achieve the desired effect through the transformation once the document has been modified. There exist a significant number of different algorithms, each which makes a different set of tradeoffs, and there’s no algorithm that’s better than the others in the general case. Its are extremely difficult to implement correctly and to verify, given the number of possible operations and operation interleaving and transformations that must be considered.

**Are there any downsides of using CRDTs?**

CRDTs can be very expensive in terms of implementation and state synchronization. Lots of effort has gone into reducing the overhead in state transmission through both operations-based CRDTs (a variant that sends just operations instead of state, with the tradeoff that it requires a stronger property for message delivery from the network) and delta-CRDTs (a variant that minimizes the required state that needs to be transferred by minimizing the change representation.)

One open challenge for both CRDTs, and any system that has to manage a large-amount of replicas of objects that will be concurrently operated on, is actor management. Typically, these systems and data structures must carry metadata sizes O(n) on the number of actors to ever modify an object in the system: in a system with a large amount of mobile devices and high churn, this can be prohibitive in terms of space. Recent approaches to try to address this problem rely on either imposing a structure on the way nodes share information with one another to allow some nodes to subsume the changes of other nodes, or allowing transient nodes to temporarily “borrow” identities of a smaller number of permanent nodes so the identifiers of temporary nodes aren’t carried around in the objects metadata indefinitly.

**What is** [**Lasp process registry**](https://lasp-lang.readme.io/docs/what-is-lasp-pg)**? Why create a new process registry if we already got global, pg2, gproc or syn in Erlang?**

About three or so years ago I created Riak PG, a process registry that used the same distribution strategy as Riak (with, a corresponding paper presented at the Erlang Workshop that year.) I created this process registry because I had done an extensive study and writeup about why pg2, proc, and global are not designed properly for  
distributed scenarios where partitions can occur and availability is paramount.

Lasp PG is a natural extension of this, where it uses an unstructured overlay with full replication and CRDTs instead of partial replication across a structured overlay network and CRDTs.

While Ericsson is working on scaling the global facility and growing distributed Erlang to support a larger number of nodes, they are focusing on:

a) supporting existing applications developed at Ericsson with distributed Erlang

b) smaller scale of nodes, operating in a LAN configuration (think ~200–500)

Their solution for scaling global shards the information across nodes, and requires availability of the DHT (they are using Kademlia, a structured overlay network) for requests to be serviced.

Lasp (and, therefore, Lasp PG) is focusing on large-scale, wide-area programming: think ~10–100k nodes operating at geo-scale. In this scenario, partitions are common — in fact, mobile clients or IoT devices might disable their antennas to preserve battery. In this scenario, we have to assume that nodes are not aware of every other node in the network, have to route messages through other nodes, and have to be resilient to partitions and have the ability to keep operating. Lasp PG is the first step towards this, and our work that was presented at Erlang Factory, Loquat, is the second step towards this goal.

**Do you think that parts of what you created and found while developing Lasp will be ported to more traditional programming languages?**

Hopefully, but it’s unclear what components will end up being useful or not. We are mainly focused on quickly prototyping things and performing evaluations, all in Erlang and Elixir, to determine which approaches scale, are easy to program with, etc.

I think that once we get a bit further along in the research, maybe 4 to 5 years, that some of the ideas we’re just coming up with now might be useful and developed enough to make it into the mainstream.

**What is** [**Partisan**](https://lasp-lang.readme.io/docs/overview)**? Why did you not use the default distributed Erlang?**

Partisan is a membership layer for clustering groups of Erlang nodes. It bypasses distributed Erlang completely, can run in a variety of topologies, will soon support connection multiplexing, and has full support for TLS/SSL. Partisan can support clusters of nodes running in star-topologies, random unstructured overlays, and clusters that are fully connected.

`Distributed Erlang is a bit too rigid for us: it assumes a fully connected network, which is extremely difficult to scale to large clusters of nodes, and uses a single TCP connection between all processes communicating from one node to another. In order to build a more reliable system that would scale to large clusters of nodessecurely, we needed our own membership layer for state dissemination.` Right now, partisan doesn’t support all of the semantics that Erlang provides, but we’re actively working with some developers in the open source community to extend partisan to support normal Erlang message passing across our highly-available framework.
We’ve also started to see some companies using Erlang and Elixir pick up partisan as a tool for helping them build reliable, large-scale, applications that need to do efficient state dissemination which is very nice for an implementation that came out of a research group.

**What other programming languages or pieces of software do you keep an eye on?**

Right now, what I’m most excited about is Space-Time Insight’s implementation of Microsoft Orleans in Erlang, called [Erleans](https://github.com/SpaceTime-IoT/erleans). I think that Orleans take a lot of the complexity out of actor management in Erlang and Elixir and helps developers get straight to building distributed applications without having to focus on the low level details around message routing, actor placement, and actor creation and termination.

**What do you recommend reading or doing for those of us that we are trying to learn more about distributed systems?**

I think that everyone is building distributed applications nowadays — from your web developer building a rich-web client in JavaScript to mobile developers building the next hit application — we all have to deal with the problems of state, synchronization, offline operation, and maintaining consistency.

That said, from the academic side, I’ll recommend [“Introduction to Reliable and Secure Distributed Programming”](http://www.springer.com/gp/book/9783642152597) from Cahin, Guerraoui, and Rodrigues. It’s the book that my university teaches with, and the course that I’ve TA’d and contributed content to.

The course is also taught via a MOOC on edX as well, and I’ll be doing  
a guest lecture this semester in Part 2 on Lasp and CRDTs.

| [Reliable Distributed Algorithms, Part 1](https://www.edx.org/course/reliable-distributed-algorithms-part-1-kthx-id2203-1x)
###### This course gives a comprehensive introduction
to the theory and practice of 
distributed algorithms for designing…
###### www.edx.org | [Reliable Distributed Algorithms, Part 1
KTHx on edX 
Course About Video](https://www.youtube.com/watch?time_continue=8&v=HRslgnaIrPU&feature=emb_title) |
|--|--|


