# Interview with Weld’s main contributor: accelerating numpy, scikit and pandas as much as 100x with Rust and LLVM

After working for weeks with Python’s and R’s data science stack I started to ask my self if there could be a common intermediate representation, similar to CUDA, that could be used by many languages. There should be something better than reimplementing and optimizing the same methods in each language. In addition to that, having a common runtime that could optimize the whole program instead of each function separately would be better. After a few days of researching and testing different projects I found [Weld](https://www.weld.rs/) (you can also read its [paper](https://cs.stanford.edu/~matei/papers/2017/cidr_weld.pdf)). To my surprise, one of the creators of Weld is [Matei Zaharia](https://twitter.com/matei_zaharia), who also is the creator of Spark.

That is how I contacted and interviewed [Shoumik Palkar](https://shoumik.xyz/), the main contributor of Weld. Shoumik is a Ph.D. student in the Computer Science department at Stanford University, that is advised by Matei Zaharia.

Weld is far from being production ready but it is promising. If you are interested in the future of data science and in Rust, you will like this interview.

![](https://miro.medium.com/max/360/1*hqC6KtF-l1RN8uDg99rmow.png?q=20)
###### Not a Monad Tutorial new logo!


Join the Not a Monad Tutorial Telegram [group](https://t.me/notamonadtutorial) or [channel](https://t.me/channel_notamonadtutorial) to discuss about Weld and software in general. See you there!

_If you are looking for good engineers send me an email to mail@fcarrone.com or you can also reach me via twitter at_ [_@federicocarrone_](https://twitter.com/federicocarrone/)_._
                                              . . .
 
 
