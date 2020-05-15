# Interview with Weld’s main contributor: accelerating numpy, scikit and pandas as much as 100x with Rust and LLVM

After working for weeks with Python’s and R’s data science stack I started to ask my self if there could be a common intermediate representation, similar to CUDA, that could be used by many languages. There should be something better than reimplementing and optimizing the same methods in each language. In addition to that, having a common runtime that could optimize the whole program instead of each function separately would be better. After a few days of researching and testing different projects I found [Weld](https://www.weld.rs/) (you can also read its [paper](https://cs.stanford.edu/~matei/papers/2017/cidr_weld.pdf)). To my surprise, one of the creators of Weld is [Matei Zaharia](https://twitter.com/matei_zaharia), who also is the creator of Spark.

That is how I contacted and interviewed [Shoumik Palkar](https://shoumik.xyz/), the main contributor of Weld. Shoumik is a Ph.D. student in the Computer Science department at Stanford University, that is advised by Matei Zaharia.

Weld is far from being production ready but it is promising. If you are interested in the future of data science and in Rust, you will like this interview.

![](https://miro.medium.com/max/360/1*hqC6KtF-l1RN8uDg99rmow.png?q=20)
###### Not a Monad Tutorial new logo!


Join the Not a Monad Tutorial Telegram [group](https://t.me/notamonadtutorial) or [channel](https://t.me/channel_notamonadtutorial) to discuss about Weld and software in general. See you there!

_If you are looking for good engineers send me an email to mail@fcarrone.com or you can also reach me via twitter at_ [_@federicocarrone_](https://twitter.com/federicocarrone/)_._
                                              . . .
 
 **What was the motivation to develop weld and what problem’s does it solve?**

The motivation behind Weld is to provide bare-metal performance for applications that rely on existing high-level APIs such as NumPy and Pandas. The main problem it solves is enabling cross-function and cross-library optimizations that other libraries today don’t provide. In particular, many commonly used libraries provide state-of-the-art implementations for algorithms on a per-function basis (e.g., a fast join algorithm implemented in C in Pandas, or a fast matrix multiply in NumPy), but do not provide any facility for enabling optimization across these functions (e.g., preventing unnecessary scans of memory when performing a matrix multiply followed by an aggregation). Weld provides a common runtime that enables libraries to express computations in a common IR; that IR can then be optimized using a compiler optimizer, and can then be JIT’d to parallel native machine code with optimizations such as loop fusion, vectorization, etc. Weld’s IR is natively parallel, so programs expressed in it can always be trivially parallelized.

We also have a new project called split annotations which will integrate with Weld that’s meant to lower the barrier for enabling these optimizations in existing libraries.

**Optimizing numpy, pandas and scikit wouldn’t be easier? How faster it is?**

Weld provides optimizations across functions in these libraries, whereas optimizing these libraries would only make individual function calls faster. In fact, many of these data libraries are already highly optimized on a per-function basis, but deliver performance below the limits of modern hardware because they do not exploit parallelism or do not make efficient use of the memory hierarchy. For example, many NumPy ndarray functions are already implemented in C, but calling each function requires scanning over each input in entirety. If these arrays do not fit in the CPU caches, most of the execution time can go into loading data from main memory rather than performing computations. Weld can look across individual function calls and perform optimizations such as loop fusion that will keep data in the CPU caches or registers. These kinds of optimizations can improve performance by over an order of magnitude on multi-core systems, because they enable better scaling.

![](https://miro.medium.com/max/1104/1*eheS9p1hxxEPH8Fqo3As8A.png)

###### _Prototype integrations of Weld with Spark (top left), NumPy (top right), and TensorFlow (bottom left) show up to 30x improvements over the native framework implementations, with no changes to users’ application code. Cross library optimizations between Pandas and NumPy (bottom right) can improve performance by up to two orders of magnitude._


