+++
title = 'a bit of information'
description = '"afdljal;dkjfalkjf" : that was very informative'
date = 2024-03-30
draft = false
+++
---
> 🎵 One pendrive is all it takes. Storing a bunch of me. 🎵 - Dua Lipa

**1.5 Gigabytes.** That is the space that your entire genome sequence will take if stored on my phone. *(yup, that's it)* Seems so counterintuitive that the amount of information present in a 2-minute video is much more than the information which defines your entire physical appearance.

Information is all around us. I believe that the reason mankind is on top of all species on Earth is because of our ability to crunch the information around us and make meaningful deductions from it.

- The sky is blue
- 2 + 3 = 5
- The CS245 Professor sucks

Pretty much everything happening around us will count as information. In short, it's an information overload out there. So how do we organize it all? What exactly is "information" in mathematical terms? Why should we even care? *(You don't need to, ECE majors may take up Information Theory Elective though)*

### what really is information?

What's the smallest piece of information that you can get? Well, the answer is:

> "Is it true?"

Is the sky blue? Is it grey? pink? Turns out the atomic unit of information is indeed a simple Yes/No question, or in other words, a binary (0/1) question.

Every piece of information is composed of bits. LITERALLY EVERYTHING.

- The characters that I am typing in right now are all decoded numbers; numbers which are all binaries under the hood.
- The grey color that you're seeing in the background is actually `RGB (29, 30, 32)`

. . . and the list can go on.

### quantifying information

Here is some information *(a fact actually)*:

**Shreya Ghoshal eats 100 Taylor Swifts for breakfast**.

So how informative is this? Are there any tools apart from language which can express the scale of information? How will the extremes of "no information" and "maximum information" look like? Is there a mathematical approach?

Consider these two images:

{{< figure src="image.png" align="center" width="600">}}

Now intuitively, one can say that the first image has little to no information. . . it's just yellow. Whereas, the second image has a lot going on; multiple colors, all dispersed in a **random** manner *(a lot of information?)*. Let's take another example.

~~~
1. 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 //rep(0)
2. 0 0 0 1 0 0 1 0 1 0 0 0 0 0 0 1 1 0 0 0 0 0 0 1 //one 1 in every 4 bits
3. 0 1 1 1 1 0 0 0 1 1 0 0 1 0 0 0 1 1 1 1 1 0 1 1 //wtf
~~~

We have 3 channels transmitting binaries. To fit in an analogy, we can say that the first image corresponds to the flat first channel and the second image corresponds to the noisy third channel. **So can we say that "information" is linked to randomness?** The greater the unpredictability, the greater the information?

The second channel is striking a balance *(moderate information)*. It's neither completely predictable nor completely unpredictable. It may correspond to this image:

{{< figure src="image2.png" title="iykyk" align="center" width="300">}}

Now some of you might claim that the image above is the most informative of all. I agree. This is because we humans will find meaning in things which are moderately informative (I am using the mathematical meaning of information). In short, nature loves balance.

It is understood that information is related to randomness or a more scientific term will be: ***Entropy***

### entropy?

The entropy @H(X)@, measured in bits, of a random variable @X@ with probability mass function @p(x)@ is given by

$$H(X) = -\sum_{x} p(x)\log_{2}{p(x)}$$

The entropy of the result of a coin toss is 1 bit, whereas that of a die is approximately 2.58 bits. Entropy measures, in a sense, how "chaotic" a particular event is. Reverting back to our images, we know for sure that every pixel in the first image is yellow or in mathematical notation: @p(x) = 1@ for all @x@ which gives us @H(X) = 0@.
Simply put, a highly predictable event will have low entropy, an unpredictable one will have high entropy.

Though it's majorly focused on data transmission and communication channels, the concepts of information theory find their applications in almost all fields. Consider this example:

Suppose that we have a horse race with eight horses taking part. Assume that the probabilities of winning for the eight horses are @( \frac{1}{2} , \frac{1}{4} , \frac{1}{8}, \frac{1}{16}, \frac{1}{64}, \frac{1}{64}, \frac{1}{64} , \frac{1}{64})@ and you are tasked to transmit the information about the winning horse using binary code. How will you do it?

The naive way to do this will be to use 3 bits to transmit the message. However consider these strings:

$$ 0, 10, 110, 1110, 111100, 111101, 111110, 111111 $$

The average length of strings in this case is just 2 bits which is also equal to the entropy of this event. Coincidence? No. It can be proven that the value of entropy is the lower bound for the average length of strings required. This also explains the fact that the Morse Code for the most frequent letter i.e. 'e' is the shortest.

Information Theory goes much deeper than this.

Hope you found this post informative (even a bit would do).
