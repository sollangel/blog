# A brief introduction to the beauty of Information Theory

## Or how to be a hardcore Guess Who gamer


_Authors: Juan Pablo Amoroso, Javier Rodríguez Chatruc, Camilo Plata, and Federico Carrone._

> *The fundamental problem of communication is that of reproducing at one point either exactly or approximately a message selected at another point.  
> — Claude Shannon, 1948*

Imagine you were tasked with designing a comunications system between a space station and a ground control headquarters back in Earth. The system would transmit and receive messages encoded in binary, that is, as a sequence of 1s and 0s. As the message travels, there may be interferences from other radio signals, so that what is picked up in ground control is not exactly the same as the original message. Under these circumstances, is it possible to devise a scheme that allows reliable comunication?

A simple workaround would be to add redundancy: send each bit a number of times, let’s say 5:

    11111  
    00000  
    …

If ground control receives the message _11101_, they could be fairly certain that what was truly sent was _11111_. Although this simple system would work (up to a point), we can see that it is very wasteful: we have to send 4 extra bits for every bit in the original message. The _transmission rate_ is therefore only 20%. Can we do any better?

    There seems to be a dilemma here: if we want accuracy, we must lower the rate of transmission.

This is the problem Claude Shannon tackled in his 1948 paper _A Mathematical Theory of Communication_. In it, he proved that there is a limit for the rate of information that can be reliably transmitted over a noisy channel (the _Shannon limit_). However, below this limit we can transmit information with an increasingly small error. This important result tells us that there _exists_ a code that allows arbitrary accuracy over a given comunication channel, but it does not tell us how to build it.

This is the problem Claude Shannon tackled in his 1948 paper _A Mathematical Theory of Communication_. In it, he proved that there is a limit for the rate of information that can be reliably transmitted over a noisy channel (the _Shannon limit_). However, below this limit we can transmit information with an increasingly small error. This important result tells us that there _exists_ a code that allows arbitrary accuracy over a given comunication channel, but it does not tell us how to build it.

More precisely, let’s say a channel has a probability _p_ of transmitting a bit correctly, and a corresponding probability of 1 — _p_ of sending the wrong bit, Shannon proved that the optimum rate of transmission is:

![](https://miro.medium.com/max/360/1*kCNAUFlhWJv1kUzKatqINA.png?q=20)

![](https://miro.medium.com/max/560/1*TgCse1znfuWYULYwXeo4Aw.png?q=20)

The plot is symmetrical around _p = 0.5_, with maxima at _p = 0_ and _p = 1_. The case of _p = 0_ is interesting, the channel has perfect noise: it flips all the bits in the original message. But if we know that, then the message is trivially deciphered, we just flip them back.

The formula is commonly stated in terms of [information entropy](https://en.wikipedia.org/wiki/Entropy_(information_theory)), a measure Shannon devised that can be interpreted as the level of ‘uncertainty’ or ‘surprise’ associated with the channel.



![](https://miro.medium.com/max/560/1*fCyO-nZidJEiOqwqDGWJ3g.png?q=20)

![](https://miro.medium.com/max/560/1*s0z3MUCTtJvh_AsvxGg72g.png?q=20)

We can see that the entropy has a maximum at 1 when _p_ = ½, and minima at 0 for _p =_ 0 and _p =_ 1.

More generally, given a random message _M_ that can take _n_ different values with probability _pᵢ_ for _i =_ 1,…,_n_, we define the entropy of the message as:


![](https://miro.medium.com/max/460/1*ezweLprVK1INseQwDCN2yg.png?q=20)

## Guess Who example

Let’s take a different approach. Suppose you are playing _Guess Who_, the game where you ask yes/no questions about the appearance of your opponent’s character in order to single him or her out among a set of characters. You ask yourself: what order should I ask the questions in to maximise the probability of winning? Intutively, you try to ask first about features most of the characters have.



![](https://miro.medium.com/max/560/1*TkW9quvg52IBgM06fM-7IA.jpeg)Hardcore Guess Who gamers apply Information Theory for optimal results


