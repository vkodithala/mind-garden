#### Principle of inclusion-exclusion
- **Def.** The principle of inclusion-exclusion (PIE) states that $$N(\overline{a_1}...\overline{a_n}) = |A| - \sum_{1 \leq i \leq n}N(a_i) + \sum_{1 \leq i \leq j \leq n}N(a_ia_j), \ldots$$
	- Where $a_1 \ldots a_n$ represent some series of conditions that you want to exclude, and |A| counts the entire solution space!
	- More generally, $$N(\overline{a_1}...\overline{a_n}) = |A| - S_1 + S_2 + \ldots + (-1^n)S_n$$
	- In this equation, $S_k$ represents the number of ways that exactly $k$ of the conditions can be violated
- Let's now use PIE to solve a problem that we might see in the real world![[Screenshot 2025-05-03 at 4.12.44 PM.png]]
> Find the number of integer solutions to $x+y+z=12$, with the conditions $0 \leq x \leq 5$, $0 \leq y \leq 7$, and $0 \leq z \leq 6$.
- We can formulate three conditions: $a_1$, which is where $x \notin [0, 5]$, $a_2$, which is where $y \notin [0, 7]$, and $a_3$, where $z \notin [0, 6]$
- Then we use PIE to solve this problem
- |A| (the solution space, not considering those conditions) can be found via stars and bars, with 12 stars and 2 bars: $\binom{14}{2}$
- $S_1$ counts the number of solutions where $a_1$, $a_2$, or $a_3$ evaluate to false
	- If $a_1$ is false, then $x > 5$. If x is at least 5, this means that the problem simplifies to $x' + y + z = 7$, where x' = x - 5. The number of solutions to this is the number of ways for $a_1$ to be false. By Stars and Bars, this is $\binom{8}{2}$
	- By similar logic, the number of ways for $a_2$ and $a_3$ to be false is $\binom{6}{2}$ and $\binom{8}{2}$, respectively
- Thus, $S_1 = \binom{8}{2} + \binom{6}{2} +  \binom{8}{2}$
- $S_2$ counts the number of ways that any pairs of conditions evaluate to false. We can distribute this into cases:
	- Case I. $a_1, a_2$ are false. This means that $x \geq 6, y \geq 8$, and the equation transforms to $x' + y' + z = 12 - 14$, which is impossible. Thus there are no solutions to this
	- Case II. $a_2, a_3$ are false. Same logic as Case I.
- Thus our final solution is simply $$|A| - S_1 = \binom{14}{2} - (\binom{8}{2} + \binom{6}{2} +  \binom{8}{2})$$
>**Hw5 q1.** A group of 56 ambitious students will take summer classes at the Institute of College University. Of these students, 24 will take a math class, 32 will take an English class, and 16 will take a lab science.
>Further, we know that 4 of these students will take math and science, 12 will take English and science, and some number will take math and English. How many students must be taking all three in order to guarantee that no student is only taking math and English and exactly one student is taking Science only?
- We're given that exactly 1 student is taking only Science
- The total number of students taking a lab science, though, is 16 and if $X$ is equivalent to all of the students taking all 3 subjects, $16 = 4 + 12 + 1 + X$, so X then equals $1$
> **Hw5 q3.** How many nine-digit sequences have each of the numbers 2, 5, and 8 appearing at least once?
- This seems to be a pretty trivial application of PIE
- Let $a_1, a_2, a_3$ denote the conditions that 2/5/8 appear zero times, respectively
	- $N(a_1) = 9^9$, and this is equal to $N(a_2)$ and $N(a_3)$. Reasoning is that in any of these cases, our nine-digit sequences are formed from 9 numbers instead of 10 -- as we exclude one. And so there are $9^9$ ways to pick the sequence by combinatorics. Thus $S_1 =  3(9^9)$
	- If we exclude 2 of the digits, then we have 8 left over. The number of ways that we can make a 9-digit sequence from 8 composing elements is $8^9$. Since there are exactly 3 scenarios where 2 of the digits are excluded, $S_2 = 3(8^9)$
	- Finally, if we exclude all 3 of these digits, then we have 7 digits left over with which to construct our sequence. There are $7^9$ to do this, and only one scenario in which all 3 digits are excluded. Thus, $S_3 = 7^9$
