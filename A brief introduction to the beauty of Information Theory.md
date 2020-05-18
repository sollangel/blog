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
