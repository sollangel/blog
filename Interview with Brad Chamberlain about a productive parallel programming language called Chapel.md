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

In my opinion, many scalable parallel language attempts have also failed to gain traction because they’ve been insufficiently general-purpose. Once programmers have a capability, they tend to be reluctant to give it up. This lack of generality often stems from the fact that most efforts have been undertaken by academic groups who need to pick their battles in order to publish papers and graduate students. With Chapel, we’ve created a language whose capabilities exceed C or Fortran with MPI and OpenMP, yet in a language that strives to be as attractive to read and write as Python.

It’s obviously very difficult for any new programming language to succeed, yet that’s no reason to avoid pursuing them — particularly when existing languages have major capability gaps. In our case, we believe that parallelism and locality are first-class programming concerns — not just in High-Performance Computing (HPC) where they make or break a program’s ability to scale well, but also in mainstream programming now that multicore processors and accelerators are pervasive. That said, parallelism and locality have traditionally been afterthoughts in language design. So rather than being paralyzed by the challenges of language adoption, we’re striving to fill that gap. And happily, users seem to be excited by our efforts.

**In some ways, don’t OpenMP, MPI, and CUDA solve the same problem?**

OpenMP, MPI, and CUDA are the _de facto_ standards for HPC programmers today when targeting multicore processors, distributed memory systems, and (NVIDIA) GPUs, respectively. In that sense, Chapel is targeting a similar problem space as they are. However, they’re not considered very productive approaches and are great illustrations of my claim that parallelism and locality are traditionally afterthoughts in language design: they’re implemented using pragmas, libraries, and language extensions rather than first-class syntax and semantics. As a result, they feel like they’re “tacked on” rather than a core part of the language. This hurts not just their ease-of-use but also their ability to be optimized by compilers.

These approaches also tend to be very specific to a given type of parallelism in the system architecture: If you’ve written an OpenMP program and now want to run it at scale on a cluster or a Cray, you’ll have to rewrite it in something like MPI, which requires learning a completely different set of features and abstractions. In contrast, Chapel is designed to support parallel programming across these diverse types of parallel hardware with a single, unified set of features for expressing parallelism and locality.

To their immense credit, OpenMP, CUDA, and (especially) MPI have been responsible for the vast majority of scientific advances in high-performance computing over the past several decades. And if you program in these notations and are happy with them, Chapel may not be for you. Yet, just as early computational results were obtained using assembly language before giving way to more modern and portable approaches like Fortran, C, C++, Java, and eventually Python, we think parallel computing is overdue for a similar leap in evolution: from lower-level, detail-oriented approaches to higher-level ones that improve productivity and portability. As such, Chapel strives to empower the millions of desktop-only programmers to use distributed parallel computers for the first time, while also making existing parallel programmers even more effective.

**What does Chapel do differently / better?**

A characteristic shared by most general-purpose approaches to scalable parallel programming — including MPI, SHMEM, UPC, and Fortran 2008’s co-arrays — is that they express parallelism using the _Single Program, Multiple Data_ (SPMD) programming model. The basic idea is that the user writes their program with the understanding that when it is run, multiple copies of ‘main()’ will execute simultaneously and cooperatively across a number of processors. This forces parallel programmers to write programs using a _local view_, in which the code expresses the perspective of a single process out of many: “What subset of the data do I need to allocate? What subset of the iteration space do I need to execute?” While the SPMD approach is sufficient for many computations, it’s also very different from traditional programming where one copy of ‘main()’ executes and all computation proceeds from that point. This requires programmers to think differently, to manage lots of bookkeeping details, and even to launch their programs differently. It also means that in order to get finer-grain parallelism, they need to mix in some other parallel programming model like POSIX threads, OpenMP, or CUDA.

In stark contrast, Chapel supports what we call a _global view_ of programming, in which a single task runs ‘main()’ and then any additional parallelism is created as features that introduce tasks are encountered. Similarly, computation and data are distributed across compute nodes when features that control locality are encountered. This permits scalable parallel programming to be far more intuitive and similar to conventional desktop programming, making it accessible to the millions of developers who might never get around to learning MPI. At the same time, Chapel’s global view also supports SPMD programming for computations or users that require it, so nothing is lost.

As an illustration of Chapel’s advantages, consider the STREAM Triad benchmark which multiplies a vector by a scalar, adds it to a second vector, and assigns it to a third. In Chapel, this can be written in a way that will use all the cores of all the system’s compute nodes as follows:

    use BlockDist, HPCCProblemSize;

    config type elemType = real;
    config const m = computeProblemSize(numArrays=3, elemType),
             alpha = 3.0;
         
    proc main() {
      const ProblemSpace = {1..m} dmapped Block(boundingBox={1..m});
      var A, B, C: [ProblemSpace] elemType;
      
      B = 2.0;
      C = 1.0;
      
      A = B + alpha * C;
      }
 
 In contrast, doing the same thing in C + MPI + OpenMP results in computation like the following, due to the SPMD programming model as well as the lower-level notations (MPI-oriented code is in red, OpenMP in blue):
 
 ![](https://miro.medium.com/max/1400/0*7A817ub0TK9QWgLW.)

**Performance-wise, how does Chapel compare to languages like C, C++, Go, Rust?**

Today, Chapel programs tend to perform competitively with hand-coded C and C++. I’m not aware of any detailed performance comparisons with Go and Rust, though in the [Computer Language Benchmarks Game](https://benchmarksgame.alioth.debian.org/) (CLBG) we’re currently ranked as being a bit faster than Go and a bit slower than Rust. That said, there are specific CLBG benchmarks where any of these five languages win or lose, and many of the fastest entries take a far more heroic and painstaking approach than the Chapel versions.

Since we care about code clarity, we tend to graph the Computer Language Benchmark Game results on scatter plots showing normalized execution times versus code compactness (as a proxy metric for clarity). In such views, Chapel tends to fall in a very unique position, being competitive in speed with the fastest languages while also nearly as compact as scripting languages. The following two plots illustrate this (the right graph zooms in on the fastest entries for readability):

![](https://miro.medium.com/max/760/0*5HKm6tfdrqfXiXWP.?q=20)


![](https://miro.medium.com/max/760/0*YHDLOnTtFepJmsUV.?q=20)



**Why did you implement Chapel using LLVM**