- Thus, $N(\overline{a_1a_2a_3}) = 10^9 - 3(9^9) + 3(8^9) - 7^9$
> **Hw5 q4.** In how many ways can the letters of the word ARRANGEMENT be arranged so that there are *exactly* two pairs of consecutive identical letters?
- To solve this, we must use another formula gone over in class that we forgot to transcribe
	- $E_k$ represents the number of ways that exactly $k$ conditions can be met, and is equivalent to $E_k = S_k - \binom{k+1}{1}S_{k+1} + \binom{k+2}{2}S_{k+2} + ... + (-1)^n\binom{k+n}{n}S_{k + n}$
- Thus we must solve for $E_2 = S_2 - S_3 + S_4$
- We recognize first that there are 4 pairs of matching letters in the word, that each appear 2x: A, R, N, E
	- We need to pick 2 of these, which we can do in $\binom{4}{2}$ ways
	- Then we treat the chosen numbers as a block and go on to pick the ordering, which can be done in $\binom{9}{2, 2}$ ways
	- Thus $S_2 = \binom{4}{2}\binom{9}{2, 2}$
- Choosing 3 pairs is done in $\binom{4}{3}$ different ways, and then the number of permutations of the resulting word is $\binom{8}{2}$
	- Thus $S_3 = \binom{4}{3}\frac{8!}{2!}$
- And the number of permutations with 4 pairs is simply $7!$
- Thus the number of valid arrangements of the letters of the word is $\binom{4}{2}\binom{9}{2, 2} - \binom{3}{1}\binom{4}{3}\frac{8!}{2!} + \binom{4}{2}7!$
> **E2 q1.** Dr. Shannon wants to find out how many exams her students have during the week before spring break. She decides to use PIE to perform some calculations, and will use conditions
> - $a_1$: the student has an exam on Monday
> - $a_2$: the student has an exam on Tuesday
> - $a_3$: the student has an exam on Wednesday
> - $a_4$: the student has an exam on Thursday
> - $a_5$: the student has an exam on Friday
- (a) What is the universal set $A$ that can be best used with these conditions?
	- A = {All students in Dr. Shannon's class}
- (c) Dr. Shannon wants to know how many students have at least 2 exams in the week. What calculation should she perform?
	- She needs to find $|A| - E_1 - E_0$
> **E2 q2.** Let $X = \{x_1, \ldots, x_{10}\}$ and $Y = \{y_1, \ldots, y_5\}$. How many functions $f: X \to Y$ have exactly 3 elements in their range?
- We let our conditions $a_k$ represent the fact that some value in the codomain $y_k$ does NOT exist in the range of the function
- We want to find the number of functions in which exactly 2 elements are left out of the range, so $E_2$
- We know from prior work that $E_2 = S_2 - \binom{3}{1}S_3 + \binom{4}{2}S_4 - \binom{5}{3}S_5$
	- $S_2$ counts the number of functions in which exactly $k$ conditions are violated, which means that 3 elements are in the range and this is thus $\binom{5}{3}3^{10}$
	- $S_3$ counts the number of functions in which 3 conditions are violated, so 2 elements are in the range, this is $\binom{5}{2}2^{10}$
	- $S_4$ counts the number of ways that 1 function can be in the range, which is $5(1)$, and $S_5$ is nonexistent
#### Derangements
- **Def.** For a set of elements $X$ and a function $f: X \to X$, a derangement is a function that maps none of the elements in the domain to their "mirror" in the codomain
	- In other words, $(x, x)$ isn't mapped to in the function for any $x \in X$
- It's not useful to worry ourselves with the derivation, but the number of derangements of $n$ objects is $!n = [\frac{n!}{e}]$, where the brackets represent rounding to the nearest integer
	- For $n \geq 7$, this calculation is correct by at least 4 decimal places, so rounding works quite well in most cases
> **Hw6 q1.** Use a combinatorial proof to show that the following equality holds for any positive integer $n$: $$n! = \binom{n}{0}(!0) + \binom{n}{1}(!1) + \ldots + \binom{n}{n}(!n)$$
- Let's say that we are $n$ people visiting Six Flags and drop off our bags. On our way back, we want to determine the number of ways that our bags are handed back to us
	- We can count this by solving $n!$. This is based on the idea that our bags can be handed back to us in any (random) order, and the number of such orderings is $n!$
	- We can also count this by saying that some number of people won't receive their own bags. In other words, some number of people in the range $0 \to n$ will be *deranged*. The right-hand side counts this (we can prove this mathematically, but for the sake of time I'll leave it here)
