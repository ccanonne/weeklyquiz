# First Quiz: Alice and Bob compute EQUALITY (or go to jail)

_See the [Quiz on BlueSky](https://bsky.app/profile/ccanonne.bsky.social/post/3lidolztdy22n)._

## Backstory

 🧩 📊 Alice and Bob are in trouble. They've been accused (wrongly, of course!) of stealing classified data, and as theorists they now have to prove they innocence and defend their honour: they've never used any data!

Now their lawyers will give them each a statement, including a list of things they must swear to have done or not. So many things—$`n`$ of them—to state or deny! To be found innocent, they'd better agree on all of these $`n`$ items..

That's the trick though. They won't get these lists before the trial!

Worse, they'll have to testify separately, without knowing what the other one says. Their only chance to coordinate? On the way to the trial, in the corridor, they might be able to briefly wink or wave at each other before being ushered into the courtroom.

Alice and Bob are a _little_ stressed.

Thankfully they have an extra resource: the courtroom has one of these big lava lamps in the hall, which they'll both see, and can use to get some shared random number, should they see a need for it. Better than nothing?

They can only communicate 1 bit of information after getting their lists... so **help them** make it count!

1. Alice gets her list of $$n$$ yes/no 📋
 Bob gets his list of $$n$$ yes/no 📋
2. They go to the courtroom, both see the same lava lamp 💡🎲
3. They get to wave or not in the corridor, sharing one bit of information 👋
4. Alice and Bob both testify then plead guilty/innocent

If they plead innocent and their lists are identical (the $$n$$ yes/no statements), they walk free 🎉
If they plead innocent and their lists are different, they go to jail for a long time 🧑‍⚖️
If they plead guilty or disagree, they go to jail (but a much shorter time)

So they'd better be confident!

This is where YOU can help them. What do you think they should say to the judge? Or, to make it simpler: _what should they do to figure out if the lists their lawyers gave them are the same or not?_

> What is the best probability Alice and Bob with which can be sure their $$n$$ yes/no statements 📋📋 are all the same?

Now, the above asks you what you think their best probability of success is. The second question is open ended, and will require you to put your lava lamp where your mouth is:

>  **How** you would suggest Alice and Bob proceed?

## What's the question, again?

Let's try to extract the gist of the question: 
+ Alice and Bob are given, respectively, arbitrary $$n$$-bit strings $$a,b \in \\{0,1\\}^n$$ (the lists 📋 given to them by their lawyers), where $$n$$ is a very large integer; and must jointly decide whether $$a=b$$ with as high a probability of success as possible (over whatever randomness they use to make that decision).
+ To do so, they can only communicate __one__ bit of information (through the waving 👋) to each other. That's not a lot, given that both $$a$$ and $$b$$ require $$n$$ bits to fully communicate!
+ But have an extra resource: they have _shared randomness_, which you can model as a string of uniformly random bits $$r\in\\{0,1\\}^\ast$$ (thanks to the lava lamp which they both observe), which is independent of $$a,b$$.

This question, framed this way, is known as the $$\text{EQ}_n$$ problem in communication complexity:
> How much communication between Alice and Bob is required to decide whether $$a$$ equal to $$b$$?

and more specifically, here, the _randomized communication complexity_ of the Equality function, $$R^{pub}(\text{EQ}_n)$$, in the public-coin 🎲 setting. _("Public-coin" refers to the fact that there is shared randomness: there is a magic fair coin being tossed publicly infinitely many times, that both Alice and Bob observe.)_

That's good news for Alice and Bob, since a lot of **very** clever people have done a lot of work on communication complexity over the past decades. **There's hope.** 🎉

## What's the answer?
As [≈21% of you answered at the time I write this](https://bsky.app/profile/ccanonne.bsky.social/post/3lidokfl6tq2n), by communicating only _one_ bit 👋 __Alice and Bob can succeed with probability at least 50%__. I don't know how to emphasize how _absolutely bonkers_ that is 🤯: they both have an arbitrary $$n$$-bit string, and yet _1 measly bit_ is enough to decide whether they're equal or different!

Thanks to the lava lamp, that is...

## How do we do that?
Let's first consider what happens without randomness 🎲 at all: no lava lamp, first, and no randomness for Alice and Bob either. If they must act deterministically and succeed no matter what $$a,b$$ are, then there's basically nothing they can do unless they communicate $$\Omega(n)$$ bits -- basically their whole input. Put differentially, with only one bit of communciation, Deterministic-Alice and Deterministic-Bob are screwed, and go to jail.

> __Theorem.__ The deterministic communication complexity of $$\text{EQ}_n$$ is $$D(\text{EQ}_n) = \Omega(n)$$.

But the deterministic requirement is a lot to ask. Allowing Alice and Bob to use their own "private" (not shared) randomness, and err with some probability $$\delta < 1/2$$, could help maybe? _(Here we want probability less than 1/2, since the trivial protocol where Alice chooses an answer at random (without even looking at her input $`a`$) and sends it to Bob takes one bit of communication, and has error exactly 1/2...)_

Well, yes. But not __that__ much: one can prove that to achieve probability of error say $$\delta=1/3$$, with only private randomness Alice and Bob still need to communicate quite a lot!

> __Theorem.__ The (private-coin) randomized communication complexity of $`\text{EQ}_n`$ is $`R^{priv}_{1/3}(\text{EQ}_n) = \Theta(\log n)`$.

The upper bound (algorithm) is not too complicated, but we won't give it here (you'll why soon! 🧐). As for the lower bound, it actually follows from the deterministic case, $`D(\text{EQ}_n) = \Omega(n)`$, as in general one can show (it's not trivial!) that $`R^{priv}_{1/3}(f) = \Omega(D(f))`$. That's not exactly what we want (we want to see what's the best Alice and Bob could do with _one_ bit of communication), but somehow this does imply that the best probability of error they could achieve __without public randomness__ vanishes with $$n$$.

Which brings us to the lava lamps! 🎲

> __Theorem__(Alice and Bob Get Out of Jail). The (public-coin) randomized communication complexity of $`\text{EQ}_n`$ is $`R^{pub}_{1/3}(\text{EQ}_n) = O(1)`$. Moreover, given $`k`$ bits of communication, $`\text{EQ}_n`$ can be solved with probability of success $$1-1/2^k$$.

Here is the basic idea:
+ Alice and Bob use the lava lamps to agree on a uniformly random string $$r\in\\{0,1\\}^n$$.
+ They compute respectively the bits $x = \langle a, r\rangle = \sum_{i=1} a_i r_i \mod 2$ and $y = \langle b, r\rangle = \sum_{i=1} b_i r_i \mod 2$
+ By waving 👋 (or not), Alice learns $$y$$, Bob learns $$x$$, and they both say "equal!" iff $$x=y$$
_(One could argue this is two bits of communication, one from Alice to Bob and one from Bob to Alice. Well, this is just so that both know the answer: if we just want Bob to know the answer, then only Alice needs to send $`x`$ to him)_

It is not hard to see that (1) if $$a=b$$, then of course $$x=y$$ (always); and that if $$a\neq b$$, then they differ in at least _one_ bit, say the $$i$$-th and that contribution $$r_i a_i$$ vs. $$b_i r_i$$ to the sum modulo 2 will make $$x \neq y$$ will probability $$1/2$$ over the randomness of $$r$$.

(Given $$k$$ bits of communication instead of $$1$$, repeat the above $$k$$ times independently, and only say "equal" if all $$k$$ runs outputted "equal". That drives the probability of error to 0 exponentially fast.)

This is amazing! And what's better, this also shows the upper bound of the previous theorem ($$R^{priv}_{1/3}(\text{EQ}_n) = O(\log n)$$ immediaely, via a jewel of a result know as Newman's Theorem:
> __Theorem__(Newman's Theorem). For any $f$, $`R^{priv}_{1/3}(f) \leq R^{pub}_{1/3}(f) + O(\log n)`$.
This theorem is **amazing**. Even better, it is conceptually very simple, and can generalize beyond communication complexity: the idea is to show that "well, actually, Alice and Bob can always transform a public-coin protocol (using an arbitrary amount of shared random bits) into one that that just picks the random string $$r$$ uniformly at random _among only $`T=O(n)`$ fixed options_." Which is great, because now **a private-coin protocol can simulate the public-coin one** by making Alice to pick $r$ at random by herself, and tell Bob which of the $T$ options it has picked... which only takes $$\log T = O(\log n)$$ bits of communication!

# Concluding remarks
The public-coin protocol for Equality given above seems quite _ad hoc_: interestingly, one way to interpret it is to scream _"Error-correcting code!"_ 3 times and ask the coding theorist that appears. What the protocol actually does with that random string $$r$$ can be interpreted as passing $$a$$ and $$b$$ through the [Hadamard code](https://en.wikipedia.org/wiki/Hadamard_code) $$C\colon \\{0,1\\}^n \to \\{0,1\\}^{2^n}$$, an error-correcting code with _distance_ 1/2: if $$a\neq b$$, then $$C(a)$$ and $$C(b)$$ differ in exactly half of their $$2^n$$ bits. Then choosing $$r$$ uniformly at random corresponds to picking one of these $$2^n$$ bits uniformly at random -- the same for Alice and Bob -- and checking whether $$C(a)_r=C(b)_r$$. So if $$a\neq b$$, then they'll find a coordinate on which $$C(a),C(b)$$ differ with propbability half!

Seen this way, there is nothing specific about the Hadamard code: any error-correcting code $$C'\colon \\{0,1\\}^n \to \\{0,1\\}^{k}$$ with constant distance will do. And having $$k$$ smaller than $$2^n$$ (in particular, some good error-correcting code exist with $$k=O(n)$$) means needing fewer random bits: $$\log k = O(\log n)$$ suffice! Which matches what Newman's Theorem guarantees exists....
