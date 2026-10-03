# CSC110: Programming and proofs

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Quantifier order controls meaning and permissible choices

## Reading quantified statements
A **quantifier** says how a variable ranges over a domain: ∀ means “for every,” and ∃ means “there exists.” Here ℤ denotes the integers, ℕ the nonnegative integers, and ℝ the real numbers. A **witness** is a value supplied to establish an existential statement. An **arbitrary** variable represents any member of its stated domain, without a special choice that would make the claim easier.

Compare:
- ∃x ∈ ℕ, ∀y ∈ ℕ, x ≥ y: one fixed natural number is at least every natural number. This is false: after any proposed x, y = x + 1 defeats it.
- ∀y ∈ ℕ, ∃x ∈ ℕ, x ≥ y: for each given y, choose a suitable x. This is true; x = y + 1 works. Although the formula only requires “at least,” this choice even gives a strictly larger number.

The difference is not the letters: it is whether the choice can respond to the other variable. Introduce variables in quantifier order. An existential witness may depend on variables already introduced, not on variables that appear later. This resembles Python **scope**, the part of a program in which a name is available.

## Zero-sum example and disproof
For ∃n ∈ ℤ, ∀m ∈ ℤ, m + n = 0, starting “choose n = −m; now let m ∈ ℤ” is invalid: m was unavailable when n had to be chosen. In contrast, ∀m ∈ ℤ, ∃n ∈ ℤ, m + n = 0 is true: let m be arbitrary, choose the integer n = −m, and calculate m + n = 0.

An invalid proposed proof alone does not show a statement is false. To **disprove** the original statement, prove its negation. Negation reverses each quantifier and negates the final claim. Renaming the variables to match the board example:

¬(∃m ∈ ℤ, ∀n ∈ ℤ, m + n = 0)
⇔ ∀m ∈ ℤ, ∃n ∈ ℤ, m + n ≠ 0.

Let m ∈ ℤ be arbitrary. Choose n = −m + 1, which is an integer. Then m + n = m − m + 1 = 1 ≠ 0. This supplies a successful response to every possible m, so the negation is true.

A fixed choice such as n = 10 does not prove this negation: the arbitrary m might be −10. More generally, any fixed integer c fails at m = −c. The chosen n must avoid the particular value −m; using m makes it easy to do so.

## Worked parity proofs: responding to an integer versus choosing once

An integer t is **even** when ∃p ∈ ℤ, t = 2p. It is **odd** when ∃p ∈ ℤ, t = 2p + 1. Thus proving evenness requires both a suitable integer witness and the required equality. The witness letter is immaterial: p or k is fine, provided it is used consistently.

## Statement 1: a separate response to each m
∀m ∈ ℤ, ∃n ∈ ℤ, Even(m + n).

In English: for every integer m, there is an integer n, possibly depending on m, such that their sum is even. Expanding the definition gives

∀m ∈ ℤ, ∃n ∈ ℤ, ∃p ∈ ℤ, m + n = 2p.

**Proof.** Let m ∈ ℤ. Choose n = m, an integer. Choose p = m, also an integer. Then m + n = m + m = 2m = 2p. This proves the expanded statement, hence the original one.

**Finding the choices.** In rough work, work backward from the goal m + n = 2p. Choosing n = m makes the left side 2m, suggesting p = m. This backward exploration helps discover a proof, but the final proof proceeds forward from the permitted introductions and choices. We choose n and p, not the arbitrary m. A split into the cases “m even” and “m odd” would be possible but unnecessary. Other choices mentioned in class also work: n = 3m with p = 2m, or n = 5m with p = 3m.

## Statement 2: one response for all m
∃n ∈ ℤ, ∀m ∈ ℤ, Even(m + n).

In English: there is one fixed integer n that makes m + n even for every integer m. Expanding and then negating gives

¬(∃n ∈ ℤ, ∀m ∈ ℤ, ∃p ∈ ℤ, m + n = 2p)
⇔ ∀n ∈ ℤ, ∃m ∈ ℤ, ∀p ∈ ℤ, m + n ≠ 2p.

