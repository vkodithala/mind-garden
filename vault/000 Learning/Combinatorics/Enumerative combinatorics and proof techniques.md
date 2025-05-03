#### Fundamentals of set theory
- **Def.** A set S is a collection of elements
	- For instance, the set {1, 2, 3, 4, ...} is the set of all natural numbers -- sets can be infinitely long, as the set of all natural numbers is
	- By definition, sets don't contain repeat elements
	- Sets aren't limited to containing numbers, and can actually contain anything
		- Set builder notation gives us a way to encode this: $A = \{\lambda | \lambda \in alphabet\}$
- **Def.** The cardinality of a set == the number of elements in the set
	- For instance, card(empty set) = 0 because the empty set contains no elements -- this is also something of note
- **Def.** A relation (colloquially) is a description of how two objects can be related. A basic example is the comparators </=/>
	- Mathematically, a relation $R$ from set $X$ to set $Y$ is a set of ordered pairs $r = (x, y)$ such that $x \in X$ and $y \in Y$
		- This is written as $R = \{(x_1, y_1), (x_2, y_2), \ldots\}$
	- Properties of relations:
		- **Non-empty** if it contains $\geq$ 1 pair of elements $(x, y)$
		- **Reflexive** if for every $x \in X$, $(x, x)$ is an element of the relation -- an example of this is the relation $\geq$ because elements are equal to themselves
		- **Symmetric** if for every pair $(x, y)$, $(y, x)$ also appears in the set
			- **Anti-symmetric** if $\leq$ 1 of these pairs appears in the set -- they cannot both be true
		- **Transitive** if pairs $(x, y), (y, z)$ both exist in the set, then $(x, z)$ also appears by consequence (example is == set)
- **Def.** The Cartesian product of two sets $X, Y$ is the largest possible relation between them
	- In other words, this is the set of all possible ordered pairs $(x, y)$ that could possibly make up a relation between the sets
	- Useful property: if two sets $X, Y$ are finite, the cardinality of their Cartesian product is simply $|X| * |Y|$ -- we can use this to check that our Cartesian products were computed correctly
- **Def.** A set $T$ is a subset of $S$ if every element in $T$ also exists in the set $S$
	- $T$ being a subset is usually indicated by "loose" containment, or $T \subseteq S$, which means that it's a subset but also might be equal to the larger set
	- We also have a notation for "strict" containment, which means that the subset is necessarily $\neq$ to the larger set
		- This is represented by $T \subsetneq S$
- **Def.** A function $f: X \to Y$ is a special relation such that every element $x \in X$ is mapped to exactly one ordered pair $(x, y)$
	- Fully understanding this (and the injective/surjective nature of functions) requires 3 other definitions:
		- The domain of a function is the first set $X$ (meaning that every element in the *domain* is mapped to exactly one ordered pair, in a more combinatorial definition)
		- The codomain of a function is the second set $Y$, and its range is a subset of the codomain which consists of all of the mapped elements $\in$ the range
	- Then, we must understand injectivity/surjectivity
		- A function is injective if every element $y \in Y$ appears in at MOST one ordered pairing (there are no doubly mapped elements in the range!)
		- A function is surjective if the range = the codomain, or every $y \in Y$ exists in at LEAST one ordered pair
		- A function is a bijective if both are true. This means that every $y \in Y$ is mapped to exactly one element $x \in X$, and can only be true if |domain| == |codomain|
> Let $A = \{Fish, Bird, Bee\}, S = \{California, Georgia\}$. Give an example of a function $f: A \to S$. Then give, a relation $R$ from $A$ to $S$ where $|R| = 4$. Finally, explain (in a sentence or two)
- Since a function must map every element of the domain to exactly one ordered pair, here's one: $f = \{(Fish, California), (Bird, California), (Bee, California)\}$
- A relation between the sets that has cardinality 4 is some set of ordered pairs that relates them. It must include 4 such ordered pairs. So an example is: $R = \{(Fish, California), (Bird, California), (Bee, California), (Bee, Georgia)\}$
- Why can't a relation of cardinality 4 also be a function here? Answer is simple: in its definition, we say that a function maps every element in the domain to exactly 1 ordered pair
	- There are 3 elements in the domain, so that makes 3 ordered pairs
	- Thus the maximum (and only) cardinality that a function can have is 3. 4 would mean that some element of the domain appears in > 1 pair
	- Can prove this with the Pigeonhole principle
		- Akin to the problem of fitting $k + 1$ pigeons in $k$ holes. Here, we have 3 elements of the domain (our holes) to which we must assign 4 ordered pairs (our pigeons). At least one "hole" (element of the domain) will be repeated
