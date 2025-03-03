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

Carol and Dave are relieved. The students, not so much. So they keep asking questions!

(A Monte Carlo algorithm has a fixed runtime, but can err with small probability.)


Well, the class is almost over, Carol and Dave are nearly off the hook... Just a few more minutes, nothing wrong can happen now...

"Can we always convert a Las Vegas algorithm into a Monte Carlo one, and vice-versa?"

Uhoh. 🥒
> 1. ... Las Vegas to Monte Carlo, yes! 🎲
> 1. ... Monte Carlo to Las Vegas, yes! 🪙
> 1. ... Both! 🎰
> 1. ... Neither! 💸


## What are the answers?

The **first question** was _very_ tricky, and Carol and Dave were not the only ones to be fooled... at the time I write this, [only 13% of you got it right](https://bsky.app/profile/ccanonne.bsky.social/post/3liw6ibt26s2v)! A randomized algorithm could very well be consistent, and always give the same answer: an example you might have seen is Randomized QuickSort, which sorts an array (by randomly choosing a pivot for QuickSort). It's always going to return the sorted array, and... there aren't that many distinct options for what that is!

A randomized algorithm also could also always have the same runtime on a given input (or even on *all* inputs). The [Fisher–Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle), an algorithm to generate a uniformly random permutation, is a non-trivial example you might have seen: it always takes the same number of steps.

And some algorithms will have both properties: here's a slightly artifical (but hopefully not too much) example: say you have data structure duplicated (for redundancy and resilience) over many hard drives (N of them), and want to search for a given record in that data structure. To distribute the load, you may just pick one of the N hard drives uniformly at random, then use the copy of the data structure on that drive: always the same runtime, always the same output, just a different memory path!

So the answer is **Neither**. What is a randomized algorithm, then?! At the risk of being tautological, it is... an algorithm using randomness. One way to model it is, instead of an algorithm A only taking an input x, denoted A(x), to have an algorithm A taking as input x as well as a source of 🎲 uniformly random bits r∈{0,1}*, denoted A(x;r). Once you fix r, then the algorithm A(.;r) is just a deterministic algorithm: all the randomness 🎲, however it manifests, comes from that auxiliary input r. 

Equivalently, a randomized algorithm is a *probability distribution over deterministic algorithms*: you have a (possibly infinite) family of deterministic algorithms {Aʳ: r in {0,1}*}, and the randomised algorithm can be viewed as follows: on a given input x,
- Pick the random seed R at random; 🎲
- Run the algorithm Aᴿ on x

This "distribution over deterministic algorithms" is quite useful, incidentally, especially when you start looking at specific models of computations (e.g., randomized decision trees), or at things like [Yao's Principle](https://en.wikipedia.org/wiki/Yao%27s_principle).

The **second question**... [63.6% of you answered it correctly](https://bsky.app/profile/ccanonne.bsky.social/post/3liw7343dso2n)! A 🎰 Las Vegas algorithm is _always correct_, but with a random runtime. Specifically, it has a *finite* expected runtime, but that could be arbitrarily large: that being said, it never makes a mistake. (It might just be very slow, sometimes.) An example? Randomized QuickSort mentioned above! It _always_ sorts the input array correctly. In the _worst_ case, if really you're incredibly unlucky with the choice of the random bits it's using, it may take time O(n²): but its *expected* runtime, on every input array, is O(n log n).

If you're interested in Randomized QuickSort and its analysis, by the way, see refs at the end!

_Small remark:_ for decision problems (the answer is "yes" or "no"), Las Vegas algorithms with polynomial expected runtime are known by the sweet and beautiful name [ZPP](https://en.wikipedia.org/wiki/ZPP_(complexity)) (Zero-error Probabilistic Polynomial time). Oh, complexity theorists, you do know how to pick acronyms! 🎩

The **third and last question**... [50.0% of you answered it correctly 🎲](https://bsky.app/profile/ccanonne.bsky.social/post/3liw7obikpp2p): a coin flip 🪙, ironically! Monte Carlo algorithms are the counterpart of Las Vegas algorithms: fixed worst-case running time, but only correct with high probability (also small remark: for decision problems, polynomial-time Monte Carlo algorithms form the complexity class class [BPP](https://en.wikipedia.org/wiki/BPP_(complexity)), which stands for 🎲 "bounded-error probabilistic polynomial time." Yes, we _truly_ suck at names.)

One can **always** convert a 🎰 Las Vegas algorithm with _expected_ running time T into a Monte Carlo algorithm with worst-case running time O(T), and success probability 99%. The basic idea is simple, and boils down to our friend [Andrey Markov and its eponymous inequality](https://en.wikipedia.org/wiki/Markov%27s_inequality): if you have a Las Vegas algorithm A with expected running time T: run it for 100T time steps. If it ended by then, you're done 🎉, output the result (you know it's correct). If it hasn't ended, you're out of luck: output anything you want, say "🥔". The probability of a 🥔 is the probability the running time is 100 times its expectation, which has 🥔 probability at most 1/100 by Markov's inequality.

The *converse is not (known to be) true, however* (for people following the nice acronyms, for decision problems we have ZPP ⊆ BPP, but whether that's an equality is very much unknown 🤷). We can only show it under some additional assumptions: for instance, _"given a solution, we can very quickly verify it is correct."_ In that case, here's an approach: if you have a Monte Carlo algorithm A with worst-case running time T, and verifying a solution takes time V. On a given input x:
- Run A until you get a solution y: time T
- Check if the solution is correct: time V
- If so, success 🎉; if not, go back to the first step and repeat
If A is correct with probability 9/10, then one can show that the expected number of repetitions until with get a correct solution is O(1): it's the sum of a geometric series. So the overall running time is O(T+V), which is great if, say, V=O(T), or even O(1)! 🍰

Unfortunately, V is not necessarily small, so that doesn't give us a general-purpose conversion in this direction... Life, not unlike coconuts, can be hard. 🥥

## Where to look for more?
There are many great books 📚 to learn more about randomised algorithms, for instance the classic _Randomized Algorithms_ by Raghavan and Motwani. A must-read! Now, what is covered above can be found in the **first two lectures of my course** [Randomised and Advanced Algorithms](https://ccanonne.github.io/teaching/COMPx270), also with a beautiful name: "COMP4270." All materials are freely available on that page: for example, Randomised Quicksort is in 📝 [p.4 of the first set](https://ccanonne.github.io/files/compx270-chap1.pdf).