**Proof of the negation.** Let n ∈ ℤ. Choose m = n + 1, which is an integer. Let p ∈ ℤ be arbitrary. Then m + n = 2n + 1 is odd, whereas 2p is even. Since no integer is both even and odd, m + n ≠ 2p. Thus every candidate n has a counterexample m, and Statement 2 is false.

The last p remains arbitrary: finding just one p with an unequal value would not establish that the sum is not even. We must exclude every integer p.

## Why the first proof cannot prove the second statement
Choosing n = m works only after m is introduced. Statement 2 requires choosing n before the universal m, so that choice is unavailable. The obstacle is quantifier structure, not simply the fact that m might be even or odd: doubling either kind of integer is even. The proof above uses the fact that even and odd integers are mutually exclusive; the lecture later connects parity classification to the quotient-remainder theorem.

## Using universal statements does not create an element

A **predicate** is a statement depending on inputs, such as Even(n) or IsPrime(n). An **implication** P ⇒ Q says that whenever the hypothesis P holds, the conclusion Q holds. To prove it directly, assume P and establish Q. To use an implication already available, establish its hypothesis before concluding its consequent.

Consider the set

S = {n ∈ ℕ | IsPrime(n) ∧ 2 ∣ n ∧ n > 2}.

Here ∧ means “and,” and d ∣ n means “d divides n”: n = dk for some integer k. S asks for even prime natural numbers greater than 2. It is empty. Indeed, an even n > 2 has the divisor 2, which is neither 1 nor n, so it cannot be prime.

The proposed claim is

(∀n ∈ S, 2 ∣ n) ⇒ (∃n′ ∈ S, 2 ∣ n′).

The mark in n′, read “n prime,” merely creates a different variable name. It does not assert that the variable's value is a prime number.

## The invalid argument
The argument assumes every member of S is even and then says to let n′ be “such an n.” Assuming the hypothesis is the correct opening for an implication proof. The unsupported step is selecting an element: a universal statement does not establish that any element exists.

A universal statement is like a function in this respect: to obtain a particular conclusion, supply a value or available variable of the required domain. Here that requires a member of S, and there is none. The premise is **vacuously true**: a universal statement over an empty set has no counterexample. The existential conclusion is false because it requires a member. Therefore the whole implication is false, even though its hypothesis is true.

## What nonemptiness would change
If the condition n > 2 is removed, the corresponding set is {2}. A valid proof can choose n′ = 2, establish membership by showing 2 is prime and even, and conclude the existential statement. In this version the universal assumption is unnecessary.

More generally, if nonemptiness has been independently established, it can justify introducing an element of the set even if its numerical value is unknown. What matters is a justified source for that element. Merely pointing at ∀n ∈ S does not supply one.

## Moving a domain restriction into an implication
The hypothesis alone can also be written

∀n ∈ ℕ, (IsPrime(n) ∧ 2 ∣ n ∧ n > 2) ⇒ 2 ∣ n.

Now any natural number can be substituted, but the result is an implication, not an unconditional assertion about membership. Substituting 2 gives

(IsPrime(2) ∧ 2 ∣ 2 ∧ 2 > 2) ⇒ 2 ∣ 2.

To obtain its conclusion by using this implication, all three hypothesis conditions would need to hold. The last does not. In fact no natural number satisfies the combined restriction, so these implications are vacuously true because their hypotheses are false. This translates only the original universal hypothesis; it does not turn the full false universal-to-existential implication into a true statement.

## Evaluating arguments: converse, square roots, and contrapositive

The target is

∀n ∈ ℤ, Even(n²) ⇒ Even(n).

Keep track of both the hypothesis being assumed and the conclusion being established. A true conclusion is not enough to make the steps leading to it valid.

## Argument A proves the converse, not the target
Argument A lets n ∈ ℤ, assumes n is even, and writes n = 2k for some k ∈ ℤ. Then

n² = 4k² = 2(2k²).

Since 2k² is an integer, this proves n² is even. The argument correctly proves ∀n ∈ ℤ, Even(n) ⇒ Even(n²).