> Let $Z$ denote the set of all integers, and define the function $f: Z \to Z$ by $f(n) = 4n$. Is $f$ a 1:1 function? Is $f$ onto? Justify both of your answers. The second should be justified with the range of $f$.
- 1:1 functions are such that every element in the range is mapped to $\leq$ 1 ordered pair. Is this true for $f(n) = 4n$?
	- Yes. The matching element $x$ for an element $y$ is found by $\frac{y}{4}$, which yields a unique result for every value of $y$, or none at all. Thus this is true.
- Is the function also onto? No. This is because some elements $y \in Y$ are integers, but not multiples of 4
	- Let's take 5, for example. $\frac{5}{4}$ doesn't fit nicely into an integer, and thus there's no element in $X$ that maps to it
	- So, what's the function's range? Can be written in SBN: $\{4x | x \in X\}$. This isn't equivalent to the range of all $y \in Y$, which is further proof that the function isn't onto
> Let $A = \{a, b, c, d, e\}, B = \{a, e\}$. How many sets $C$ have the property that $C \subseteq A$ and $B \subseteq C$?
- This means that $C$ must contain both elements from B, so it has by default $a, e$. Then it must also contain some number of elements from the subset $\{b, c, d\}$. There are $\binom{3}{1} + \binom{3}{2} + \binom{3}{3}$ such combinations, which yields $\frac{3!}{2!} + \frac{3!}{2!} + 1$ = 7 options. Including the set $\{a, e\}$, there are 8 sets with this property
> **E1 q1a.** Let X = {4, 5, 6, 7} and Y = {D, E, F}. Which of the following functions is it possible to define? (multiple-select)
- We *can* define a surjection that's simply not injective. This would require a double-mapping of one of the elements of the domain
- We can also define an injective function that's not surjective, but injective from $Y \to X$. This can be done simply by mapping every element of Y to some distinct element in the domain
- Of course, we can have a mapping $X \to Y$ that's neither injective nor surjective by simply mapping mapping every element of the domain to every element of the codomain
- So the answer is simply "all of the above"
> **E1 q1b.** Give a function $f: X \to Y$ that is neither one-to-one (injective) or onto (surjective). Justify why the relation you've given is a function, and why it has these properties.
- We give the function f: {(4, D), (5, D), (6, D), (7, D)}
	- A function only has to map each element of the domain to exactly one ordered pair (x, y) where y is an element of the codomain, which this does
	- It's not one-to-one/injective because D is mapped > 1 time
	- It's not onto/surjective because E and F both exist in none of the ordered pairs
#### The Pigeonhole principle, and more on bijections/proof writing
-  **Def.** The Pigeonhole principle states broadly that if we have $M$ items to be placed in $N$ bins, with $M > N$, then some bin will contain > 1 item
	- We can use this to prove that a injective function exists between 2 sets iff $|X| \leq |Y|$
		- First, let's prove the opposite, using the Pigeonhole principle. If $|X| > |Y|$, then in creating a function, at least one element $y \in Y$ is mapped to > 1 $x \in X$
		- And if $|X| \leq |Y|$, then we can simply map every $x$ to a unique element from the codomain and this makes a surjection
- By a series of proofs that use the Pigeonhole principle, we show that there exists a bijection between two sets iff $|X| == |Y|$
	- This is useful to us because we can now prove that two sets are equal in size/cardinality just by proving that a bijection exists between them (iff is reversible)
	- But how do we even prove that a function is a bijection? Two ways:
		- (1) Prove both properties of a bijection independently. First, that it's injective -- maps every element of codomain $\leq$ 1 time. And then that it's injective -- maps every element of codomain $\geq$ 1 time
		- (2) Just show that $f$ has an inverse function $g$ such that $f(g(y)) = y$ for all $y \in Y$. This inverse function maps all elements of codomain to their corresponding elements of the domain
> **E1 q2.** Suppose a subset $S \subseteq \{1, 2, \ldots, 39, 40\}$ contains exactly 21 distinct integers between 1 and 40. Explain why $S$ must contain at least one pair of consecutive integers, i.e., there exist $x, y \in S$ with $|y-x| = 1$.
- The number of pairs of consecutive integers in range $1 \ldots 40$ is equal to $\lfloor \frac{40}{2} \rfloor$, because the pairs themselves are {(1, 2), (3, 4), (5, 6), (7, 8), ...}. This number is equivalent to 20
- Thus the total number of non-consecutive integers that can be picked is $40 - 20 = 20$
- We can now translate this to a Pigeonhole problem with 20 holes and 21 pigeons 
	- We only have 20 non-consecutive integers and want to pick 21 *distinct* ones, which is impossible to do while maintaining the property of distinction
