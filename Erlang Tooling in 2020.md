# Erlang Tooling in 2020

![](https://miro.medium.com/max/460/1*dSvpbd5KfoJA6FJUCV0JbA.jpeg?q=20) 
The Henry Studley carpenter’s tool chest — 1838 

The _vox populi_ is that Erlang tooling has always been lagging behind other languages’, and that when Elixir came along, it somewhat forced the community into action. Whatever the exact causes, it’s true today that Erlang has been benefiting from cross-pollination with the new communities that are building systems around the BEAM platform.

One of these fruits is the new rebar3 plugin adding a formatter to Erlang. The project’s initiator, Brujo, wrote a [blog post](http://tech.nextroll.com/blog/dev/2020/02/25/erlang-rebar3-format.html) when it was presented. We asked him some follow-up questions.

Another sorely needed piece of tooling was a working implementation of a [Language Server Protocol](https://en.wikipedia.org/wiki/Language_Server_Protocol) server for Erlang. Several efforts have been underway, but in particular [erlang-ls](https://erlang-ls.github.io/) ([Github](https://github.com/erlang-ls/erlang_ls/releases/tag/0.1.0)) headed by Roberto Aloi has reached 1.0 recently.

Given this serendipitous situation where two important developments for Erlang were launched at the same time, we decided to do a joint interview with both Brujo and Roberto.

# Brujo

# What led you to the decision to start working on an Erlang code formatter?

Almost since I started working as a developer, I’ve always cared a lot about software maintainability.

That’s why I generally advocate for the usage of tools like xref, dialyzer, etc.

A while back, while working at Inaka I learned about Hound for Ruby. I wanted something like that for Erlang and found nothing similar. Therefore I worked with Juan Facorro and other inakos to create Elvis to fill that void.

In the same way, since I learned about _mix format_ I started looking for a similar tool for Erlang. I knew that I could not create such a tool on my own. So I presented it as a project idea for Spawnfest (twice) with no luck. But then, the opportunity presented itself: HackWeek at NextRoll. It’s a week-long hackathon that happens twice a year. I presented my project and Juan Bono and Diego Calero were thrilled to help, so we built a team and in a week we had our first MVP, which was even published in hex.pm and all.

# Do you have any thoughts on why Erlang is a latecomer to the code formatting game?

I believe that, except for languages that are generally ahead of its time (like Smalltalk), having a code formatter that everybody uses is a relatively new thing. It’s something that’s _expected_ from a modern language now, but that was not the case 10 or even 5 years ago. On the other hand, getting traction for an _official_ formatter for a new language (like Elixir or Go) is not hard, since there are not many huge codebases around and people didn’t have enough time to get _attached_ to their own formatting style yet.

For older ecosystems, like Erlang, where we have impressively huge repositories with widely different code styles (even within themselves), convincing developers to start using a single _universal_ code format is not easy.

And the fact that different editors (like Intellij, emacs, etc.) provide their own auto-formatting tools, each one producing code in their very very distinct style that shares nothing with the others makes it even worse.

# What did the design process for the tool look like?

Well… It was a hack week, right? It was 90% trial & error. Or, more accurately: digging, trial & error.

Jokes aside, we knew what we wanted from the very first day: We were going to create a tool capable of parsing, formatting and not breaking Erlang code. We didn’t want the formatted code to be **pretty**, we just wanted it to be **consistently ugly**. And we achieved that: as I said before… you can check that version (0.0.1) on hex.pm.

To achieve that goal, we wanted to use as many existing tools as we could find (we didn’t want to reinvent the wheel). That’s when Juan Bono came in. In pure hackathon-style, we were working on different time-zones. I was in Barcelona and Juan was in Buenos Aires. By the time I was done for the day, his day had just started. So I told him, for instance: “We need a tool to parse Erlang terms and another one to print them out”. My next day started with a message from him like “I found epp to parse and erl_prettypr to produce the formatted output”. I implemented our solution with them and came back with “But epp doesn’t handle macros well” and “erl_prettypr is very limited”, so he dug up stuff again and told me about epp_dodger and the many different reimplementations of erl_prettypr in different projects… trial & error again… and the process repeated.

At the same time, Diego was constantly trying to find strange edge cases to break our formatter and he found some **very very strange stuff**, too. Check the test app on the rebar3_format repo and I promise you, you will say “but… you can actually do that?” more than once.

# How does the Erlang formatter compare to other well-known formatting tools (e.g. go fmt, mix format, or rustfmt)?

On one hand, rebar3 format is still much younger and less battle-tested than those tools.

It’s also not _the official formatter_… yet? Hey! OTP team… if you read this maybe it would be a good idea to have a conversation ;) — I’ll visit Stockholm soon, just saying…

On the other hand, the tools that you mention (particularly mix format) are much more _strongly opinionated_ than rebar3 format. There is practically just _one way_ to format code with them.

Knowing that it will be very very hard to convince every Erlang developer in the world to format their code in the same way, we aimed at a more humble goal: To convince every Erlang developer in the world to format their code **with the same tool**. You can keep your style, any style, but you use rebar3 format to apply it to your whole project at once. That way, reformatting your code in _my style_ is just a matter of running a single command on the console: _rebar3 as brujo format_. I’m a ROK-style comma-first fan after all… and that’s certainly not the default formatting style ;)

# Which were the main difficulties you found on the way to developing it?

I think the main issue we had was the lack of clear documentation and extensibility around existing tools. The work Juan did finding very obscure OTP modules with names like _epp_dodger_ that actually had functions that resembled what we actually needed or were close enough was invaluable. Much of the code was there, but it was invisible for the unprepared eye.

If all those tools, like epp_dodger, erl_prettypr, etc were more visible and extensible, this formatter would’ve been a reality many years ago.

# Handling comments along with valid code is a common difficulty that code analysing tools must deal with. How does the Erlang formatter handle this?

Luckily for us, the tools provided by OTP already did most of the work. We need a few patches here and there, but it was mostly trivial stuff. On the other hand, there are still some comments that rebar3 format can’t handle in the most beautiful manner. Rebar3 format won’t lose any comment in your files, but it _might_ misplace them a bit, particularly if you want to add comments in things like type fields. Nothing that can’t be solved by manually moving the comments to a better place in the file.

# Likewise, how does the code formatter handle incorrect code?

Since we use the same tools that the compiler uses to parse code, if the formatter finds syntactically incorrect code, it fails just like the compiler would. On the other hand, since the formatter doesn’t process macros, if your code is invalid **because of a macro** then the formatter will format it anyway. Even further, syntactically correct but semantically incorrect code (e.g. functions that are unused and not exported, variables that are used before being bound, etc.) is just formatted anyway.

What the formatter actually does, and we learned that from @old-reliable’s steamroller, is to verify that the AST generated from the code is not affected by the formatter itself (i.e. that the AST generated from the code **before** formatting it is exactly the same -except for the line numbers- as the AST generated from the code **after** running the formatter on it).

# How configurable is the code formatter?

A lot. In basically two levels:

-   First, you need to choose what formatter you’ll use (i.e. what implementation of _rebar3_formatter_ behavior will format your code). Rebar3 format comes with two by default: _otp_formatter_ (basically, _erl_prettypr_) and _default_formatter_ (NextRoll’s preferred way of formatting code).
-   Then each formatter has its own list of options to use, the default formatter has approximately 10 different options you can tweak, but the OTP formatter only allows you to change _paper_ and _ribbon_.

Of course, you can create your own formatter, too. You just need to implement the _rebar3_formatter_ behavior. We accept contributions! If you create a ROK-style formatter, **please** send a pull request with it!!

# Which obstacles to adoption do you perceive? Or, What kind of adoption do you expect?

Oddly enough, the biggest obstacle now will likely be the _competition._ As Daniel (@old-reliable) puts it: ‘_Do you think that this time will be remembered as “The Great Formatter Wars”?’_ For ages, there were no formatters for Erlang and many people complained about that. But now, there is not one, not two, but soon there will be **three** of them: rebar3 format, steamroller and one that Michał Muskała will soon present.

In any case, we’re not sad, we believe that having three different formatters is far better than having none of them ;)