The **converse** of P ⇒ Q is Q ⇒ P. It exchanges hypothesis and conclusion without negating them. It is not logically equivalent to the original implication: proving one generally does not prove the other. Both happen to be true in this parity example, but Argument A establishes only the converse.

## Argument B aims at the target but has invalid steps
Argument B assumes n² is even and obtains n² = 2k for an integer k. It then writes n = √(2k) = √2√k and claims that a factor of √2 makes n even.

There are two separate issues:
1. **First invalid step:** n = √(2k) loses the negative possibility. The principal square root is nonnegative, so the justified equality is |n| = √(2k), or n = ±√(2k). For example, n = −2 and k = 2 satisfy n² = 2k, but n ≠ √(2k).
2. **The final inference still fails after fixing the sign.** Evenness requires n = 2j for an integer j. An expression containing √2 is not that form, and no integer j has been provided. Merely knowing n is an integer does not repair the missing reasoning.

The square roots also require nonnegative arguments. Here k ≥ 0 follows from k = n²/2, so that concern can be addressed from the assumptions. None of these flaws disproves the target theorem; they show that this proposed proof does not establish it.

## Argument C proves the contrapositive
The **contrapositive** of P ⇒ Q is ¬Q ⇒ ¬P. Unlike the converse, it is logically equivalent to the original implication: both rule out exactly the situation where P is true and Q is false.

For this target the contrapositive is ¬Even(n) ⇒ ¬Even(n²). Using the fact that an integer is odd exactly when it is not even, it is enough to show that odd n has odd square.

Let n ∈ ℤ and suppose n is odd. Then n = 2k + 1 for some k ∈ ℤ. Calculate

n² = (2k + 1)² = 4k² + 4k + 1 = 2(2k² + 2k) + 1.

The quantity 2k² + 2k is an integer, so this matches the definition of oddness. Hence n² is odd, proving the contrapositive and therefore the target.

This uses more than the two separate definitions of even and odd: it relies on **exhaustiveness** (every integer is one of them) and **exclusivity** (no integer is both). The instructor allowed this familiar fact here and explained that the quotient-remainder theorem provides a way to justify it. The distinct definitions alone do not immediately establish that they are logical opposites.

## Waterproof: making proof structure explicit

A **proof assistant**, also called an interactive theorem prover, is software in which proofs are written using a specified language and checked against logical rules. It addresses two difficulties discussed in class: ordinary proofs mix flexible English with mathematics, and students cannot simply run a paper proof to check it.

**Waterproof** is a teaching-oriented proof assistant. Its language resembles paper proofs more closely than many research proof assistants, and automation can supply some routine arithmetic or logical steps. Its goal display shows what remains to be proved after each step. It is especially useful for practicing structure, quantifier order, and how assumptions can be used.

Automation is also a limitation for learning mathematical reasoning. On paper, from x > 0 one can explain that squaring gives x² > 0. Waterproof may accept the conclusion without an explicit squaring step. Acceptance by the tool is not permission to omit crucial reasoning on a paper assessment.

## Demonstration: quantifiers followed by an implication and an existence claim
The example was

∀a ∈ ℝ, ∀b ∈ ℝ, b > 5 ⇒ ∃c ∈ ℝ, c > b − a.

The steps follow the statement's outer structure:
1. Introduce arbitrary a, then arbitrary b. In Waterproof the demonstrated command uses `Take`, rather than ordinary paper wording such as “let.”
2. Assume b > 5, because the remaining goal is an implication.
3. Choose c = b − a + 1, because the remaining goal is existential.
4. Check c ∈ ℝ, then conclude c > b − a: their difference is 1 > 0.

The choice can use both a and b because both are already available. The hypothesis b > 5 is handled as part of the statement even though this particular choice of c works without needing it.

A practice goal displayed ∀x, y ∈ ℝ, x > 3 ⇒ ∃z ∈ ℝ, x < z − y. The demonstration introduced x and y but did not finish this proof. Mathematical symbols could be entered using LaTeX-style commands such as `\reals` and `\in`. In that displayed exercise, trying to introduce z where the next variable was y produced a variable-name error. This is a tool syntax constraint, not a ban on consistently renaming bound variables in mathematics.