#### The principle of mathematical induction
- **Def.** The Catalan numbers are a sequence of numbers such that $C_n = \frac{(2n)!}{(n+1)!n!}$
- **Def.** Let $S_n$ denote a statement that depends on the number $n$ and can carry a concrete truth value
	- Then, we can define induction in 1 of 2 ways
	- Weak induction requires a proof of:
		- Some base case $S_1$
		- Proof that $S_k$ being true implies that $S_{k+1}$ is true also
	- If both are proven, then we say that $S_1$ implies that $S_k$ is true $\forall k > 1$
	- Weak induction works for most problems, but sometimes we need strong induction
		- This type of induction indicates $n_0$ as the smallest value of $n$ that we "care about"
		- It then defines some series of base cases, like $S_{n_0}, S_{n_0 + 1}$, etc., and proves them
		- Lastly, it proves that those base cases being true (up to $S_k$) implies that $S_{k+1}$ is true also
	- To end this section, we'll provide an example where weak induction doesn't suffice, and we actually need to employ strong induction
> Show that for any amount of postage $p$ cents that's $\geq$ 12 cents can be paid using only 4 and 5-cent stamps exactly
- We define the smallest value that we care about to be 12, and go on to prove that $S_{12}, S_{13}, S_{14}, S_{15}$ are all true (this is trivial)
- After we've proven this, we must prove that for $k > 15$, $S_k$ is true also
	- At this point, we know that $S_{k-3}$ is true, so we just take whatever distribution was used here and add 4 cents to it to make our new value
- Note that this proof couldn't have been completed with weak induction
	- We needed a "history" of > 1 value in the past in order to make our case
> Use induction to prove that $\sum_{i=1}^{n}(i)(i!) = (n+1)! - 1$ for $n$ that's some positive integer.
- Let's start by proving this for the base case, and then we can make a more intelligent decision about whether we want to use strong or weak induction
- For $n = 1$, the left-hand side equates to 1, and the right-hand side equates to $(2)! - 1 = 1$. Thus $S_1$ is true
- Now let us (try to, because we might have to use the strong form of induction) prove $S_{k+1}$ given that $S_k$ is true. $\sum_{i=1}^{k+1}(i)(i)!$ = $\sum_{i=1}^{k}(i)(i)! + (k+1)(k+1)!$ = $(k+1)! - 1 + (k+1)(k+1)!$
	- And also for $k+1$, the value on the right-hand side equates to $(k+2)! - 1$ 
	- We know that $(k+2)! = (k+1)!(k+2)$, right? 
	- So the value on the right-hand side for an input $k+1$ is equivalent to $(k+1)!(k+2) - 1$
	- The left hand side can be rewritten as $1(k+1)! - 1 + (k+1)(k+1)! = (k+2)(k+1)! - 1$, which makes both sides equivalent
- Thus we've also proven the inductive step, and the equality of both expressions is true for any positive integer by weak induction.
#### Enumeration
- **Def.** The sum rule states that if an event $A$ can happen $n$ different ways, and an event $B$ can happen $m$ different ways, the number of ways that the event "A or B" can happen is $n + m$
- **Def.** Let's say we have the same scenario as above, except the problem statement gives that for some choice of $A$, there are exactly $m$ ways to choose $B$ and we need to find the number of ways that $A$ AND $B$ can both happen
	- By the product rule, this is $n * m$
- **Def.** An $X-string$ of length $n$ is an ordered sequence of $n$ elements from the set $X$
	- Given $X, n$, the number of different $X-strings$ is equivalent to the permutation $P(X, n) = \frac{|X|!}{(|X|-n)!}$, assuming that there's no repetition
- **Def.** A permutation is an arrangement of some collection of objects
	- For $n$ distinct objects, the number of permutations is equal to $n!$
	- The number of length-$k$ permutations of $n$ distinct objects is equal to $P(n, k) = \frac{n!}{(n-k)!}$
- **Def.** A combination $\binom{n}{k}$ denotes the number of ways that we can pick $k$ elements from a set of $n$, irrespective of the order in which they're picked
	- Mathematically, $\binom{n}{k} = \frac{n!}{k!(n-k)!}$
	- In situations where we care about this order, we'd use permutations
	- This is particularly useful for "picking" scenarios, not "ordering" or "arrangement" ones
> Bea is deciding what to wear on the first day of her internship. It's supposed to rain, so she will wear a sweater over either a dress or a shirt and slacks. If she has 10 sweaters, 5 shirts, 3 pairs of slacks, and 4 dresses, how many possible outfits can she choose from?
- Her first choice is to wear some sweater, of which there are $\binom{10}{1} = 10$ possibilities
	- If she choose to wear a dress under the sweater, she's picking from $\binom{4}{1} = 4$ possibilities. Thus there are $10 * 4 = 40$ ways of wearing a sweater over a dress
	- If she instead chooses to wear a shirt and slacks, we use the product rule to find that there are $\binom{5}{1} * \binom{3}{1} = 15$ ways to choose a shirt-slack combination. Thus there are $10 * 15 = 150$ of wearing a sweater over a shirt and slacks
	- Lastly we use the sum rule to combine all of this information. There are 190 ways of Bea dressing