# What do you think is in the future for the Erlang formatter?

It’ll become the _de facto_ formatter for all Erlang code in the whole wide world, of course! You have to dream big, right?

Seriously, my greatest dream for the formatter would be for it to be considered the default Erlang formatter. Maybe the OTP team adopts it and starts requiring all contributions to Erlang/OTP code to be formatted with it, etc. etc. …

More realistically, though… I have to say that we plan to use it for all of our Erlang projects within NextRoll. We are already using it for all the open-source ones and we’re progressively adding it to our private ones that are much more massive. Keeping that in mind, we (as a company) are committed to providing support for it **actively**. And we expect some help from the community in the form of bug reports, configuration and formatter ideas and… of course… pull requests!

# Now that the Erlang formatter is usable, what else are you up to?

I think that my main focus these days is the Education Working Group in the Erlang Ecosystem Foundation. As an active member of it, I’m now working on an initiative called _Meetup Interventions_. It’s in its early stages, but the idea is to help meetup organizers around the world to provide workshop sessions with training in different subjects, some introductory, others more advanced. The first round of workshops is about to start very soon with Miriam Pena, Simon Thompson, Johnny Winn and me as trainers. We’ll see how that goes and then we’ll decide how to properly scale up the idea. I think it will be a great success.

Besides that, I recently moved to Sant Cugat del Vallès, a beautiful town near Barcelona, Spain which gives me access to many European things. I’ll be giving a talk at CodeBEAM STO in May and before that, I’ll be attending ElixirConf EU. I’m also helping some new friends revitalize the Elixir meetups at Madrid and hopefully visiting A Coruña’s university soon to give a guest lecture, too.

