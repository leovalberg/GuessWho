# GuessWho

Teaching a seven year old the problem with being an optimist

Using Monte Carlo and Decision Trees

## Spoiler alert

![](images/hth3.png?raw=true)

## The motivation

Playing GuessWho with my seven year old son, I was unable to convince him to that his “risky” strategy was just not working.

I thought this would be a good opportunity to teach him a bit of coding and an important lesson in life.


## The rules

![](images/guesswho.png?raw=true)

If you don’t already know:

* GuessWho is a board game where each player is trying to guess which of the 24 characters the other player is. 

* There is a picture of each characters face. By asking “yes-no” questions, such as “are you wearing a hat?” you try and eliminate the characters until you are left with only one face remaining.

* The first player to correctly GuessWho wins.



## The problem

Some questions will eliminate around half the cards in one go. Other questions will, if you are lucky, leave you with only a couple cards and almost guaranteed victory.

But the flip side of this approach is that you are much more likely to only eliminate a couple of cards and be left not much better off than you were before your turn. That is why this high risk approach is correctly termed “optimistic”.

The most “optimistic” approach would be to think you could guess the name outright, without any other questions first. But even my son recognised this was a losing bet.

With an understanding of Binary Search and the mounting evidence, I was pretty certain that my strategy of playing safe and eliminating close to half the cards in each turn was better than my son’s all or nothing approach.
However O(log n) wasn’t cutting it with him and I wasn’t totally sure that with handpicked features of each character the correlations wouldn’t change this.

So I got my son to help me mock a Monte Carlo simulation of GuessWho.


## The method

![](images/excel.png?raw=true)

We encoded the cards into an excel/csv file. With a 1 or a 0 answer for each question. 
And then developed three strategies:

* Safe: Always ask the question that is guaranteed (in the worst case) to eliminate the most cards
* Random: Pick a random question from the unasked questions
* Risky: Pick the question that in the best case will eliminate the most cards

There are other strategies, I will cover another one shortly. But these are 3 easy rules of thumb that a child could follow.

We then ran 1000 simulations for each strategy.

We didn’t include all possible questions but had enough so that each card had a unique solution. 

I won’t go into any details here about the relative frequency of blonde hair to brown hair, there are other [blogs](http://chalkdustmagazine.com/blog/cracking-guess-board-game/) that do this. And as it turns out there are many different versions of the game with different characters.

With the questions we had, all cards could be guessed with two or three questions. We did not include the possibility of asking more complicated questions such as “do they have brown hair OR wearing earings”. This is covered [here](https://lifehacker.com/almost-always-win-the-game-guess-who-with-this-math-bas-1743859796).



## The results


![](images/dist3.png?raw=true)


You can see there is a clear advantage to playing it safe. On average guessing who, after 7.3 rather than 8.4 turns for the risky or optimistic player. And this 10% improvement in number of turns to guess who for the safe strategy translates into a 50% increase in games won over the risky strategy

Because we have cross correlations between the questions e.g., all bald cards are men, the number of turns to guess who is less than we would expect from purely uncorrelated questions. In the uncorrelated case we would expect around 15 turns to guess who (see the notebook for more details).

**However we also see that the safe approach never gets a really quick win in 2 or 3 turns, the way the random approach does.**



## Decision Tree Classifier

The problem with the simple and safe approach is that is does not think ahead. But thinking ahead requires comparing 10 of thousands of different possible combinations of questions and the best question to ask next will depend on the previous answers.

A defined order of questions and what question to ask next depending on the previous answers is called a decision tree.

And luckily there are a number of very fast algorithms that will optimise the order of the questions, so that the average number of questions you will need to ask is as low as possible.

![](images/dt.png?raw=true)
[image source](https://towardsdatascience.com/decision-tree-hugging-b8851f853486)

Implementing the sklearn Decision Tree Classifier (DTC) we can compare the results with the 3 basic heuristics we have already looked at.

![](images/dist4.png?raw=true)

The DTC absolutely smashes it. Guessing who on average two and half turns quicker and beating the optimistic strategy 94% of the time.
But by now my son has lost interest and been playing Lego in the other room for a quite a while. I better go and see what he has made.

![](images/hth4.png?raw=true)



---

The code for all these results and some additional results on random decks of cards of various shapes and sizes can be found in the jupyter notebook.

The simulation take a while to run, so I have provided the results in the data folder if you want skip those steps to save some time.

I think all the libraries used came preinstalled with Anaconda.