> A student is given a 10 question exam and needs to choose exactly 7 of the 10 questions to answer. How many ways can they do this if: (a) there are no restrictions on which 7 questions, (b) they must answer the first 2 questions, and (c) they must answer $\geq$ 4 of the first 6 questions
- (a) If there are no restrictions, they simply need to choose 7 questions out of 10, and this becomes an application of combinations. The number of ways that they can do this is $\binom{10}{7} = \frac{10!}{7!3!} = 120$
- (b) They must answer the first 2 questions, which they can do in only 1 way. This leaves 5 more questions, which they must pick from the remaining 8. There are $\binom{8}{5}$ ways for them to do this
- (c) We must distribute this into cases, which is a little unintuitive at first because our mind leads us to a different answer
	- The first case is that we choose 4/6 of the first 6 questions, and then must choose 3 of the remaining 4: $\binom{6}{4} * \binom{4}{3}$ 
	- The second is that we choose 5/6 of the first 6, and then 2 of the remaining 4: $\binom{6}{5} * \binom{4}{2}$
	- And the last case is that we choose all of the first 6, and 1 of the remaining 4: $\binom{4}{1}$
	- We use the sum rule to sum up all of these cases (as we do for any case-based problem), and we get an answer of: $\binom{6}{4} * \binom{4}{3} + \binom{6}{5} * \binom{4}{2} + \binom{4}{1}$
> The country of Lilliput is considering instituting a new license plate system, where each vehicle will be assigned a tag consisting of two different letters (from the English alphabet), followed by three distinct integers (chosen from 0-9, inclusive. If there are 800,000 vehicles in Lilliput,
- (a) Will the proposed system be sufficient to identify them all?
	- At its core, this question is asking how many different license plates can be generated given the constraints
	- The tag sequence begins with 2 distinct letters, of which there are $P(26, 2) = \frac{26!}{24!}$ possibilities, and this is followed by 3 distinct integers $P(10, 3)$ possibilities). Thus the total number of distinct tags that can be made is $P(26, 2) * P(10, 3) = 650 * 720 = 468,000$. This is lower than the number of vehicles in 800,000, so the proposed system would NOT work
- (b) Gulliver suggests modifying the system to allow two letters and three numbers, all symbols distinct but allowed to be in any order. Will the modified system have enough possibilities to license all the vehicles?
	- Again we are solving for the size of the solution space, but we must use a mix of permutations and combinations here
	- Picking 2 letters and 3 numbers yields $\binom{26}{2} * \binom{10}{3} = 325 * 120 = 39,000$ possibilities
		- But ordering them is a different problem altogether, and we have 5! ways to do this
	- Thus the total number of distinct tag sequences is simply $39,000 * 5! = 4,680,000$, and this is more than sufficient for the population of Lilliput
> **E1 q4a.** Let X = {A, B, C, D, E, F, G} (7 letters in all). Find the number of X-strings of length 5 that contain *exactly* 2 A's.
- We have exactly 2 A's, so we need to pick the remaining 3 letters out of the 6 that we have. We can do this in $6^3$ different ways. **This takes care of order**
- We then need to order the 5 letters in the string that we've assembled. We've already taken care of order for the non-A's, but need to pick 2 slots out of the 5 that we have in between the non-A's to place the A's
	- This can be done in $\binom{5}{2}$ different ways
- Thus the number of length-5 X-strings that we can make is $6^3\binom{5}{2}$
> **E1 q4b.** Find the number of X-strings of length 5 that contain *at least* two A's. You do not need to simplify your answer.
- We can distribute this into four discrete cases, and then use the sum rule afterwards:
	- **Case I.** 2 A's. This devolves to what we proved in part a., and can be done in $\binom{5}{2}6^3$ different ways
	- **Case II.** 3 A's. We choose the other 2 letters (and their ordering) in $6^2$ ways, and have 3 places for the non-A's, so $\binom{3}{2}$ choices. Thus there are $\binom{3}{2}(6^2)$ ways of doing this
	- **Case III.** 4 A's. We choose the other letter in $6^1$ ways, and it can be placed in one of 5 different places. Thus there are $6 * 5 = 30$ ways
	- **Case IV.** 5 A's. There are no choices, ordering or otherwise, to be made here. And thus there's only one way to create this scenario
- Summing these up the number of length-5 X-strings with $\geq$ 2 A's is $\binom{5}{2}6^3 + \binom{3}{2}(6^2) + 30 + 1$
- *We can alternatively do this by counting the number of ways to produce length 5 X-strings with no restrictions, and then subtracting the X-string with 0 and 1 A's. This is easier than our current approach!*
> **E1 q5.** Darren wants to order a sandwich and has the following choices:
> 
> Breads: white, wheat, or rye
> Proteins: ham, turkey, tofu, or tempeh
> Cheeses: cheddar, swiss, or pepper jack
> 
> If he must choose a type of bread and either two proteins (which may/may not be distinct) or a protein and some cheese, how many different sandwiches could Darren order?
- He can choose a type of bread in $\binom{3}{1} = 3$ ways
	- Case I. He chooses two proteins. We can count this using Stars and Bars, with 2 "stars" and 3 "bars". Results in $\binom{5}{3}$ ways
	- Case II. He chooses a protein and a cheese. This can be done in $4 * 3 = 12$ ways
	- Thus his choices after choosing bread amount to $\binom{5}{3} + 12$