I’m very excited about all the opportunities that are now much closer to me. I’m still just settling in my new home, but I already love it!

Finally, I would like to mention that even when I was not particularly active in Erlang Battleground lately, I’m still looking for writers… If you happen to have an idea of something BEAM-related you want to write about, contact me… I’ll help you get it published and shared with a lot of people!

# Roberto Aloi

# How and why did the project start?

My original plan in 2018 was to simply start contributing to the [EDTS](https://github.com/sebastiw/edts) project, the _Erlang Development Tool Suite_ for the Emacs editor. The project was not actively maintained any longer and I was experiencing a few issues after upgrading to the latest version of Erlang/OTP.

I started diving into the EDTS code-base and discovered that its architecture was particularly interesting. The Emacs client, implemented in Emacs Lisp, was communicating via HTTP to a server, implemented in Erlang, which was responsible for the heavy-lifting. I played with the idea that, with a bit of effort, one could grow the HTTP API that was used for the client-server communication in EDTS into something more solid and generic, allowing other editors to re-use the logic implemented by the EDTS server.

Some weeks after I was at the Code Beam Stockholm conference. I attended a talk from the author of the [Sourcer](https://github.com/erlang/sourcer) project, who gave an introduction to the [Language Server Protocol](https://microsoft.github.io/language-server-protocol/). Apparently, Microsoft had already thought about this. To me it meant that I didn’t have to go through the process of designing a brand-new protocol, but that I could simply focus on implementing a server, maybe based on the EDTS one, which would comply to the protocol. I started looking at existing Language Server implementations in Erlang and I guess that leads to your second question…

# How does it compare to other similar efforts such as sourcer or other LSP implementations?

There are currently three major implementations of the Language Server I am aware of:

>-   [Sourcer](https://github.com/erlang/sourcer)
>-   [VSCode Erlang](https://github.com/pgourlain/vscode_erlang)
>-   [Erlang LS](https://github.com/erlang-ls/erlang_ls)

