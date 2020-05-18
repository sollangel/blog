## We interviewed its creators

Shells have been around forever and, for better or for worse, haven’t changed much since their inception. Until [NuShell](https://www.nushell.sh/) appeared to reinvent shells and defy our muscle memory. It brought some big changes, which include rethinking how pipelines work, structured input/output, and plugins.

We wanted to learn more about NuShell so we interviewed both of its creators: [Jonathan Turner](https://twitter.com/jntrnr) and [Yehuda Katz](https://twitter.com/wycats).

----------

Join the Not a Monad Tutorial Telegram [group](https://t.me/notamonadtutorial) or [channel](https://t.me/channel_notamonadtutorial) to talk about programming, computer science and papers. See you there!

----------

_If you are looking for good engineers send me an email to mail@fcarrone.com or you can also reach me via twitter at_ [_@federicocarrone_](https://twitter.com/federicocarrone)_._

----------

## **Why create NuShell? Is it me or does it have a next-level AWK vibe?**

**Jonathan Turner**: Sometimes the simplest ideas are the ones that hook you the most :). When Yehuda and I first started discussing how shells could be improved, we settled on the idea of using structured data rather than just text between applications (for example stdin/stdout). He had just been experimenting with PowerShell and saw how adding some structure to the data opened up a lot of possibilities.

The basic idea is pretty simple: Nu opens everything into a table you can work with. Files, streams, commands like ls and ps all output this one table format. Then you have a set of commands that work with these tables, to help you get the data you want, change it, view it, etc.

Funny that you mention “awk”. In a way, Nu is a way of saying “what if we didn’t need tools like awk so often?” Since you’re working with structured data, as we add more support for file types, it’s less often you need to reach for “awk”, “jq”, “grep”, and the array of other tools to open and work with common file types.

In a way, it’s taking the original spirit of Unix — where you use pipelines to combine a set of tools — and imagining how that original spirit would work today, with what we know about programming languages and tools. And, being crossplatform, it’s nice to learn this approach and then be able to easily switch operating systems and use the same techniques.

![](https://miro.medium.com/max/560/1*u77ccTVL0VJ7Cw2xT_7puA.png?q=20)
NuShell screen capture
