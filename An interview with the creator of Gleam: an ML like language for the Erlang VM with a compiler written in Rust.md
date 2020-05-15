# An interview with the creator of Gleam: an ML like language for the Erlang VM with a compiler written in Rust

![](https://miro.medium.com/max/360/1*ivv-xih7D4rulPdRNmSYkg.png?q=20)

I have been writting soft real time systems with Erlang for almost a decade and for that task I think it is the best tool we have around. The concurrency model, the preemptive scheduler, the GC, the profiling tools, the libraries and the community are excellent for the task. Distribution libraries like [Lasp](https://lasp-lang.readme.io/docs) or distributed systems frameworks like [Riak Core](https://github.com/lambdaclass/riak_core_tutorial) are not easily available in other languages. At last, cheap processes, non shared state, supervisors and the let it crash philosophy are great tools when you are writing backends. Instead of trying to catch all the errors at compile time, you accept that it is impossible to catch all the possible problems and you deal with that reality. It is a very different error handling model from what you can find in Haskell or OCaml.

