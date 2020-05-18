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

If ground
