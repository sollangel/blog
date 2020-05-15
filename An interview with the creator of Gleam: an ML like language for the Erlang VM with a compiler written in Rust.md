# An interview with the creator of Gleam: an ML like language for the Erlang VM with a compiler written in Rust

![](https://miro.medium.com/max/460/1*ivv-xih7D4rulPdRNmSYkg.png?q=20)

I have been writting soft real time systems with Erlang for almost a decade and for that task I think it is the best tool we have around. The concurrency model, the preemptive scheduler, the GC, the profiling tools, the libraries and the community are excellent for the task. Distribution libraries like [Lasp](https://lasp-lang.readme.io/docs) or distributed systems frameworks like [Riak Core](https://github.com/lambdaclass/riak_core_tutorial) are not easily available in other languages. At last, cheap processes, non shared state, supervisors and the let it crash philosophy are great tools when you are writing backends. Instead of trying to catch all the errors at compile time, you accept that it is impossible to catch all the possible problems and you deal with that reality. It is a very different error handling model from what you can find in Haskell or OCaml.

However Erlang language is pretty simple. I always miss sum types when I am coding in Erlang. I miss ML’s type system expressiveness, safety and practicality. That is why I am interested in the development of Gleam, a statically typed functional programming language for the BEAM.

Another interesting thing about Gleam is that its compiler is written in Rust. I think that Rust is a sort of ML + C language. I like C since the developer is at the driver seat driving with manual transmission. I can’t explain very well but I have always seen C as a simple and powerful language but I have always disliked C++. Knowing that I like ML and C you might understand why I find Rust an interesting language.

To sum up we (me and [Juan Bono](https://twitter.com/JuanBono)) decided to do this interview with [Louis Pilfold](https://twitter.com/louispilfold) not only because of what it is, but also because it is implemented in Rust. Go ahead and check [Gleam’s repo](https://github.com/lpil/gleam)!

![](https://miro.medium.com/max/460/1*x_OU1YRmBR8037eqsSAfYA.png?q=20)


I ‘ve created a Not a Monad Tutorial newsletter so that you receive an email whenever we publish a new story. [Sign up!](https://mailchi.mp/9302d4f60de9/not-a-monad-tutorial)
