+++
title = 'raise > adieu > pzazz'
date = 2024-07-08
draft = false
+++
---
Puzzles slipped into my daily routine pretty smoothly. I don't remember how I got into this in first place and it continued growing on me, day by day, slowly and steadily. Here is a stat describing the gravity of my current situation:

> I solve 8 different puzzles[^1] every single day.

Is this something to be proud of? Definitely not. Do I want to change it? Absolutely not ! It all started with Wordle back in Dec 2023 and I was also able to reel Tanush in it as well. We'd compete with each other *(ik, sounds very lame)* and discuss how we'd solved that day's puzzle.

One afternoon, he comes into my room... 

Him : Bro, let's make a wordle bot. \
Me : \*thinks\* Yeah, let's do it. Sounds fun.

Then we spent a while discussing on how we can actually make one. Initially he had a very naive brute-force approach but I suggested him to have a look at this 3b1b [video](https://www.youtube.com/watch?v=v68zYyaEmEA) for some inspiration which I vaguely remember watching when the game was trending on twitter (sometime in 2021).

I kind of faltered after this. It didn't look challenging at all. Grant made it all look so easy.

Tanush would often bring this up in between our conversations, asking me when should we start and I kind of didn't pay any heed.

"this weekend", "after that quiz", "once we get GTA VI"

The string of excuses was long. However, all I needed was a little push.

> On 11th April 2024, I lost my 89 day streak to the word **LOUSE**. With mere 11 days off a century, it was a devastating feeling.

{{< figure src="image.png" align="center" width="200" title="louse for the lousy">}} 

I had to solve this game now !

### DIGIT
A few numbers about the game. @12953@ words are allowed as guesses; of these, @2309@ are possible solutions to a game. Why not all 13k can be answers you may ask. Well, how many of the words given below do you know?

$$ \text{ZOOEA, XEBEC, TYIYN, QOPHS, MYRRH} $$

No, this is not some gibberish which I made up, but are actual words which you can guess while playing the game. However they are not one of the possible solutions. *(Imagine losing your streak to TYIYN)*

The game can be played in two modes : Easy (you can guess any word) & Hard (you must utilise previous hints)

After you play a word, you can get one of @3 \cdot 3 \cdot 3 \cdot 3 \cdot 3 = 243@ possible color combinations. This information will come in handy later :)

### DRAFT

At first, I assumed that coding out the working and logic of the game shouldn't be difficult and I should worry about the algorithmic part first. This is what I came up with:

* Let @G@ be the set consisting of all allowed guesses and let @S@ be the set consisting of all possible solutions.
* @\forall g \in G, @ get the corresponding color @\forall s \in S@ and then find @|S_{new}|@ based on hints in the color pattern
* Use the @|S_{new}|@ generate the distribution of the solution space.

{{< figure src="slowalg.png" align="center" >}}

I understood that this was kind of inefficient but didn't bother myself about it at that time and went to code out the logic. Little did I know, that would turn out to be a nightmare.

You may be thinking... "how hard can it be?" Let me present you with a challenge...

> How will YOU assign colors when you have an answer and a word guessed by the user?\
> Think about it. Construct some solution before scrolling down

So this was my first algorithm:

* Get the letter counts of each word as dictionary
* Assign Greens and Greys first
* Assign Yellows based on letter count

The need to count letters arose because of the case of repeating letters. Words like @\text{STASH}@ and  @\text{LEVEL}@ were causing issues otherwise.

So, I got busy coding out the logic and after I was done, it didn't take me long to realise that...

*It was dead slowwwww*

It was taking 5-6 seconds to process a single word. JUST ONE WORD !!!\
At this rate I was not getting anywhere. \
Forget simulations, even solving a single game seemed a distant dream.

### SPEED

As many of you might've noticed, the algorithm was a primary reason of the bot choking. It required @\approx 2.5 ~ \cdot 10^7@ computations [^2] for a single word. Now, multiply that by @12953@...

Then, it clicked me. Instead of iterating over the solution space, then getting the colors, then dividing the solution space, I could've just iterated through all possible color combinations and then found the required distribution.

{{< figure src="fastalg.png" align="center" >}} 

The computations for a single word were now reduced to @\approx 2.8 \cdot 10^6@. This was slightly better. However, there was still room for improvement in the mechanism to get @|S_{new}|@. \
I believe that in order to improve speed, you can make two fundamental changes to your program:

* Improve the algorithm *(very taxing, you (most probably) won't succeed)*
* Store some data *(consumes memory, but very easy)*

I, without second thoughts, went with the latter. I created dictionaries based on the color combinations. 
* `green_dict` stored all the words which gave green for a particular letter at a particular position. 
* `grey_dict` stored all words which gave grey for that word.
* `posgrey_dict` all words which did not have a particular letter at a particular position.
 
These accelerated the runtime. I then utilised this momentum to create a ginormous (623 MB) matrix, which stored all the words which gave a particular color pattern with a particular word. In short, once loaded, this matrix can return the whole set of words which satsify a particular condition in @O(1)@ time. 

Neat.

### SCORE

I had the word-color pattern distribution on me now, which meant that I can know the exact number of words (the "split") my solution set will be reduced to if I play a particular word and it shows a particular color pattern.

So how do I use this to pick up the best words? I stuck with two strategies:

* Min-Max : Minimise the worst case split.
* Avg-Split : Minimise the average split size.

And these were the simulation results (Hard Mode with @\text{RAISE}@ [^3] as starter):
$$
\begin{array}{|l|l|l|}
\hline
  \text{Algorithm} & \text{Success \%} & \text{Avg. Moves} \\\\
\hline
   \text{Min-Max} & 99.437 & 3.658 \\\\
\hline
   \text{Min-Avg} & 99.350 & 3.633 \\\\
\hline
\end{array}
$$

And these were the Easy Mode sim results:

$$ \text{computationally challenging !} $$

Yeah. Ironically, Easy Mode is tougher to simulate than Hard Mode. The reason for this is that in Hard Mode, the set @G@ reduces in size with each subsequent move, whereas in Easy Mode, it stays the same (all @12953@ words). This put a speed bound and each game was taking, on average, 30s to simulate in Easy Mode (which is not desirable when you have @2903@ words in queue !)

### FLAWS

The bot I have made is far from perfect.

* TARDY : Unable to simulate easy mode
* BULKY : Thanks to that nasty matrix
* GREED : Optimises on the next move, rather than the whole game.

If anyone's interested, here is the code repository : [Mind-Your-Wordle](https://github.com/u-d-ash/Mind-Your-Wordle)

I will continue working on it because this is far from finished.

### ADIEU

Here is a fun comic by [@tomgauld](https://www.instagram.com/tomgauld/) :

{{< figure src="comic.png" width="400" align="center">}}



[^1]: **NYT** : Strands, SpellBee, Connections, Wordle, Letterboxed, Mini; **LinkedIn** : Crossclimb, Pinpoint
[^2]: @|S| \cdot 5 \text{ (to get the color)} \cdot |S| \text{ (words in } S \text{ matching the color)} : 2309 \cdot 5 \cdot 2309 = 26657405 \text{ (worst case)}@
[^3]: I tried the same with @\text{PLATE}@ and the results were better. However that was not the recommended starter according to my bot !
