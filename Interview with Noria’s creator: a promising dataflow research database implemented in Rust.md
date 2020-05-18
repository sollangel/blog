# Interview with Noria’s creator: a promising dataflow research database implemented in Rust


After reading [Tensorflow’s original paper](http://download.tensorflow.org/paper/whitepaper2015.pdf) I learnt that four of its authors were authors of Microsoft [Naiad’s research paper](http://sigops.org/s/conferences/sosp/2013/papers/p439-murray.pdf) too. Naiad influenced many systems like Tensorflow.
The Naiad paper is really interesting since it brings together many computation patterns: batch computation, streaming computation, and graph computation. It seemed to be a higher level abstraction than the traditional MapReduce and at the same time a more elegant [Lambda architecture](https://en.wikipedia.org/wiki/Lambda_architecture) that combines batch processing with streaming methods. Being an practitioner and not a researcher this paper helped me to understand to compare different projects like Hadoop, Spark, Storm, Samza, Flink, Kafka streams.

Naiad’s paper introduced a computational model called timely dataflow that has influenced other systems like [Noria](https://github.com/mit-pdos/noria). Noria is a fast storage backend for read-heavy web applications implemented in Rust with a MySQL adapter. One of its creators is Jon Gjengset, a PhD student in the Parallel and Distributed Operating Systems group at MIT. Jon has a great [blog](https://thesquareplanet.com/blog/) and [youtube channel](https://www.youtube.com/channel/UC_iD0xppBwwsrM9DegC5cQQ) where he discusses everything from distributed algorithms to how to implement a ZooKeeper clone. He is into many subjects I love: Rust, distributed systems, databases and the relationship between Noria, Naiad and Tensorflow were enough reasons to interview him. We did not discuss Tensorflow but I hope to find out more about the relationship between it in the future.

----------

Join the Not a Monad Tutorial Telegram [group](https://t.me/notamonadtutorial) or [channel](https://t.me/channel_notamonadtutorial) to talk about programming, computer science and papers. See you there!

_If you are looking for good engineers send me an email to mail@fcarrone.com or you can also reach me via twitter at_ [_@federicocarrone_](https://www.twitter.com/federicocarrone)_._

----------

**What is Noria?**

Noria is a dynamic dataflow database that supports partial and incremental materialized views. To break that down a bit more, it is a database that is implemented using a streaming dataflow engine that can be changed on the fly (that’s the dynamic part). It supports pre-computing the results of queries (materialized views), and updates those materialized results as the data is updated (view maintenance).

When this happens, the results are updated in-place rather than recomputed wholesale (i.e., the maintenance is incremental). When queries have parameters (e.g., _foo = ?_), it supports materializing the results for only some value of _foo_, and will compute results for “missing” values only when they are required (i.e., the materializations are partial).

**How does it compare to other software like a relational database, in memory key-value stores, map reduce systems like Spark, stream processing like Storm or timely data flow?**

The answer here requires some nuance, so please bear with me.

Noria is very similar to a relation database on the outside. It has tables and SQL queries, and even supports the MySQL binary protocol. You interact with Noria through prepared statements, _SELECT_s, _INSERT_s, and _UPDATE_s. Internally though, it is quite different. Whereas a traditional database executes a query when it receives a _SELECT_, Noria generally executes **all** queries when data the query’s result depends on changes. In the steady state of the system, we expect queries to be executed on **write**, not on **read**. There are some smarts required here to not do undue work. For example, Noria only computes and maintains results for query parameters the application cares about (this is the “partial materialization” piece). Noria also executes queries “incrementally”; if you add a new vote to an article with a million votes, it knows to increase the count by one, rather than count a million and one things all over again.

While Noria implements a key-value store internally to maintain and serve its materialized results, it does not seem like a key-value store to users of the system. Application authors write full SQL queries, and get structured results (rows of columns) back, just like with a normal database. The only sense in which Noria is like a key-value store is in its performance — query results will generally be fetched about as fast as a key-value store lookup.

Streaming data-flow systems like Spark, Storm, Kafka, and timely dataflow share many similarities with Noria. They process data in the same streaming fashion, and have a similar distributed system design that relies on sharding and operator partitioning. Noria differs from these systems in a few principal ways though. First, users of Noria can change the running dataflow at any time without downtime. If a new SQL query is issued that the system has not seen before, it adapts the running dataflow on the fly to incorporate the operators from the new query. The adaptation also knows to re-use existing operators where possible to produce an overall more efficient dataflow than what you would get if you just ran each query as its own dataflow program.  
Second, Noria supports partial materialization. Existing dataflow systems that support materialization are usually either windowed (i.e., they only reflect “recent” updates) or fully materialized (i.e., all results are always stored and maintained). Neither of these would work for web applications. When you issue a query, you expect to get all the results for **that** query (so no windowing), but you also don’t expect the system to waste resources on results that your application does not care around. You can think of this latter requirement as “you need the ability to evict from your cache”. And finally, Noria has a familiar interface for its queries: you issue SQL queries, and the system automatically translates them into efficient dataflow and adapts the running system to them. The other systems do not generally support this.

**The Noria paper states that it can scale to 5x higher load than a hand optimised MySQL database. How does it manage to do that?**

The primary reason for this is Noria’s view materialization. When you issue a query to MySQL, the system has to **execute** that query to produce the query’s results. In Noria on the other hand, an application query effectively turns into a hashmap lookup in the common case, which is **very** fast. Noria’s reads can also happen entirely in parallel, with very little synchronization overhead, whereas MySQL includes a lot of machinery to support full-fledged transactions (which Noria does not support). Part of the trick here is Noria’s use of a neat little datastructure called an evmap (for “eventual map”; named for its support for eventual consistency). It allows reads and writes to proceed entirely in parallel with very little overhead by having reads and writes go to different hashmaps, and then occasionally atomically switching between them.

**What is the relationship between** [**Noria’s paper**](https://jon.tsp.io/papers/osdi18-noria.pdf)**, the** [**Differential dataflow**](https://github.com/timelydataflow/differential-dataflow/blob/master/differentialdataflow.pdf) **paper and** [**Naiad: a timely dataflow system**](http://sigops.org/s/conferences/sosp/2013/papers/p439-murray.pdf)**?**

Differential dataflow, timely dataflow, and its implementation in Naiad is one of the hallmark systems in the dataflow community. Most papers in this area relate to timely dataflow in one way or another. In the case of Noria, we also implement an incremental dataflow model, but we are trying to solve for a different use-case, and therefore arrive at different solutions.

In particular, timely established a model for incrementally executing dataflow programs that include iteration and cycles with strong guarantees about consistency through a sophisticated timestamp tracking scheme. Noria does not support iteration or cycles, and is eventually consistent.

Instead, we provide high-performance materialized views for fast reads, partial state so only the working set needs to be kept in memory, automatic multi-query optimization, support for dynamically modifying the dataflow as it runs, and of course SQL support. Timely does not support these features, though you could likely manually implement some of them on top of timely’s core given enough time and research effort.

Overall, I don’t think it’s fair to say one system is **better** than another. In many ways they complement each other. Timely largely targets arbitrary batch computations over a large, interconnected dataset where reads are less frequent than just observing the “output” of the computation. And it is **very** good at that. Noria targets read-heavy applications where repeated and similar queries are common, and where the queries change over time. And it is **very** good at that.

**Do you implement the full SQL language?**

It’s not clear what “full SQL” even really means, with all of the various extensions to the language that have been added to different implementations over time. Even if you restrict yourself to ANSI SQL though, the answer for Noria is no, although mostly for uninteresting reasons. Noria is a research prototype, and as such we have focused on the features that required active research to implement in Noria’s database model. Many of the remaining features we believe could be added with sufficient engineering effort, but without too much technical difficulty. To give some examples of things we don’t support: joins whose join condition is not a single column equality; _ORDER BY_ without a limit; _CASE_ statements; _LIKE_ conditions; and of course the _SOUNDEX_ operator. There are also patterns that we support, but that we believe could be optimized further, such as multi-way joins and multiple aggregations in a single query.

**Why did you choose to use RocksDB to persist the data?**

This was a more or less arbitrary decision. We initially wrote all base table writes directly to disk as a log, but quickly realized we needed to also keep indices over that on-disk data, otherwise recovery would be far too slow. We looked for an off-the-shelf solution, and RocksDB seemed to fit the bill. The interface for this base storage layer is pretty straightforward, and it should not be too difficult to slot in another solution there. The biggest challenge in doing so is maintaining some invariants around what writes are visible when for the purposes of Noria’s upqueries, but we believe these are solveable without too much trouble.

**Why does Noria needs to have Zookeeper running? Why did you choose ZooKeeper over etcd or consul?**

Zookeeper serves two purposes in Noria at the moment. Service discovery and leadership change. And in fact, if you run a Noria server and client in a single process, you don’t actually need Zookeeper running at all.

If you run a single Noria worker, and a separate client, Zookeeper is only used for the client to have an easy way to discover the location of the server. We’re considering adding a non-distributed mode to Noria which supports this single-worker use-case without Zookeeper. The bits are already in place (take a look at the _Authority_ trait in the code if you are curious), it just hasn’t been a priority for us to fix. If you are running **multiple** Noria workers, then they need some way to agree on which worker is responsible for driving application-issued changes to the dataflow, and that is where Zookeeper’s consensus comes into play. We needed a system that allowed the workers to agree on who that should be, with a mechanism for failover, and Zookeeper provided that. The API we need from Zookeeper is very limited, and it should be straightforward to slot in another consensus provider in its place.

**Is Noria production ready? Do you know anybody using it?**

Noria is most definitely still a research prototype, though I think the thing standing between where it is now and a production-ready version is mostly just engineering effort. We are a small team of researchers working on it, and we focus our efforts on the aspects of the system that are related to our ongoing research. There is relatively little room for spending lots of time on doing “production engineering” in the academic setting :)

That said, I know of several large companies who are very interested in  
using Noria in a production setting, and many of them have gotten in touch with me about what might be required to achieve that. I also know that multiple companies are trying Noria out privately internally to test its viability as a replacement for certain parts of their stack. What ultimately comes of that is unclear at the moment, but I of course hope that they find Noria promising, and that they are willing to invest time into making it production ready!

There are a few features missing from Noria that I think are the primary blockers from using it in production. The first is checkpointing of materialized views. Currently, if the system is turned off and then restarted, all the materialized views are empty. This would be equivalent to a full cache purge. An external system could heat the cache, but it’d be better if Noria internally kept some form of snapshot to aid in this process. The second is fault tolerance — when Noria is run in a distributed setting, a machine failure results in related parts of the dataflow being blown away entirely and restarted. This is obviously problematic in production settings. We are actively pursuing research in this area, and have some ideas for how to fix it, but it is a complex subject. And finally, Noria currently requires ownership of its base storage. If you have an existing data store that you’d like to keep using, you’d have to feed changes to that data to Noria, and Noria would store it a second time. Changing Noria such that it can handle the base storage being managed by a different system is possible, though would require some careful engineering with respect to consistency of upqueries.

**When would you avoid using Noria?**

Noria is not great for systems that do not have a well-defined working set. If your application is constantly issuing entirely new queries over your data that do not relate to previous queries, or if it rarely queries by the same set of keys more than once, then Noria’s materializations will be mostly useless and just add unnecessary overhead. Noria is also primarily built for read-heavy applications; if your application rarely does reads, but sustains a **very** high write load, then Noria in its current form is probably not what you want. I say “probably” because Noria already supports a fairly high write throughput, and if your inputs fall below that threshold, Noria will still work fine. And, crucially, its materializations will make your reads over this quickly growing collection really fast!

**Why did you choose to use Rust to implement it?**

The answer to this is perhaps less interesting than you’d expect. When  
we first started this iteration of the project in mid-2016, Rust was just starting to appear as a viable systems language. We were looking for a systems language to use for this new project, and did not particularly want to write C++, and wanted to explore something beyond Go. We heard about Rust, its claims were enticing, so as researchers we figured we’d try it out and see if it could live up to its promises. The cost of failure was low, as we could always start over, so we just ran with it. And now we’re 80k lines of code in, and I still think it was the right choice.

I can also give a post-hoc analysis of the journey. I think choosing Rust has worked out great for us; specifically, it has saved us countless hours of debugging. We write a lot of concurrent code in Noria, and the Rust compiler has caught a ridiculous number of concurrency bugs that would just have slipped right by in another language. Debugging those in a distributed context in a research system where we don’t even know if the underlying **algorithm** is sound would have been a major pain (and still is when it happens). Rust has also allowed us to write low-level code when we needed to squeeze out those last bits of performance in the core pieces of Noria. What is more, I have found the Rust ecosystem to be a joy to participate in and to rely on; solid libraries like tokio have allowed us to focus on the core research-y parts of the application, and lots of knowledgeable Rustaceans have helped us when we’ve run into weird issues. Not only that, but we have been able to contribute back to that ecosystem by publishing libraries of our own and by contributing back to the compiler and other core libraries that we relied on.

**You have videos on how to implement a TCP stack, a minimal Zookeeper implementation and blog posts explaining how to implement Raft. What would gain a traditional full stack developer learning how to implement low level structures, distributed algorithms or distributed software?**

I think much of this comes down to curiosity. If you’re curious about how stuff works, there’s an endless number of rabbit holes you can easily start down that teach you all of this stuff. For me, much of my distributed systems knowledge came from MIT’s 6.824 Distributed Systems class, which is **excellent**. All of their reading materials, lecture notes, and labs are also available online. For algorithms, the easiest way to get started in thinking about them is to read some of the early papers describing these algorithms, and then trying to implement them yourself! You’ll find that many of them aren’t as difficult to build as you may think, and you will learn a lot in the process. Including how tobread academic papers! I can also recommend trying to follow some low-level OS-building resources. For example, Philipp Oppermann has a great blog series on implementing an operating system from scratch in Rust that goes through all of the low-level details you’ll need to know.

Alternatively, all the components of MIT’s 6.828 Operating Systems class are also available online.

**What papers, readings and exercises do you recommend doing to learn about distributed programming?**

I would highly recommend following the 6.824 class schedule  
([https://pdos.csail.mit.edu/6.824/schedule.html](https://pdos.csail.mit.edu/6.824/schedule.html)). It covers both classic  
papers, established approaches, and new research in the area. Do the  
labs as well; they will force you to get **intimately** familiar with many  
subtle distributed systems problems!

   . . . 
-   [Rust](https://notamonadtutorial.com/tagged/rust)
-   [Big Data](https://notamonadtutorial.com/tagged/big-data)
-   [Distributed Systems](https://notamonadtutorial.com/tagged/distributed-systems)