- Total number of sandwiches Darren can make is thus $3(\binom{5}{3} + 12)$. This equates to $22(3) = 66$
> **E1 q6a.** How many permutations are there of the letters in the word 'MAMMALIAN'?
- This word has 3 M's, 3 A's, 1 L, 1 I, and 1 N. The number of permutations of it is given by the multinomial coefficient $$\binom{9}{3, 3, 1, 1, 1}$$
> **E1 q6b.** How many permutations of the letters in the word from above have the property that no two A's are consecutive (i.e., "AA" does not appear)?
- Let's first pick the order of the non-A letters, which there are 6 of. This can be done in $\frac{6!}{3!}$ ways
	- Then, we have 7 spots in which to place the 3 A's such that they none of them appear together. We choose 3, a choice of which there are $\binom{7}{3}$ ways
- Thus the total number of fitting permutations of the word is $\binom{7}{3}\frac{6!}{3!}$
#### Variations of permutations
- **Thm.** If there are $N$ total objects of $k$ different types, with $n_1$ objects of type 1, $n_2$ objects of type 2, up to $n_k$ objects of type k, where objects of the same type are indistinguishable, the number of orderings of those objects is $\frac{n!}{n_1! * n_2! * ... * n_k!}$
	- This seems quite straightforward, but applications can get tricky -- consider for instance the problem of counting the number of 6-letter permutations of the letters "ATLANTA"
	- The word itself is a 7-letter word, so we must exclude exactly one letter: we can exclude either an A, T, L, or N, and must consider all of these cases and adjust our problem accordingly *before* applying this theorem (see work below)![[Screenshot 2025-04-27 at 2.36.28 PM.png]]
- **Thm.** The number of circular permutations of n distinct objects is $(n-1)!$
	- To understand this, think about what it means to arrange items in a circle. If I arrange the letters (A, B, C) in a circle, their *ordering* remains the same even if they differ through rotation
		- In this example, (A, B, C) = (B, C, A) = (C, A, B) through circular rotation, and so we must divide each ordering by 3, resulting in $\frac{3!}{3} = 2 = (3-1)!$ , which is a proof of the theorem that we introduced prior
> But now for a more difficult example. Let's say that we're arranging a plate of snacks. We have 4 types of crackers and 4 types of fruits that we want to arrange in a circle such that we alternate between crackers and fruits in the circle. How many different arrangements are possible?
- **Method 1: Pretend it's linear, and then adjust for overcounting**
	- In the linear case, there would be 4!4! ways to arrange the crackers and fruits such that they alternate
	- We know that we always start with a cracker, and that a rotation would mean changing which cracker is placed at the head of the list/circle
	- There are 4 crackers, so there are 4 such rotations -- thus our result is $\frac{4!4!}{4} = 4!3!$
- **Method 2: Worry about the crackers first!**
	- Since we know that fruit must be placed in between the crackers, let's worry about how to place the crackers first. There are 4 of them, which must be placed in a circular order -- by the theorem introduced earlier, there are $3!$ ways of arranging them
	- Now we must place fruit between them. This is akin to a linear counting problem -- all we have to worry about is the order of the fruit, as their rotations are symmetrical with those of the crackers
	- Thus the number of of possible arrangements is $4!3!$, which matches our result from Method 1
> Determine the number of permutations of the letters in the word SENSELESSNESS. You don't need to simplify your answer.
- This is a straightforward application of the multinomial theorem -- there are 6 S's, 4 E's, 2 N's, and 1 L in the 13-letter word
	- Thus by the multinomial theorem, the total number of permutations equals $\frac{13!}{6!4!2!1!}$
> A group of 8 friends is sitting down at a circular table to eat dinner on Janet's birthday. If Cady and Regina do not want to sit next to each other and the seats at the table are indistinguishable, how many ways can they sit down?
- We can solve this by first counting the number of ways that Cady/Regina CAN sit together, and subtract this from the total number of circular permutations of the 8 friends
	- By the theorem that was introduced in class, the total number of circular permutations of the eight friends is $7!$
	- To find the number of permutations where Cady/Regina are seated together, let's consider them as a block (b/c they're seated together) such that we simply have to place 7 items circularly. This yields $6!$ distinct orderings
		- But we have to multiply this again by 2, because Cady and Regina can be placed on either side of one another