## Demonstration: using a universal hypothesis
The next example was

∀x ∈ ℝ, (∀ε ∈ ℝ, ε > 0 ⇒ x < ε) ⇒ x + 1/2 < 1.

Let x ∈ ℝ and assume the entire universal statement, naming it (i). To use it, select ε = 1/2 and establish that 1/2 ∈ ℝ. Instantiation gives

1/2 > 0 ⇒ x < 1/2.

Since 1/2 > 0, the implication yields x < 1/2. Adding 1/2 to both sides gives x + 1/2 < 1. Notice the two different operations: substituting into a universal statement gives an implication; establishing that implication's hypothesis then gives its conclusion.

A further displayed exercise used the same hypothesis with goal 10x < 1. Its solution was not completed in the demonstration. Instead, the instructor showed errors:
- Sentence syntax, punctuation, and the form `Assume that` matter to the parser; naming the hypothesis is allowed.
- Asking the checker to conclude x < 0 failed. Indeed, x = 0 satisfies x < ε for every positive ε but does not satisfy x < 0. A checker failure by itself is not a mathematical disproof; this counterexample explains why that particular inference is false.
- Trying to choose x = 1 while the goal begins ∀x is structurally invalid. An existential goal permits choosing a witness; a universal goal requires handling an arbitrary input.

The tutorial also briefly displayed the reminder that an available conjunction P ∧ Q can be used through either of its two components. The detailed worked examples above, rather than the tutorial's later headings, determine the proof techniques practiced in this lecture.

## From definitions to programs: sums and divisibility

A mathematical definition often suggests a direct implementation, but a theorem may justify a faster one. The key requirement is not that the code resemble the definition: it must produce the same result for every input in its intended domain.

## Summing the first n positive integers
The notation Σᵢ₌₁ⁿ i means 1 + 2 + ⋯ + n. The direct version builds these numbers and adds them:

```python
def sum_to_n_v1(n: int):
    """Return the sum of the numbers from 1 to n, inclusive."""
    return sum([i for i in range(1, n + 1)])


def sum_to_n_v2(n: int):
    """Return the sum of the numbers from 1 to n, inclusive."""
    return n * (n + 1) // 2
```

Python's `range` excludes its stop value, so `n + 1` is needed to include n. The formula Σᵢ₌₁ⁿ i = n(n + 1)/2, stated for positive integers n, connects the implementations. The second does not enumerate all n terms. The product of consecutive integers is even, so integer division `// 2` gives the exact value here. The formula was used as a theorem; its proof was deferred. The lecture did not justify these functions for unrestricted negative inputs merely by annotating n as `int`.

## Divisibility and an existential search
For integers d and n,

d ∣ n ⇔ ∃k ∈ ℤ, n = dk.

Here d is the proposed divisor and k is the integer multiplier witnessing divisibility. Python's `any` corresponds to an existential check over an iterable: it returns true if at least one checked condition is true. But a terminating direct search cannot enumerate all integers. The displayed bounded implementation was:

```python
def divides(d: int, n: int) -> bool:
    """Return whether d divides n."""
    possible_divisors = range(-abs(n), abs(n) + 1)
    return any({n == k * d for k in possible_divisors})
```

Despite the variable name `possible_divisors`, this code searches possible **multipliers k**, not values of the input d. The upper stop includes |n|.

**Mathematical correction to the stated bound.** The unqualified claim “for all integers n and d, d ∣ n implies −|n| ≤ d ≤ |n|” needs n ≠ 0. Every integer divides 0, so d = 1, n = 0 disproves the unqualified version. For n ≠ 0, if n = dk then both d and k are nonzero integers. Thus |k| ≥ 1 and |d| ≥ 1; consequently |d| ≤ |n| and |k| ≤ |n|. This explains the finite search when n is nonzero.

