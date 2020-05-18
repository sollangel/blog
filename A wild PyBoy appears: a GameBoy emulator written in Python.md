# A wild PyBoy appears: a GameBoy emulator written in Python

Most of us would be lying if we said the idea of making an emulator hasn’t crossed our mind, but usually when you start digging into everything that is involved it becomes a bit overwhelming and the idea stops sparking joy, so we ditch it as soon as some other shiny thing appears. On the other hand, we have the brave people that take on this task and work on it until completion (or at least until having something decent enough to show) which by itself is an amazing accomplishment.
Most emulators are written in a system programming language (C, C++, Rust, etc), but every now and then we find a hidden gem, not only using a different language, but that evolved into something more. In this case, we found [PyBoy](https://github.com/Baekalfen/PyBoy) a GameBoy emulator written in Python with a new found focus on making the external components available to bots or AI.

We interviewed one of the creators of PyBoy, [Mads Ynddal](https://github.com/Baekalfen).

Join the Not a Monad Tutorial Telegram [group](https://t.me/notamonadtutorial) or [channel](https://t.me/channel_notamonadtutorial) to talk about programming, computer science and papers. See you there!

                                                       ---

_If you are looking for good engineers send me an email to mail@fcarrone.com or you can also reach me via twitter at_ [_@unbalancedparen_](https://twitter.com/unbalancedparen)_._

                                                       ---
                                
![](https://miro.medium.com/max/519/1*4RLyE-WB7MXRGjMhxm7L8w.png)
Credit: dayoftheshirt.com
## Why make a GameBoy emulator? For the time being, this is only for GameBoy not GameBoy Color, correct?

I’ve always been interested in emulation and hardware architecture. Throughout my childhood, I had somehow stumbled into almost every kind of emulator. Even consoles that I was too young to have had myself.  
So from one of the first years of university (I studied Computer Science at the University of Copenhagen), I had seriously been looking at Game Boy emulators, and particularly Gearboy. I even did a poorly written PR for a save/load state feature for it. But I never finished it.

At some point, I convinced two of my friends at the university to join me in making an emulator. We didn’t even consider other platforms. I had done a bit of research, and because of the age of the platform, there is plenty of documentation readily available. I think that might be what have settled the decision very quickly.

It’s only the original Game Boy at the moment. We hope to expand to Game Boy Color very soon. Especially, if the new interest might draw someone in, who wants to help.

## What is the process of making an emulator like? What would you say is the hardest part?

We had the benefit not to have to reverse engineer anything to write the emulator. People have been working on it for 30 years, and have been really good at sharing that knowledge. So it was very similar to doing work based on some insanely specific requirements specifications. Whatever the documentation states, that is what we implement. Bugs and everything included.

An entire platform like the Game Boy is impossible to make in one go, so the process is to make single components by themselves, and hope that everything will line up when they are brought together. We started with the CPU and memory, and worked our way out from there.

Because of the many years of reverse engineering work, there are also a bunch of test ROMs available. One we have been using a lot are the “Blargg” test ROMs. They are “game” ROMs that you start on the emulator and it runs the emulator through a bunch of tests and tells which opcodes* might have problems. This test ROM even works before implementing any graphics, as it will write the output through the Link Cable.

Although it’s good at telling you that there is a problem, it’s still a lot of work to read documentation and debug to figure out what is missing to make the test pass. But it’s definitely an incremental process. You keep adding small things, discover bugs or glitches, and fix them, while the emulation keeps getting slightly better. But counting the amount of hours of work for the benefit clearly has diminishing returns. But it’s a excellent feeling, when a really obscure bug gets sorted out. And that makes you go and fix the next one.

I don’t know what the hardest part was, but getting the opcodes to work is not easy. It’s easy enough to program things like an add, sub, cmp opcode and so on. But there are a trillion small things that need to be handled. Carry, half-carry, DAA (Decimal Adjust after Addition) and all kinds of different conditions that apply in some cases but not all.

## How does one go about debugging an emulator?

It’s a lot of detective work, and writing debugging tools for yourself. Multiple times, I have found myself chasing bugs in a game, with a notepad and pen to write down addresses and memory values to try to see why a loop became infinite, or why I could one go up or down in Pokémon, but not left or right (it was a missing flag in an opcode).

It can easily take an entire day to find the cause, if not more.

## Any tips on how to handle the implementation of the hundreds of opcodes? I saw that you use some sort of code generation, care to explain it?

My best tip is to not do all the typing yourself. If somebody has made documentation on the opcodes, try to extract them directly. Because it’s easy to get lazy and start making assumptions. But you’ll find that opcodes — especially the flags — are very tricky. For example, assuming the flags that will be set after an add. It’s entirely dependent on which registers the numbers code from.

The code generation is quite simple. It reads through a website with a table of every opcode. For each cell, it identifies the type of operation — for example an add — and then the operands. Depending on the size of the operands (16-bit or 8-bit) and whether the second operand is a register or a literal, it will go to different code generating routines.

Having this strict process helped uncover a lot of mistakes I would otherwise have made, if I was typing by hand.

## Does the difference in speed between the host CPU and the emulated CPU cause any issues?

It causes a lot of problems. The games were programmed for a very specific clockspeed, and won’t necessarily accept any other clockspeed. There’s also the problem of synchronizing the screen buffer with the host computer. The Game Boy will always output exactly 60 frames per second. No matter what is shown on the screen, it won’t take any longer or shorter to produce the frame.

A naive approach is to make each opcode take the time that it would on a real Game Boy. The problem is, that it’s very hard to control modern computers down to the 1/1,000,000 of a second. Even controlling modern OS’s to accurately pause for 1/100 of a second (10ms), is close to impossible. So the approach of limiting each instruction is not feasible. We need to control something less frequent.  
Therefore we ended up limiting the output of frames. A quick hack is to enable V-Sync and have the host’s monitor operate at 60hz as well. This might have worked in the past, but it’s getting more and more common to have more than 60hz screens. Thankfully, SDL2 has some functions to make these limitations. But this means, that we are right now limiting the speed entirely on the frame output. This works out well, as it is more or less the only way that users get output from the machine.

This creates a time of time-warping between the real world and the emulated machine. We calculate the entire frame’s worth of CPU cycles in a fraction of what it would normally take. Then we wait until the next frame is needed, and we repeat.

This has the downside that we cannot do sub-frame input, as we only poll SDL2 for events once a frame. But we are considering a way to record the timing of input, and “replay” it at the right point of the frame calculation, to make it as if it was happening in real time.

We have several of these time-warping tricks in the emulator.

## Why use Python instead of something like C/C++? Any challenges or benefits related to your language or library choices?

It’s without a doubt harder to make a Game Boy emulator in Python,= than in C/C++. But it’s OK with a bit of challenge. And it seems like we are the only people to have pulled it off. It’s also nice to show the C-fanboys, that Python can also be applied in “high-performance” applications.

It enables us to work out prototypes much faster, and when things are working, I go and make the type definitions, to get the speed.

Usually I develop new features using PyPy, and when I know it works, I go through the process of making it work in Cython. Knowing that the code itself is working, makes it a lot easier to work with Cython, as I know any bug will be related to some variable being the wrong type.

## Another unique thing about PyBoy is that it allows to make bots or AI for it. Was this something you wanted from the start or something you added later? Do you know any interesting projects that make use of this feature?

It was added later. In the beginning, we just wanted to make an emulator. If we had planned to make it focused on AI/bots, we would probably rather have adapted an existing emulator.

We have seen some cool projects already. Before making v1.0.0 public, there had already been some people trying to make an AI for Super Mario Land. And there was a project to make PyBoy run in Discord in the style of “Twitch Plays Pokemon”.

After releasing v1.0.0, I have already seen someone make a script that dumps the entire map of one of his favorite games. This is done by tracking the tiles (part of the 2D rendering), and put them in a big image of the entire area. The bot-support we have been implementing, makes this super easy, as the developer has to know very little about the Game Boy hardware, and just use the PyBoy interface. We provide functions that will convert the internal graphics format that the Game Boy uses (16 bytes for an 8x8 pixel 4-four tile) to a PIL.Image in RGB format.

## Do you have any recommendations for anyone looking to make an emulator?

The [report](https://github.com/Baekalfen/PyBoy/raw/master/PyBoy.pdf) we wrote for the first version of PyBoy is still in the GitHub repo. People should definitely take a look at it, for a light introduction to emulation. For the more specific documentation, I can recommend the [Pan Docs](http://bgb.bircd.org/pandocs.htm).

[](https://notamonadtutorial.com/?source=post_sidebar--------------------------post_sidebar-)



   . . . 
-   [Python](https://notamonadtutorial.com/tagged/python)
-   [Emulator](https://notamonadtutorial.com/tagged/emulator)
-   [Game Development](https://notamonadtutorial.com/tagged/game-development)
-   [Gameboy](https://notamonadtutorial.com/tagged/gameboy)