- Since the left and right-hand sides of the proposed equality both count the same quantity, we find them to be equal by a combinatorial proof
> **Hw6 q2.** How many permutations of the string 1234567 are not derangements?
- This string contains 7 characters. Without any restrictions, the number of ways to permute its components is simply $7!$
	- The number of derangements of any set with multiplicity $n$ is simply $!n = [\frac{n!}{e}]$, and this is no exception. So the number of derangements of this 7-character sequence is simply $[\frac{7!}{e}]$
- Thus, the number of permutations of the string that are *not* derangements is equal to $7! - [\frac{7!}{e}]$
> **Hw6 q3.** As an April fools' prank, Puck the TA decides to return all of his class' graded work to the wrong students. In how many ways could he return two quizzes and an exam to his 25 students in such a way that no student gets any of their own papers?
- The clarification states that each of these assignments are distributed independently. The number of ways to derange each of them is thus $!25 = [\frac{25!}{e}]$
- Since we're deranging all 3 assignments independently, we use the product rule. Our result is $[\frac{25!}{e}]^3$
> **E2 q3.** One morning, a group of mischievous leprechauns decide to relabel the lunches in Big Media Company's fridge. In how many ways could the leprechauns put the labels back on the 18 lunches in the refrigerator so that exactly 5 people's lunch is labeled correctly?
- We want to label the 5 people's lunches correctly, which can be done in only 1 way, and then derange the other 18. So first, we need to choose which 5 people, which can be done in $\binom{18}{5}$ ways, and then derange the other 13, which can be done in $!13 = [\frac{13!}{e}]$ ways
#### Rook polynomials
- **Def.** Say that we want $k$ rooks on a chess board. By the definition of rook movement in chess, we must place them so that none of the rooks share a row or column.
	- The number of ways that we can do this is denoted by the $k^{th}$ rook number of $c$ $r_k(c)$, where $c$ is the board
	- For any chess board, $r_0(c) = 0, r_1(c)$ = the total number of available squares, and $r_n(c)$ for an n that's larger than |cols| or |rows| = 0
- **Def.** The rook polynomial of a chess board $c$ is simply $r(C, x) = \sum_{k=0}^{\infty}r_k(c)(x^k)$
- **Prop.** If a chess board consists of $n$ pairwise disjoint sub-boards $c_1 \ldots c_n$ such that none of them share a row or column, the rook polynomial of $c$, $r(c, x)$, is equal to $r(c_1, x) * r(c_2, x) * \ldots * r(c_n, x)$
- **Thm.** Any 2 rows and columns of a chess board $c$ can be swapped without changing its rook polynomial
	- This is useful for getting more favorable boards/complement boards that have easier-to-compute sub-boards