The code still handles n = 0 correctly: its range contains just k = 0, which witnesses 0 = d·0 for every d, including d = 0. For d = 0 and n ≠ 0, no checked multiplier works because 0·k is always 0. These special cases complete the justification that the bounded search matches the existential definition. This is a clarification of the lecture's bound, not an additional theorem attributed to the instructor.

The previously proved positive-integer result, d ∣ n ⇒ d ≤ n for positive d and n, is a related bound. Its positivity conditions matter; they cannot simply be discarded.

## Why property tests are not the whole justification
The class had previously tested properties expected of divisibility. Such results provide evidence that an implementation behaves correctly on tested cases, but do not by themselves prove agreement with the definition on every integer input. A proof must link the actual branches and operations to the mathematical specification.

## Fast divisibility, the quotient-remainder theorem, and parity

The faster implementation uses a separate zero case and Python's remainder operator `%`:

```python
def divides(d: int, n: int) -> bool:
    """Return whether d divides n."""
    if d == 0:
        return n == 0
    else:
        return n % d == 0
```

## Zero divisor branch
By definition, 0 ∣ n means n = 0·k for an integer k. Every such product is 0, so 0 divides only 0. Conversely, k = 0 witnesses 0 ∣ 0. Thus the first branch returns exactly the correct condition and avoids attempting remainder division by zero. In particular, 0 does not divide 1.

## Nonzero divisor branch
For d ≠ 0, correctness requires the equivalence

d ∣ n ⇔ n % d == 0.

An **equivalence**, written ⇔ or “if and only if,” requires both directions: every mathematical divisor must make the code return true, and every true result must represent divisibility. A one-way implication is insufficient.

The **quotient-remainder theorem** states that for all n, d ∈ ℤ with d ≠ 0, there are unique integers q ∈ ℤ and r ∈ ℕ such that

n = qd + r, with 0 ≤ r < |d|.

The quotient q records the multiple of d, while the remainder r records what is left under this convention. “Unique” means exactly one pair satisfies both the equation and the remainder bounds. The lecture assumed the theorem and referred students to the course notes for the divisibility proof.

**Clarifying the connection.** If the theorem's remainder is 0, n = qd directly witnesses d ∣ n. Conversely, if n = kd, the pair (k, 0) satisfies the equation and bounds, so uniqueness forces the theorem's remainder to be 0. For positive d, this is Python's usual remainder convention as well.

**Python sign caveat.** When d is negative, Python's nonzero remainder has the sign of d, whereas the theorem above uses a nonnegative remainder. Do not identify those nonzero remainders without adjustment. The zero-remainder test is nevertheless equivalent to divisibility for either sign of nonzero d. The implementation's validity does not require all remainder conventions to agree on nonzero values.

## How the theorem explains even and odd
Divide any integer n by 2. The theorem gives n = 2q + r with integer r satisfying 0 ≤ r < 2. The only integers in that interval are 0 and 1, so the cases exhaust the possibilities:
- If r = 0, n = 2q, so n is even.
- If r = 1, n = 2q + 1, so n is odd.

They cannot both occur for the same n, since that would provide two permitted quotient-remainder pairs with different remainders, contradicting uniqueness. This explains why every integer is exactly one of even and odd, the fact used in the earlier contrapositive proof. The lecture suggested writing this out as an exercise.

## Prime numbers: definition, logical formula, and direct implementation

An integer p is **prime** when p > 1 and its only natural-number divisors are 1 and p. This gives the predicate

IsPrime(p) : p > 1 ∧ (∀d ∈ ℕ, d ∣ p ⇒ (d = 1 ∨ d = p)), where p ∈ ℤ.

The symbol ∨ means “or.” Expanding divisibility further replaces d ∣ p by ∃k ∈ ℤ, p = kd.

The condition p > 1 rules out 1, 0, and negative integers. The universal implication says: take any natural number d; if it divides p, it must be one of the allowed values. Values that do not divide p are not counterexamples, because their implication hypotheses are false.

## Why the formula uses “or” despite the English “1 and itself”
The English description lists two allowed divisors. For each individual divisor d, the formula requires d = 1 **or** d = p. Requiring both equalities would force that single d to equal two distinct numbers when p > 1. The definition is not merely saying that 1 and p divide p: every p > 1 has those divisors. It excludes all additional natural-number divisors.