- Thus the number of orderings where Cady and Regina aren't seated together is simply $7!- 2 * 6! = 4,320$
>Professor Carney's 10 new lab assistants need to pass a lab safety test before they're allowed to handle volatile chemicals. 
>
>There are 5 indistinguishable stations where the test can be completed - he'll have them complete the test in two groups. If each possibility is different when either of the two circular arrangements has changed, determine the number of possible configurations in which the assistants could take the test
- There are $\binom{10}{5}$ to divide the students into two groups
- By the theorem introduced in class, each group has $4!$ possible circular arrangements. Thus the number of possible arrangements between the two groups is given by the product rule: $4! * 4!$
- Therefore the number of possible configurations in which the assistants could take the test is given by $\binom{10}{5} * 4!^2$
> How many ways are there to arrange the numbers 2, 3, 4, 5, 6, and 7 in a circle such that no two adjacent numbers add to 13?
- The only two numbers here that'd add up to 13 are 6 and 7. So we simply need to ensure that these numbers aren't placed next to one another
	- The total number of circular permutations irrespective of this property is equivalent to $(6-1)! = 5!$
	- The number of circular permutations where 6 and 7 are placed next to each other is $2(5-1)! = 2(4!)$. The 2 is placed in front because we can place them in two orderings, which differ based on which number appears first
- Thus the total number of arrangements is $5! - 2(4!)$
> **E1 q10a.** Mr. Glass is sponsoring a unique art competition for Valentine's Day, where every artist will paint a picture using only red and white paints.
> 
> Each artist randomly receives 5 colors of paint, including at least 2 shades of red and at least 2 shades of white (the final color could be either red or white). If there are 8 shades of red and 12 shades of white available, how many different collections of colors could each artist receive?
- We can distribute this into distinct cases, depending on whether the last shade is red or white
	- **Case I.** Last shade is red. Then we have $\binom{8}{3}\binom{12}{2}$ collections of colors
	- **Case II.** Last shade is white. Then we have $\binom{8}{2}\binom{12}{3}$ collections