- **Notation.** Given a chess board $c$, we will use $\overline{c}$ to denote the board where rooks can only be placed on the black squares (the opposite of how we'd normally place rooks)
- Rook polynomials give us a framework with which we can count the number of ways that we can satisfy functions with forbidden mappings
> **Hw6 q4.** Find the rook polynomial of a 3 x 3 square by counting the number of ways $k$ rooks can be placed.![[Screenshot 2025-04-29 at 6.05.11 PM.png]]
- We can choose any of the 9 squares to place the first rook, so there are $\binom{9}{1}$ ways to do this
	- This eliminates 1 entire row and column, which reduces our choices by 5. $9 - 5 = 4$
	- We then have 4 squares to pick from for our second rook, so $\binom{4}{1}$
	- Since we only care about which 2 non-attacking squares were chosen and not their order of selection, we divide this result by 2 to get $\frac{36}{2} = 18$
- Thus $r_1(c) = 9, r_2(c) = 18$, and of course $r_0(c) = 1$
- Now we must compute $r_3(c)$, as this is the last plausible rook number
	- We know that 1 rook must be in each column, so we simplify this to picking what rows each rook will be in
	- This leaves $3 * 2 * 1 = 3! = 6$ options
- Thus our final rook polynomial is $r(c, x) = 1 + 9x + 18x^2 + 6x^3$
> **Hw6 q5.** Professor Oftheyear wants to give each of the 5 students in their class a unique candy to congratulate them on making it through the first exam. The available candies are a butterfinger, a peanut butter cup, a packet of m&ms, a packet of skittles, and a snickers. There are some restrictions, though:
> - Abeer has a mild peanut allergy, and so should not receive the butterfinger or peanut butter cup
> - Ben wants a real candy bar, not m&ms or skittles
> - Corinne does not want a peanut butter cup
> - Drake does not want m&ms
> - Evan doesn't like peanut butter cups or snickers
- **(a)** Sketch a chessboard C that represents the allowed/disallowed candy distributions.![[220 More on rook polynomials, applications.jpeg]]
- **(b)** Find the rook polynomial $r(\overline{C}, x)$ of your chessboard.![[IMG_F710FDE5FEC1-1.jpeg]]
	- Based on the above derivation, the rook polynomial for the complement board $r(\overline{C}, x) = 1 + 8x + 21x^2 + 21x^3 + 8x^4 + x^5$
- **(c)** Use PIE and your work from part (b) to calculate the number of possible distributions. Don't forget to define your conditions carefully!
	- Let's say that we have 5 conditions $a_1, a_2, a_3, a_4, a_5$ that each represent violations of the rules set forth in the problem statement for Abeer, Ben, Corrine, Drake, and Evan
	- Then the number of ways that we can assign candies to the 5 of them without breaking any of these conditions is equal to, by PIE, $|A| - S_1 + S_2 - S_3 + S_4 - S_5$
		- $S_k$ for some $k$ in range $1 \ldots 5$ is equal to the rook polynomial of the complement grid, which gives the number of ways that $k$ of the preconditions can be met, times $(n - k)!$ to represent the permutations of the other arrangements
	- This is in turn equivalent to $5! - 8(4!) + 21(3!) - 21(2!) + 8(1!) - 1(0!)$
> **E2 q4.** How many ways can 2 non-attacking rooks be placed on a 4 x 6 board? That is, find $r_2(c)$.
- We need to choose 2 rows and 2 columns, which can be done in $\binom{6}{2}$ and $\binom{4}{2}$ ways, respectively
- We have 2 rows and 2 columns, and thus 2! ways to map them to one another **(this is pretty unintuitive)**
- Final answer becomes $2 * \binom{6}{2}\binom{4}{2}$
> **E2 q5.** Find the rook polynomial of the black squares for the board given below, and simplify your answer.![[Screenshot 2025-04-30 at 6.28.48 PM.png]]
- ![[IMG_BEBB52623B58-1.jpeg]]
- By the above work, we get the rook polynomial $1 + 10x + 12x^2 + 8x^3 + 2x^4$
#### Ordinary generating functions (and useful manipulations)
- Generating functions give us a way to encode sequences as the coefficients of a polynomial
	- The problem arises when we have sequences like $(0, 1, 0, 0, 2, \ldots)$ that are sparsely populated
		- In other words, these sequences mostly just contain zeroes
	- A generating function $p(x)$ encodes the terms of a sequence $(C_n)_{n \geq 0}$ in its coefficients
		- This gives us a better framework to represent this sequence!
- **Def.** An ordinary generating function (OGF) encodes a sequence $(C_n)_{n \geq 0}$ as the coefficients of $x^n$
	- For example, let's say we have the sequence $(1, 2, 3)$. Its OGF would be $1 + 2x + 3x^2$
	- A good example is the rook polynomial, which is simply an OGF of the sequence of rook numbers
> Suppose $x_1 + x_2 + x_3 = 10$, and we have constraints $2 \leq x_1 \leq 4$, $3 \leq x_2 \leq 5$, and $2 \leq x_3 \leq 5$. 
- There are a few ways we can solve this: by eliminating the lower bounds with modified variables, and then using PIE to deal with the upper bounds, brute force, or with OGFs
- The way to solve this with OGFs isn't intuitive and hasn't been discussed before, so we will introduce it here
	- We first make generating functions for each of the values $x_1, x_2, x_3$ that encode the possible values that they can take on
	- Here, $x_1 = y_2 + y_3 + y_4$, $x_2 = y^3 + y^4 + y^5$, and $x_3 = y^2 + \ldots + y^5$
	- We then combine these functions by multiplying them, and our result is an OGF encoding all of the ways that the variables can sum to some value $y^k$
		- $c_ky^k$ means that there are $c_k$ ways of summing to $y_k$
- And so we simply find the coefficient of 10 (which happens to be 8, in the form $8y^{10}$) in this final OGF
- Remember: OGFs themselves are a pretty standard (and easy to understand) methodology. The hard part is translating word problems to OGFs
	- Another problem with OGFs is that while they are powerful for counting really convoluted things, they suffer from the complexity of expanding out power series to get coefficients
- We shall introduce the closed form with a motivating example
	- We know from our work with the binomial theorem that $(x + y)^n = \sum_{k=0}^{n}\binom{n}{k}x^ky^{n-k}$
	- If we instead substitute 1 for y, we get that $(x + 1)^n = \sum_{k=0}^{n}\binom{n}{k}x^k1^{n-k} = \sum_{k=0}^{n}\binom{n}{k}x^k$
	- We know also that for a sequence $\binom{n}{k}_{k \geq 0}$, a generating function would be $\sum_{k=0}^{n}\binom{n}{k}x^k$, and thus the "closed form" of this generating function is simply $(x+1)^n = (1+x)^n$
- Important OGF $\to$ closed form translations (that you should memorize)
	- $(1 + x)^n$ = $\sum_{i=0}^{\infty}\binom{n}{i}x^i$, and generates the sequence $\binom{n}{i}_{i \geq 0}$ - we prove this one cell earlier
	- $\frac{1}{1-x} = \sum_{i=0}^{\infty}(x^i)$, and generates the sequence $(1, 1, 1, \ldots)_{i \geq 0}$
	- $e^x = \sum_{i=0}^{\infty}\frac{x^i}{i!}$, which generates the sequence $(\frac{1}{i!})_{i \geq 0}$
- Ways to manipulate OGFs:
	- **(1) Shift by a constant -** let's say we have a sequence $(ca_i)_{i \geq 0}$ and know that the generating function of $(a_i)_{i \geq 0}$ is $f(x)$. Then the generating function of this sequence times a constant multiple $c$ is simply $c \cdot f(x)$
	- **(2) Shift by a power -** suppose we have a generating function $f(x)$ that generates the sequence $(a_0, a_1, \ldots)$, but want it to instead generate this, right-shifted by 1, so $(0, a_0, a_1, \ldots)$
		- We can do this by multiplying $f(x)$ by $x$ to capture our new, right-shifted sequence
	- **(3) Differentiation -** we will approach this with a problem. Let's say that $f(x)$ generates $(a_i)_{i \geq 0}$, but we want to generate $(i \cdot a_i)_{i \geq 0}$
		- We know that the generating function has form $\sum_{i=0}^{\infty}a_ix^i$, and differentiating it gives us $\sum_{i=0}^{\infty}i \cdot a_ix^{i-1}$. This gets us halfway there
		- And then our final step is to shift by a power - multiplying the whole thing by $x$ gets us to $x \cdot \sum_{i=0}^{\infty}i \cdot a_ix^{i-1} = \sum_{i=0}^{\infty}i \cdot a_ix^{i}$, as desired
	- **(4) Substitution -** if a generating function looks *similar* to any of our known closed forms, then we can use substitution to translate them into those closed-forms and generate our resulting sequences
		- Motivating example: what sequence does $e^{y^2}$ generate?
		- This looks awfully similar to $e^x$, which we know generates the sequence $(\frac{x}{i!})_{i \geq 0}$, so we just use the substitution $x = y^2$
		- Substituting back the true value of $x$, we get the sequence $( \frac{y^2}{i!})_{i \geq 0}$, which then evaluates to the summation $\sum_{i=0}^{\infty}\frac{1}{i!} \cdot y^{2i}$
		- We can evaluate this to find our resulting sequence
	- **(5) Addition -** the sequence $(a_i + b_i)_{i \geq 0}$ is generated by $f(x) = f_a(x) + f_b(x)$, where $f_a(x)$ generates $a_i$ and $f_b(x)$ generates $b_i$
	- **(6) Convolution -** if $f_a(x)$ generates $(a_i)_{i \geq 0}$ and $f_b(x)$ generates $(b_i)_{i \geq 0}$, then their convolution is the sequence $(c_i)_{i \geq 0}$ generated by $f_a(x) \cdot f_b(x)$
		- By definition, $c_i = \sum_{n=0}^{i}a_nb_{i-n}$
> **Hw7 q1.** Determine an ordinary generating function that gives the number of integer solutions to the equation $$c_1 + c_2 + c_3 + c_4 + c_5 = n$$with constraints $2 \leq c_1 \leq 4, 3 \leq c_i \leq 8$ for $2 \leq 1 \leq 5$.
- A generating function for $c_1$ is $y^2 + y^3 + y^4$
- A generating function for $c_i$ where $i \in [2, 5]$ is $y^3 + \ldots + y^8$
	- There are exactly 4 of these $c_i$'s, so we must exponentiate its generating function by 4
- Thus the generating function that gives the number of integer solutions to this constrained equation is $(y^2 + y^3 + y^4)(y^3 + y^4 + y^5 + y^6 + y^7 + y^8)^4$
	- For $n = 30$, the number of solutions to this equation is the coefficient of $y^{30}$ in the expansion of this equation
> **Hw7 q2.** Find the sequence generated by each of the following ordinary generating functions.
- (a) $f(x) = \frac{4x^2}{2-6x}$. We can first factor out a 2 to get $\frac{2x^2}{1 - 3x}$
	- Next, we can factor out the $2x^2$ (taking advantage of the "shift by a power" rule), which gets us to $2x^2\frac{1}{1-3x}$
	- After that, we can use the substitution $y = 3x$ to get $2x^2\frac{1}{1-y}$, and know that $\frac{1}{1-y}$ generates the sequence $(y^i)_{i \geq 0}$. Substituting back in the true value, the sequence that we get is $(3^ix^i)_{i \geq 0}$, and then we multiply this by $2x^2$
	- The final generated sequence is $\sum_{i=0}^{\infty}2(3^{i})x^{i+2}$
- (b) $g(x) = \frac{2}{1+x^2} - e^{3x}$. We'll first find the sequences generated by both sides, and then use the addition rule to combine them
	- On the left side, we first factor out the 2 and then substitute $y = -x^2$ to get the expression $\frac{1}{1-y}$. This generates $\sum_{i=0}^{\infty}y^i$, and then by substitution we get the summation $\sum_{i=0}^{\infty}-2x^{2i}$
	- We use substitution on the right-hand side, too. We substitute $y = 3x$ to get the sequence $\sum_{i=0}^{\infty}\frac{3x^i}{i!}$, which translates to $\sum_{i=0}^{\infty}\frac{3}{i!}x^i$
	- By the addition rule, the sequence generated is thus $$\sum_{i=0}^{\infty}-2x^{2i} - \frac{3}{i!}x^i$$
> **Hw7 q3.** Find, in closed form, each of the following:
- (a) The ordinary generating function for the sequence $(2, 0, 2, 0, \ldots)$.
	- This mirrors the sequence $2(x^{2i})_{i \geq 0}$. We can use substitution here on the inner part to get $\frac{1}{1-x^2}$, and then multiply by 2
	- The final OGF for this is $\frac{2}{1-x^2}$
- (b) The ordinary generating function for the sequence $(6, 27, 128, 629, \ldots) = (1 + 5, 2 + 5^2, 3 + 5^3, 4 + 5^4)$
	- This generating function represents the addition of two sequences, and we can apply addition rule - these are $(1, 2, 3, 4, \ldots)$ and $(5, 5^2, 5^3, 5^4, \ldots)$
		- The first of these sequences is $\sum_{i=0}^{\infty}(i + 1)x^i$. We can solve this by differentiation. We know that $\sum_{i=0}^{\infty}x^i$ translates to $\sum_{i=0}^{\infty}(i)x^{i-1}$ after differentiation, which means equivalently $\sum_{i=0}^{\infty}(i+1)x^i$. This is what we want. And the generating function is $\frac{d}{dx}(\frac{1}{1-x}) = \frac{1}{(1-x)^2}$
		- The second of these sequences is generated by $\sum_{i=0}^{\infty}5^{i+1}x^i$, and can transform this into $5 \cdot \sum_{i=0}^{\infty}(5x)^i$. By substitution with a known closed form, this translates to $\frac{5}{1-5x}$
	- The final generating function is simply $\frac{1}{(1-x)^2} + \frac{5}{1-5x}$
> **E2 q6.** Let $(a_0, a_1, \ldots, a_n, \ldots)$ be the sequence defined by $a_n = \binom{n}{2}$ for $n \geq 0$. Determine the closed-form generating function for the sequence.
- We use differentiation to arrive at our result:![[IMG_46E97124C2CF-1.jpeg]]
- At the end there, we forgot to multiply the $\frac{1}{2}$ that we had before the summation, so our result ends up being $\frac{x^2}{(1-x)^3}$, actually
> **E2 q7.** Determine the ordinary generating function for the sequence $(s_n)_{n \geq 0}$, where $s_n$ is the number of non-negative integer solutions to the equation $$4a+3b+5c=n$$
- We can represent this as $a' + b' + c' = n$, where $a' = 4a, b' = 3b, c' = 5c$ and find generating functions for each of these
- The generating function for $a'$ would be simply $1 + x^4 + x^8 + \ldots$, and similar things would follow for $b'$ and $c'$
- Our OGF for the whole sequence would be the product of each of these generating functions
> **E2 q9.** Find the sequence whose ordinary generating function is given by $e^x - e^{-x}$.
- We know that $e^x$ generates the sequence $(\frac{1}{i!})_{i \geq 0}$, and by substitution that $e^{-x}$ generates the summation $\sum_{i=0}^{\infty}\frac{-x^i}{i!}$ which means that the sequence itself is simply $(\frac{-1^i}{i!})_{i \geq 0}$
- We use the addition rule to determine the sequence generated by the subtraction of both of these, which is $(\frac{1 - (-1^i)}{i!})_{i \geq 0}$
#### Integer Partitions
- **Def.** For a positive integer $n$, a partition of $n$ is a decomposition of it into positive, unordered summands. For example, a decomposition of $10$ into $5 + 3 + 2$ would be considered a partition
	- The number of partitions of $n$ is denoted $p(n)$. This is hard to compute directly, but a generating function $f(x) = \sum_{n=0}^{\infty}p(n)x^n$ is far easier to find
	- We find this generating function by making an OGF for each possible summand, and then multiplying them together
> **Ex.** Find the OGF for the number $p_O(n)$ which is the number of partitions of n into odd summands.
- Let's try a few examples:
	- From m = 1, we can generate $0, 1, 2, \ldots$, so we have the generating function $1x^0 + 1x^1 + 1x^2 + \ldots + 1x^k$
	- From m = 3, we can generate the sequence $0, 3, 6, \ldots$ and thus have the generating function $\sum_{i=0}^{\infty}x^{3i}$ and the closed form $\frac{1}{1-x^3}$
	- For some m = k where k is odd, the closed form of the generating function is $\frac{1}{1-x^k}$
- So the OGF of $p_O(n)$ is simply $$\pi_{i=1}^{\infty}\frac{1}{1-x^{2i-1}}$$
- **Prop.** If $(a_n)_{n \geq 0}$ and $(b_n)_{n \geq 0}$ are generated by the same function, then $a_n = b_n \forall n \geq 0$ and thus both sequences are equal
	- Many such examples, and we will give some from the assigned homeworks
- Let's say that we wanted to find the coefficient of $x^8$ in the sequence generated by $$f(x) = \frac{1}{(x-3)(x-2)^2}$$
	- We might jump straight into convolutions, recognizing that $f(x) = \frac{1}{x-3} \cdot \frac{1}{(x-2)^2}$
	- But this requires a pretty difficult set of multiplications, so we opt for a different approach
- A different (and better) approach to this problem is to use partial fraction decomposition, by which we represent $f(x)$ as $$\frac{A}{x-3} + \frac{B}{x-2} + \frac{C}{(x-2)^2}$$
	- Then we solve for A, B, and C and find the power series representation of each piece, adding them together to get the final OGF
	- The benefit here is that this is addition and not multiplication, like you'd get with convolutions - it's much easier!
> **Hw8 q1.** Find the coefficient of $x^5$ in the power series expansion of the ordinary generating function $$f(x) = \frac{2}{3x^2 - 4x + 1}$$
- This can be rewritten as $2\frac{1}{(3x-1)(x-1)}$, and then we can use partial fraction decomposition on the right side![[IMG_D49E1FEFC796-1.jpeg]]
- By the proof above, we get that the coefficient of any $x^i$ is simply $3^{i+1}-1$, and thus the coefficient of $x^5 = 3^6 - 1 = 728$.
> **Hw8 q2.** Find the closed form of the ordinary generating function for the convolution of the sequences $(n^2)_{n \geq 0}$ and $(n)_{n \geq 0}$.
- We know that the convolution of two sequences is generated by the product of their generating functions, so we resolve to find the generating functions of both of these sequences and multiply them together
- $(n^2)_{n \geq 0}$ represents the sequence $\sum_{i=0}^{\infty}n^2x^n$,  and we can find its closed form via differentiation of the sequence with known closed form $\sum_{i=0}^{\infty}x^n$
- By the work shown below, the OGF of the convolution is $\frac{x}{(1-x)^2}\frac{x(1-x^2)}{(1-x)^4}$
> **Hw8 q3.** Consider the convolution $(c_n)_{n \geq 0}$ of the sequence $(1, 1, 1, 1, \ldots)$ with an arbitrary sequence $(b_n)_{n \geq 0}$.
- **(a) Find the first 4 terms $c_0, c_1, c_2, c_3$ of the convolution.** 
	- The first term would be $1 \cdot b_0 = b_0$
	- $c_1 = 1 \cdot b_1 + 1 \cdot b_0 = b_1 + b_0$
	- $c_2 = 1 \cdot b_2 + 1 \cdot b_1 + 1 \cdot b_0 = b_2 + b_1 + b+0$
	- Based on a similar line of reasoning, $c_3 = \sum_{i=0}^{3}b_i$
- **(b) Use the definition of a convolution to show that, in general, $c_n = \sum_{i=0}^{n}b_i$.**
	- From the definition of a convolution, we know that the sequence generated is $(c_i)_{i \geq 0}$ where $c_i$ itself is simply $\sum_{n=0}^{i}a_ib_{n-i}$
	- We know that $a_i = 1 \forall i$, and that $b_{n-i}$ holds subscripts in the range $0 \ldots n$. Thus, our final generating function is simply $\sum_{n=0}^{i}b_n$
> **Hw8 q4.** Give an ordinary generating function for the sequence $(p_n)_{n \geq 0}$, where $p_n$ is the number of partitions of the integer $n$ such that odd summands appear at most once, and even summands appear an even number of times.
- For an odd number $k$, we know that the sequence generated is $k^0 + k^1$, and its generating function is $\sum_{i=0}^{1}x^i$. Since we only consider odd numbers, this can be $1 + x^{2k + 1}$ for all odd numbers $k$
- For an even number $k$, we know that the sequence generated is $k^0 + k^2 + \ldots + k^{2n}$, which is represented by the generating function $\sum_{k=0}^{\infty}x^{2k}$, which has the closed form $\frac{1}{1-x^{2k}}$
- Thus our final OGF for the sequence of partitions is $$\pi_{m=1}^{\infty}(1 + x^{2m-1})\frac{1}{1-x^{4m}}$$
> **E2 q10.** Suppose we want to know the number of partitions of 5 where:
> - even summands appear at most twice, and
> - odd summands appear at most once
> This number is the coefficient of $x^5$ in which of the following functions?
- We know that for some even summand $k$, it appears at most twice so its generating function is simply $1 + x^k + x^2k$
- And then for some odd summand $m$, its generating function is $1 + x^m$
- So we need to take all even and odd summands in range $1 \ldots 5$, and multiply their generating functions, and get $(1 + x) \cdot (1 + x^2 + x^4) \cdot (1 + x^3) \cdot (1 + x^4 + x^8) \cdot (1 + x^5)$