## The example 4 is not prime
Choose d = 2. It is a natural number and divides 4, witnessed by 4 = 2·2. But 2 ≠ 1 and 2 ≠ 4, so the consequent d = 1 ∨ d = 4 is false. One divisor therefore falsifies the universal implication. Although 4 > 1, it fails the other required conjunct, so IsPrime(4) is false.

The instructor also suggested trying to prove that 3 is prime as a way to understand the definition; that proof was not worked through in class.

## Direct Python translation
With `divides` already defined, the displayed implementation translates the logic into a finite check:

```python
def is_prime(p: int) -> bool:
    """Return whether p is prime."""
    possible_divisors = range(1, p + 1)
    return (
        p > 1 and
        all({d == 1 or d == p
             for d in possible_divisors if divides(d, p)})
    )
```

`all` corresponds to a universal check: every Boolean in the iterable must be true. The comprehension's `if divides(d, p)` keeps only divisors, matching the restriction expressed by the implication. For p > 1, a positive divisor cannot exceed p, and 0 cannot divide this nonzero p. Hence checking 1 through p covers every possible natural-number divisor. The conjunction with `p > 1` rejects the remaining integer inputs.

This version checks p candidate divisors for positive p. That bound makes the translation possible, but the next theorem allows a substantially smaller search.

## The square-root criterion: proof structure and implementation bugs

## The criterion and its scope
For an integer p with p > 1,

IsPrime(p) ⇔ ∀d ∈ ℕ, 2 ≤ d ≤ √p ⇒ d ∤ p.

Here d ∤ p means d does not divide p. Together with rejecting p ≤ 1, this is an alternative characterization of primality: there must be no divisor from 2 through √p, **including the endpoint**. Establish p > 1 before using the real square root; it is not defined over the reals for negative p.

**Correction to the informal explanation.** It is not true that numbers have no divisors between √p and p. For example, 12 has divisor 6 > √12. The useful claim is that a nontrivial factorization has a small factor: if p = ab with integers a, b ≥ 2, they cannot both exceed √p, since then ab > p. Thus searching the smaller interval is enough to detect that p is not prime; it need not enumerate all divisors.

## Structure of the assigned proof
The lecture outlined, but did not complete, a proof of the full characterization. Introduce an arbitrary integer p and prove both directions of the biconditional:
1. Assume IsPrime(p). Establish p > 1 and show that every d in the interval 2 ≤ d ≤ √p does not divide p.
2. Assume p > 1 and that no d in this interval divides p. Establish IsPrime(p), expanding its definition to show that any natural divisor is 1 or p.

This combines quantifier introductions, use of universal assumptions, proving and using implications, and expansion of divisibility and primality definitions. Proving only the forward direction would not show that the faster implementation never accepts a nonprime input. Try completing the proof before comparing with Section 4.7.

**Additional reading explanation — “4.7 Prime numbers: definition, square-root criterion, and converse proof excerpt.”** The supplied reading explains the key split in the reverse direction. For a natural divisor d₁ of p, the no-small-divisor assumption rules out 2 ≤ d₁ ≤ √p. The remaining possibilities are d₁ < 2 or d₁ > √p; these exhaust the possibilities because they are exactly the complement of that closed interval. In the first case, d₁ is 0 or 1, and 0 is impossible since p > 1. In the second case, expanding d₁ ∣ p supplies an integer k with p = d₁k. Because p and d₁ are positive, k is a positive integer, and d₁ > √p implies k < √p. The same no-small-divisor assumption applies to k, since k also divides p. A positive integer below √p but outside [2, √p] must be 1. This is the important positivity step needed to force d₁ = p. This reading explanation clarifies the assigned proof; it is not a proof completed in the recording.

## Connecting the criterion to the faster code
The displayed candidate range was

```python
possible_divisors = range(2, floor(sqrt(p)) + 1)
```