- Thus the total number of collections is $\binom{8}{3}\binom{12}{2} + \binom{8}{2}\binom{12}{3}$, assuming no replacement!
> **E1 q10b.** The contest will be judged anonymously, so each artist will use a secret code to identify their work. Their code must:
> - have length 6
> - consist of letters A-Z (26 options) and numbers 0-9
> - begin with three distinct letters (which could appear elsewhere)
> - contain exactly one 7
> How many secret codes are possible?
- We begin with 3 distinct letters, which can be chosen in $P(26, 3)$ ways -- with ordering considered
- Then we have 2 more slots for non-determined items, of which we have $26 + 9 = 35$ options (remember that we can't use a 7!). The number of ways we can choose this is $35^2$
- Finally, we must choose where to place the 7 between the last 2 characters. There are 3 places to put it, which makes $\binom{3}{1} = 3$ choices
- The number of possible secret codes is $P(26, 3)(35^2)(3)$
#### Combinatorial proofs and the binomial theorem
- **Def.** Combinatorial proofs are proofs of equivalence based on the property that the expression on the left-hand side counts the same thing as the expression on the right-hand side, albeit in a different way
	- Asks: given two expressions, can you reverse-engineer them into a problem where they both count the same thing?
- One strategy for solving difficult permutation problems is breaking down a complex decision into a series of simpler ones -- below is a good example of this:
> How many permutations of the letters in "MISSISSIPPI" where no 2 I's are consecutive?
- Recognize that no theorem works here trivially, so we'll need to break this down into a sequence of decisions
- First, we'll pick the order of the non-I letters, the possibilities of which we count via the multinomial theorem: $\frac{7!}{4!2!1!}$
- Then we need to figure out where to place the I's such that none of them are consecutive. Here's a visualization of the slots in which they can be placed _M_S_S_S_S_P_P_. There are 8 such slots, and we need to pick 4, which yields $\binom{8}{4}$ possibilities
- We multiply these together based on the product rule to get $\frac{7!}{4!2!1!} * \binom{8}{4}$ total possibilities
- Suppose that we want to expand the polynomial $(x+y)^n$. We recognize that doing this by hand is extremely tedious, so we introduce the binomial theorem
- **Thm.** The binomial theorem states that $(x+y)^n = \sum_{k=0}^{n}\binom{n}{k}x^ky^{n-k}$
	- This isn't always applied cleanly, and sometimes requires an approach called u-substitution, as we'll see in the below example
> Find the coefficient of $x^5y^7$ in $(2x - 7y)^{12}$.
- We rewrite the expression as $(u+v)^{12}$, knowing that $u = 2x, v = -7y$
- We then apply the binomial theorem to this modified expression, which gets us to a coefficient of $\binom{12}{5}$
- But we expand out our substitution, getting $\binom{12}{5}(2x)^5(-7y)^7$, and realize that our full coefficient is $\binom{12}{5}2^5(-7^7)$
- A more problem that deals with the binomial theorem results in what's called the multinomial coefficient
	- We start with the expression $(x_1 + x_2 + ... x_k)^n$ and want to determine the coefficient of $x_1^{n_1}x_2^{n_2}...x_k^{n_k}$
	- We know that this coefficient is derived by choosing the term $x_1$ exactly $n_1$ out of $n$ times, and so on
	- And so the coefficient becomes $\binom{n}{n_1} * \binom{n - n_1}{n_2} * \ldots$
	- This equates to the multinomial coefficient $\frac{n!}{n_1!...n_k!}$, which we formerly used to denote the number of ways to assign $n$ labels to things of $k$ types where objects that belong to a type are indistinguishable, and the number of elements in each type is known
	- This is but a formal proof of the multinomial coefficient
> Use the situation of the Lilliputian license plates from problem 4 in Homework 2 to give a combinatorial proof that $$26 \cdot 25 \cdot 10 \cdot 9 \cdot 8 \cdot \binom{5}{2} = \binom{26}{2}\binom{10}{3}5!$$
- Let's say that Lilliput has implemented a license plating system that requires each tag to consists of 2 distinct letters and 3 distinct numbers, that can be placed in any order
	- **Method 1:** We can count this by first counting the number of ways to pick 2 letters out of the 26-letter alphabet, which can occur in $\binom{26}{2}$ different ways
		- Then we count the number of ways to pick 3 numbers, which occurs in $\binom{10}{3}$ ways
		- For a 5-letter alphanumeric string with no repeated characters, there are $5!$ orderings
		- Thus the number of valid tags = $\binom{26}{2}\binom{10}{3}5!$, which is represented nicely by the right-hand side of this equality
	- **Method 2:** This is a little hard to derive independently (and I must admit, I used the solutions for it)
		- We first determine the location for the 2 letters, and then pick which 2 letters should fit in them (and in which order) - which we can do in $\binom{5}{2} * P(26, 2)$ ways
		- Then, we determine which numbers to place in the remaining three slots (and their order), which is counted by $P(10, 3)$
		- $\binom{5}{2} * P(26, 2) * P(10, 3)$ equates to the value on the left-hand side of this equality
- We therefore conclude by combinatorial proof that the given equality holds!
> **E1 q3.** Use a combinatorial proof to show that, for $n \geq k \geq 2$ $$\binom{n}{2}\binom{n-2}{k-2} = \binom{n}{k}\binom{k}{2}$$
- Let's say we are in ancient Rome, and want to pick 2 consuls and k senators out of a population of n. Assume that the consuls also count as senators
	- We can pick the consuls in $\binom{n}{2}$ different ways, and then must pick the remaining $k-2$ senators out of the rest, which can be done in $\binom{n-2}{k-2}$ different ways
		- Thus the left-hand side counts this situation appropriately
- Alternately, we can first pick the k senators, and then pick the 2 consuls from them
	- Picking the k senators can be done in $\binom{n}{k}$ different ways, and then picking the 2 consuls from them can be done in $\binom{k}{2}$ ways
	- This is equivalent to the right-hand side
- We therefore conclude by a combinatorial proof that the given equality holds
#### Combinations of indistinct objects, Stars and Bars, counting integer solutions
- **Thm.** The number of ways to distribute $n$ identical items into $k$ distinct groups is given by Stars and Bars: $$\binom{n + (k-1)}{k-1} = \binom{n + (k-1)}{n}$$
	- In this situation, $n$ gives the number of items to be categorized ("stars") and $k$ gives the groups into which they can be categorized ("bars")
- Situations that can be counted by this method:
	- Number of selections with repetition of $n$ items from a set of size $k$
	- Number of ways that $n$ identical objects can be distributed into $k$ groups
	- Number of non-negative integer solutions to $x_1 + x_2 + ... + x_k = n$
		- This is a little non-intuitive; the application here would just be with $n$ stars and $k$ bars
- Now onto an example where Stars and Bars isn't the obvious best-approach (and where determining that this is so constitutes basically all of the work of solving the problem)
> How many 7-digit phone numbers are there in which the digits are non-increasing?
- This means that a digit can be either equal to or less than its left-hand neighbor
- Another important implication is that given some pool of digits, there's only 1 way to order them given these constraints
- Thus it devolves to the simplest Stars and Bars: given a pool of 10 numbers, choose 7 where repetition is allowed
	- Here, we have 10 "bars" and 7 "stars", so our answer is simply $\binom{16}{9}$
> In how many ways can we distribute 8 identical tennis balls into 4 distinct baskets such that no basket is empty and the 4th basket has an odd number of balls in it?
- No basket is empty, so this problem can be translated to $x_1 + x_2 + x_3 + x_4 = 4$, assuming that all baskets already have 1 ball in them
- For the fourth basket, we can make cases and use the sum rule
	- **Case I.** basket 4 has 1 ball. This yields $\binom{6}{2}$ ways
	- **Case II.** basket 4 has 3 balls. This yields $\binom{4}{2}$ ways
	- **Case III.** basket 4 has 5 balls. This yields 1 way
- Adding these up, the total number of ways that we can distribute the balls is $\binom{6}{2} + \binom{4}{2} + 1$
> A co-ed ultimate frisbee league requires each team to have 7 members on the field at all times, at least 3 of whom must be men and at least 2 of whom must be women (the remaining 2 may be any gender). If a particular team has 5 men and 4 women on the roster, in how many ways can they choose 7 members to play?
- The safest way to answer this is by splitting it into discrete cases:
	- **Case I.** 3 men, 4 women - we can choose them in $\binom{5}{3}\binom{4}{4}$ ways
	- **Case II.** 4 men, 3 women - we can choose them in $\binom{5}{4}\binom{4}{3}$ ways
	- **Case III.** 5 men, 2 women - we can choose them in $\binom{5}{5}\binom{4}{2}$ ways
- Add up all of these to get the total of 37 ways to make the roster
> Determine the coefficient of $x^9y^3z^7$ in $(2x - 3y + 4z)^{19}$.
- Using u-substitution, we can convert the expression on the right to $(u + v + w)^{19}$, where $u = 2x, v = -3y, w = 4z$
- The coefficient of $u^9v^3w^7$ is equal to the multinomial coefficient of $\frac{19!}{9!3!7!}$
	- But we substitute the true values of $u, v, w$ back in and get the final expression $$\frac{19!}{9!3!7!}(2x)^9(-3y)^3(4z)^7$$
- This makes it so our final coefficient is actually $\frac{19!}{9!3!7!}2^9(-3^3)(4^7)$
>In Professor X's class, the students will do a project where each group presents on the development of mathematics on one of the 6 continents with long-term human inhabitants. 
>
>There are 27 students enrolled in the class, and they need to split into groups so that 4 students report on each of North America, South America, and Australia, and 5 students on each of Asia, Europe, and Africa.
>
>Since this is a big project, Professor X will assign one of his 6 graduate students to consult with each group. The trouble is, Ang grew up in Asia so assigning her to that group would be an unfair advantage - and Christopher is falling behind in his studies on North America so Professor X would like to use this opportunity to help him catch up.
>
>In how many ways can Professor X assign his students and grad students to groups so that each continent has the correct number of people reporting on it, Christopher is assigned to North America, and Ang is not assigned to Asia?
- It's quite clear to see that we'll need to use the multinomial formula to determine the number of ways that the students can be split up into groups. This should be equal to $\frac{27!}{4!^3(5!^3)}$
- We have 6 graduate students that should each be assigned to 1/the 6 groups. Christopher is assigned by default to North America, so we have 5 more to assign. Ang is not assigned to Asia, so we assign her first, with 4 possibilities (not including Asia)
	- For the other 4 grad students, there are $P(4, 4)$ ways of assigning them
	- Thus the total number of ways of assigning the grad students is $4 * P(4, 4)$
		- (this is equivalent to $4 * 4!$, but I was being dumb when solving this)
> Find the number of ways to buy 13 snacks in order from a vending machine that contains 1 Snicker's, 15 packs of peanuts, 17 fruit snacks, and 13 cookies.
- It's important to recognize that the amount of peanuts/fruit snacks/cookies is $\geq$ 12, so we can in theory select any amount of them that we want
- The non-trivial part of this question is our selection/non-selection of the Snicker's. We can thus separate this into two discrete cases:
	- **Case I.** We take a Snicker's. We can pick the other 12 snacks in $3^{12}$ ways, and then order them in $12!$ ways
		- There are exactly 11 slots in between these snacks, and 2 outside -- so 13 slots to place the Snicker's bar in the ordering
		- Choosing one of these slots constitutes $\binom{13}{1}$ options
	- **Case II.** We don't take the Snicker's bar. Then we must pick and order 13 snacks from the 3 remaining types, which we can do in $3^{13}(13!)$ different ways
- Since these cases are mutually exclusive, we simply add up their multiplicities to get our result: $$\binom{13}{1}(3^{12}) * 3^{13}(13!)$$
> **E1 q9.**  Find the number of integer solutions to the equation $a + b + c= 20$, where $0 \leq a, 2 \leq b, 2 \leq c \leq 4$.
- b and c both must be *at least* 2, so we'll transform this equation into $a + b' + c' = 16$
- Here, c' can equal 0, 1, or 2, and we can make discrete cases out of these and apply the sum rule afterwards
	- **Case I.** c' = 0. Then we must solve $a + b' = 16$, which has 16 stars and 1 bar. $\binom{17}{1}$ ways of doing this
	- **Case II.** c' = 1. Then we must solve $a + b' = 15$, of which there are $\binom{16}{1}$ ways of doing
	- **Case III.** c' = 2. Then we must solve $a + b' = 14$, of which there are $\binom{15}{1}$ ways
- Thus the number of integer solutions to this equation with the provided constraints is $17 + 16 + 15$

