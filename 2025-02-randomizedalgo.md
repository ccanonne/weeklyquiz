# Quiz 2: Carol and Dave have a random fight

_See the [Quiz on BlueSky]()._

## Backstory

 🧩 📊 Carol and Dave are in a pickle. They are scheduled to TA a class on randomized algorithms in a few minutes, and they did not have time to watch the lecture beforehand! Even worse, they never took the class before....

They don't even agree on what a randomized algo is! 
> A randomized algorithm is an algo which, on the same input, will...
> 1. give sometimes different answers
> 1. have sometimes a different runtime
> 1. Both
> 1. Neither?

Fine. They figured it out, just in the nick of time! But that's only the beginning... now, they have to figure out the answers to the students, who seem to have a pretty bad gambling addiction? They're asking what a _Las Vegas algorithm_ 🎰 is...
> A Las Vegas algorithm is...
> 1. ... always correct, but with a random runtime
> 1. ... sometimes wrong, but with a fixed runtime
> 1. ... something that happens and stays in Vegas 🎰
> 1. ... sometimes correct, and with random runtime

Now, Monte Carlo algorithms? They got it. These are easy: "you look at what a Las Vegas algo is, and it's the opposite." 🧐

Carole and Dave are relieved. The students, not so much. So they keep asking questions!

(A Monte Carlo algorithm has a fixed runtime, but can err with small probability.)


Well, the class is almost over, Carole and Dave are nearly off the hook... Just a few more minutes, nothing wrong can happen now...

"Can we always convert a Las Vegas algorithm into a Monte Carlo one, and vice-versa?"

Uhoh. 🥒
> 1. ... Las Vegas to Monte Carlo, yes! 🎲
> 1. ... Monte Carlo to Las Vegas, yes! 🪙
> 1. ... Both! 🎰
> 1. ... Neither! 💸


## What are the answers?

The first question was _very_ tricky, and Carole and Dave were not the only ones to be fooled... at the time I write this, [only 13% of you got it right]([https://bsky.app/profile/ccanonne.bsky.social/post/3lidokfl6tq2n](https://bsky.app/profile/ccanonne.bsky.social/post/3liw6ibt26s2v))! A randomized algorithm could very well be consistent, and always give the same answer: an example you might have seen is Randomized QuickSort, which sorts an array (by randomly choosing a pivot for QuickSort). It's always going to return the sorted array, and... there aren't that many distinct options for what that is!

A randomized algorithm also could also always have the same runtime on a given input (or even on *all* inputs). The [Fisher–Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle), an algorithm to generate a uniformly random permutation, is a non-trivial example you might have seen: it always takes the same number of steps.

And some algorithms will have both properties: here's a slightly artifical (but hopefully not too much) example: say you have data structure duplicated (for redundancy and resilience) over many hard drives (N of them), and want to search for a given record in that data structure. To distribute the load, you may just pick one of the N hard drives uniformly at random, then use the copy of the data structure on that drive: always the same runtime, always the same output, just a different memory path!

So the answer is **Neither**. What is a randomized algorithm, then?! At the risk of being tautological, it is... an algorithm using randomness. One way to model it is, instead of an algorithm A only taking an input x, denoted A(x), to have an algorithm A taking as input x as well as a source of uniformly random bits r∈{0,1}*, denoted A(x;r). If you fix r, then the algorithm A(.;r) is just a deterministic algorithm: all the randomness, however it manifests, comes from the auxiliary input r. 

Equivalently, a randomized algorithm is a *probability distribution over deterministic algorithms*: you have a (possibly infinite) family of deterministic algorithms {Aʳ: r in {0,1}*}, and the randomised algorithm can be viewed as follows: on a given input x,
- Pick the random seed R at random;
- Run the algorithm Aᴿ on x