`floor` rounds down to an integer. The stop value is exclusive, so `+ 1` makes the range include floor(√p), the largest integer that might satisfy d ≤ √p. The subsequent check was `p > 1 and all({not divides(d, p) for d in possible_divisors})`.

**Implementation caveat and editorial repair.** In the displayed code, the range was constructed before that `p > 1` check. Therefore the check cannot protect `sqrt(p)` from a negative input: evaluation of the earlier line has already happened. For the stated all-integer input domain, reject p ≤ 1 first. This repaired version preserves the intended algorithm:

```python
from math import floor, sqrt


def is_prime(p: int) -> bool:
    """Return whether p is prime."""
    if p <= 1:
        return False
    possible_divisors = range(2, floor(sqrt(p)) + 1)
    return all({not divides(d, p) for d in possible_divisors})
```

This explains the algorithmic connection under the intended square-root calculation; the lecture did not analyze floating-point accuracy for arbitrarily large Python integers.

## Why the endpoint matters
The intentionally faulty version omitted `+ 1`:

```python
possible_divisors = range(2, floor(sqrt(p)))  # Wrong!
```

It omits floor(√p). In particular, at a perfect square it omits the endpoint divisor √p. An explanatory counterexample is p = 4: the range is empty, `all` of an empty collection is true, and p > 1 is true, so the faulty version accepts 4 even though 2 divides it. Likewise, p = 9 omits its divisor 3.

The related mathematical claim with 2 ≤ d < √p instead of 2 ≤ d ≤ √p is false as a characterization: 4 has no integer d in the strict interval, yet is not prime. The implication from primality to the strict condition still holds; it is the reverse direction that fails. Also, for nonsquare p the buggy Python range is even narrower than the strict interval: if √p is not an integer, floor(√p) is strictly below √p but is still omitted. The perfect-square counterexample suffices to refute both proposed characterizations. Proof-oriented reasoning thus exposes an incorrect algorithm as well as justifying a correct one.

## Function predicates and the proof of nonnegativity

Consider the function:

```python
def absolute_difference(x: int, y: int) -> int:
    """Return the absolute difference between x and y."""
    if x >= y:
        return x - y
    else:
        return y - x
```

Its intended result is |x − y|. Instead of discussing executions one at a time, describe the relationship between inputs x, y and returned value r using a predicate:

AbsDiff(x, y, r) : (x ≥ y ∧ r = x − y) ∨ (x < y ∧ r = y − x),
where x, y, r ∈ ℤ.

Each part combines a branch condition with the value returned in that branch. The connective between branches is “or” because an execution follows one branch, not both. For integer inputs, exactly one of x ≥ y and x < y holds, so these cases cover every execution. AbsDiff(x, y, r) is true exactly when this function returns r on inputs x and y.

## Worked proof: the output is nonnegative
The statement is

∀x, y, r ∈ ℤ, AbsDiff(x, y, r) ⇒ r ≥ 0.

**Proof.** Let x, y, r ∈ ℤ be arbitrary. Assume AbsDiff(x, y, r). Expanding the assumption gives a disjunction, so consider both cases:
1. Suppose x ≥ y and r = x − y. Since x is at least y, subtracting y gives x − y ≥ 0. Substituting the expression for r yields r ≥ 0.
2. Suppose x < y and r = y − x. Subtracting x from y gives y − x > 0. Hence r > 0, which also implies r ≥ 0.

In every case allowed by AbsDiff, r ≥ 0. Therefore the implication holds for the arbitrary inputs, proving the universal statement.

The order matters: first assume AbsDiff, then split into the cases it supplies. Merely introducing x, y, r does not yet make r a return value. The boundary x = y belongs to the first branch and yields r = 0; the second branch has a strictly positive result.

The worksheet also sets two further exercises: prove, under AbsDiff(x, y, r), that r = 0 ⇔ x = y, separating the two directions; and prove AbsDiff(y, x, r), expressing that swapping inputs leaves the output unchanged. These were not completed in the recorded lecture. The worked nonnegativity proof illustrates that the same logical rules apply to a Python function's behavior as to a purely mathematical statement.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
