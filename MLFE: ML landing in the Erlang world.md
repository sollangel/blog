# MLFE: ML landing in the Erlang world

Time has passed since our last interview for [This is not a Monad Tutorial](http://notamonadtutorial.com/). OpenBSD released its 6.0 version after 20 years of continues releases without agile, OpenSSL vulnerabilities keep on breeding, [Pornhub war against ad blockers continued](http://blog.bugreplay.com/post/152579164219/pornhubdodgesadblockersusingwebsockets), the Macbook Pro is not that Pro anymore, a new javascript package manager was released by Facebook and last but not least Brexit and Trump are a reality that will shape the new world order.

Going back to our endogamic tech world, I interviewed [Jeremy Pierre](http://noisycode.com/) about [MLFE](https://github.com/j14159/mlfe) or ML Flavoured Erlang. After learning Haskell and after playing with Elm I became quite a fan of ADTs and Hindley-Milner type systems so that is why I am really exited by MLFE. So I think this is a way to counterbalance my love for Lisp languages. Let’s add a little bit of ML into our coffee. One of the projects I had in my To Do list was to implement an ML like language on top of the Erlang VM/BEAM. Instead of recreating the wheel, I hope to see further by standing upon the shoulders of MLFE. The invasion has began!

If you have any more questions please let me know via [lobsters](https://lobste.rs/s/vw8zb2/d_day_invasion_with_mlfe_ml_landing_erlang), [reddit programming](https://www.reddit.com/r/programming/comments/5d2ooi/dday_invasion_with_mlfe_ml_landing_in_the_erlang/) or [hn](https://news.ycombinator.com/item?id=12958099). Au revoir.

![](https://miro.medium.com/max/410/1*VHoeeRQqosE3Woi37aHFeA.jpeg)2016 MLFE

**What is MLFE?**
ML Flavoured Erlang is a statically typed and strict (eagerly evaluated) language for the Erlang VM patterned a little bit after parts of both OCaml and Elm. It’s incredibly early in the language’s development but we already have things like sum types and product types (e.g. tuples, records with row polymorphism) and a pretty basic foreign function interface to Erlang proper. Honestly the language is probably in need of renaming almost entirely because it’s impossible to call Erlang code directly outside of the FFI.  
  
**Why did you create an ML language on top of the Erlang VM/BEAM?**

I appreciate a lot of the practicalities in the Erlang ecosystem, not least of which are the operational aspects of the VM but wanted something with faster feedback on type issues than Dialyzer currently provides, along with the brevity and expressiveness that come with features like OCaml or Elm’s ADTs. One of the earliest drivers for this was constantly wishing for Scala’s case classes every time I wrote Erlang (my day job involves both).

**What are the main problems you find while coding in Erlang and while coding in ML like languages?**

I don’t often work directly in ML dialects to be perfectly honest although I keep trying to find reasons to use them. Maybe that’s a subconscious reason behind starting MLFE .

As mentioned above however I do often work in Scala and its less pervasive approach to pattern matching and second class nature of things like actors on the JVM are often sore points for me. Having said that, it would be really great to have something like Scala’s Future or ScalaZ’s Task in an Erlang VM language as well. Maybe we can build something with MLFE to scratch that itch but it’s a little early to say I think.

**How did you implement the type inference algorithm?**

I have to give credit (or apologize for implementing it badly?) to Oleg Kiselyov for his incredibly helpful article [How OCaml type checker works](http://okmij.org/ftp/ML/generalization.html). I relied heavily on this, [Types and Programming Languages](https://www.cis.upenn.edu/~bcpierce/tapl/), and a bit on [algorithm_w](https://github.com/tomprimozic/type-systems/tree/master/algorithm_w) for the earliest work on the typer.

It started with a very basic translation to Erlang of Oleg Kiselyov’s eager/strict inferencer examples and then grew from there. Since the algorithm relies on unification (and hence mutation), MLFE’s inferencer needs reference cells which are currently implemented as Erlang processes. I’d like to move these to something like ETS but will probably wait until trying to rewrite and clean up the typer in MLFE itself.  
  
**Do you generate core Erlang code or Erlang code?**

The MLFE AST is translated to a Core Erlang AST using the “cerl” module and then compiled to BEAM modules from there.

**How do you type check messages?**

In MLFE a receive block types as a _receiver_. Receivers are polymorphic with two parameters: the kind of messages received and the result type of the contained expression. These two parameters are determined by unifying the types in the patterns and the types in the result portion of the receive’s clauses, e.g.

    receive with   
     0 -> :zero  
     | x, is_integer x -> :not_zero
     
All of the above patterns are clearly integers and all of the result portions are atoms so the type (internally) in MLFE would be `{t_receiver, t_int, t_atom}`. As an aside we can of course use union types if we want more complex messages.

Unifying receivers with enclosing expressions makes _those_ expressions receivers too so if we’re assigning a variable in a let expression, the whole thing becomes a receiver e.g.

    let my_msg = receive with  
     0 -> :zero  
     | x, is_integer x -> :not_zero   
    in [my_msg, :in_a_list, :why_not]

The above let expression is a _{t_receiver, t_int, {t_list, t_atom}}._ If we wrapped that in a function, the whole function is a receiver:

    foo () = let my_msg = receive with   
     0 -> :zero  
     | x, is_integer x -> :not_zero   
    in [my_msg, :in_a_list, :why_not]

The type of the above is:

    {t_receiver, t_int, {t_arrow, [t_unit], {t_list, t_atom}}}
    
That is a function from unit to a list of atoms that internally receives integer messages.

Now when we spawn a process, _spawn_ uses the receiver’s first parameter to constrain the resulting PID to a specific type. If we spawn a function that’s a receiver of integers (resulting in a _{t_pid, t_int}_), all messages sent must be able to unify with _t_int_ so if we try to send it a float or a string we get a type error at compile time.

If we spawn a function that is _not_ a receiver, we get a _{t_pid, undefined}_. Since _undefined_ will not unify with any other type, it’s a type error to send that particular process any messages at all.

**One of the biggest issues I have with Erlang is that in some cases it is necessary to write code that has a lot of nested cases. Elixir added the** [**with**](http://learningelixir.joekain.com/learning-elixir-with/) **special form to deal with this issue. Rust 1.3 added a new** [**? operator**](https://blog.rust-lang.org/2016/11/10/Rust-1.13.html#the--operator) **that provides syntax sugar that helps to deal with this issue. The RabbitMQ team created** [**Erlando**](https://www.rabbitmq.com/blog/2011/05/17/can-you-hear-the-drums-erlando/) **using Erlang’s parse transform (Erlang like macros) that adds syntax extensions to Erlang, with the** [**do syntax**](https://github.com/rabbitmq/erlando#user-content-lots-of-different-types-of-monads) **for monads being the most important one to solve somehow this issue. From what I have seen the mlfe_type.erl of MLFE also** [**has this issue**](https://github.com/j14159/mlfe/blob/master/src/mlfe_typer.erl#L403-L427)**.**

**Do you plan on adding some construct or syntactic sugar to deal with this type of issue?**

This is certainly a common issue within MLFE’s typer itself. I don’t currently have plans to add any specific error handling sugar since I worry that doing so might push people away from OTP and supervision hierarchies. Having said that, I don’t think it’s at all unreasonable to have a simple error type and handling alternatives in a library. While try/catch don’t exist yet in MLFE it could look like something as simple as the following:


    module simple_try
    export try_f/1, try_map/2
    type t ‘result ‘error = Ok ‘result | Error ‘error
    -- try to run f, wrapping successes in Ok and failures in    Error:  
    try_f f = try  
     Ok (f ())  
    catch   
     err -> Error err-- If t is a success apply the function f to    it or maintain a failed result:  
    try_map t f = match t with  
    Ok result -> let runner () = f result in try_f runner  
    | Error e -> t
