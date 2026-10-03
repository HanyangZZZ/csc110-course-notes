# CSC110: Introduction to proofs

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Why proofs belong in a programming course

As programs become more complex, correctness becomes harder to judge by inspection. The lecture connected four ways of building confidence:
- **Testing:** run code on inputs and check its results, using the Python console, `doctest`, `pytest`, or property-based tests with `hypothesis`.
- **Code reuse:** reuse a function whose behaviour is already trusted instead of introducing fresh copies that might contain new mistakes.
- **Simplification:** replace complicated code, such as an unnecessarily complicated `if` statement, with equivalent code that is easier to inspect.
- **Proof:** give a logical argument that an implementation satisfies its specification for **all valid inputs**.

The motivating example computes the sum of the first `n` positive integers. The two demonstrated implementations have these return expressions:
```python
# sum_to_n_v1(n: int) -> int
return sum([i for i in range(1, n + 1)])

# sum_to_n_v2(n: int) -> int
return n * (n + 1) // 2
```
For positive integer `n`, `range(1, n + 1)` includes 1 through `n`: Python excludes the stopping value, which is why the stop must be `n + 1`. The first expression directly constructs and sums the required numbers. The second replaces that process with a formula. Its justification is the theorem

**For every n ∈ ℤ⁺, 1 + 2 + ⋯ + n = n(n + 1)/2.**

Here ℤ⁺ means the positive integers. The product of consecutive integers is even, so the exact mathematical quotient is an integer, consistent with the demonstrated `// 2` expression. This arithmetic observation clarifies the code; it does not itself prove the sum formula.

Testing that both versions agree on several inputs does not establish agreement on every positive integer. A proof of the theorem would justify replacing the obvious, or **naive**, summation algorithm with the formula. This illustrates how proofs can unlock simpler or faster algorithms. The theorem was motivation, not a completed proof: the instructor deferred its proof by **induction** to CSC111.

**Reading clarification — 4.5 Justifying Correctness (Beyond Using Test Cases)**: finite testing cannot guarantee correctness over an infinite input domain. This is a limitation of coverage, not a reason to abandon testing; a good test suite can still provide very high confidence.

## Definitions, predicates, and divisibility

A **definition** gives a precise meaning to a term, allowing a larger formula to be treated as one understandable unit. Like a function, it has a domain of permitted inputs and a body describing its meaning. For divisibility, the parts are: let n,d ∈ ℤ (the domain); d divides n (the term); when (the connection); there exists k ∈ ℤ such that n = dk (the body). Here ℤ is the set of all integers, including zero and negative integers.

A **predicate** is a true-or-false statement depending on input values. Turning a definition into a predicate asks whether the definition applies to the supplied values:
- **Even(n)** means ∃k ∈ ℤ, n = 2k, for n ∈ ℤ.
- **Odd(n)** means ∃k ∈ ℤ, n = 2k + 1, for n ∈ ℤ.
- **Divides(d,n)** means ∃k ∈ ℤ, n = dk, for d,n ∈ ℤ.

The symbol ∃ means **there exists**: at least one value in the stated domain makes the following formula true. Thus being even means being twice some integer; being odd means being one more than twice some integer. The multiplier must be an integer, not merely a real number.

We usually write Divides(d,n) as **d | n**. Here d is the divisor and n the number being divided, or dividend. Evenness is exactly 2 | n.

**Divisibility is not division.** The expression d | n has a Boolean truth value. The arithmetic expressions n ÷ d, n/d, and a fraction calculate a number when defined. In particular, the definition of divisibility permits a zero divisor:
- **0 | 0 is true:** 0 = 0·k for an integer k, for example k = 0.
- **Every integer d divides 0:** choose k = 0, so 0 = d·0.
- **0 cannot divide a nonzero integer:** 0·k is always 0, never a nonzero n.
These facts do not make arithmetic division by zero valid. They follow from an equation involving multiplication, not from performing division.

The definition also permits negative divisors and negative multiples. Do not silently replace the integer domain with positive integers; positivity is an extra condition used in a later theorem.

## Expanding definitions without changing logical structure

To **expand** or **unfold** a definition, replace a predicate by its defining formula, substituting the actual arguments in the correct places. Keep the surrounding logical structure intact.

For example, “For every integer x, if x divides 10, then it divides 100” becomes

∀x ∈ ℤ, (x | 10) ⇒ (x | 100),

and then

∀x ∈ ℤ, (∃k₁ ∈ ℤ, 10 = xk₁) ⇒ (∃k₂ ∈ ℤ, 100 = xk₂).

Here ∀ means **for all**, and ⇒ is **implication**: if the left-hand statement holds, the right-hand statement must hold. The two existential variables are independent. Using different names makes it clear that the multiplier yielding 10 need not be the multiplier yielding 100. Reusing `k` in two separate quantified scopes can be valid, much as local variables in different Python scopes can have the same name, but it is easier to misread. Renaming a bound variable consistently within its scope does not change meaning; accidental capture or identification of independent variables would.

### Exercise 1: oddness
The direct predicate is Odd(n) : ∃k ∈ ℤ, n = 2k + 1, with n ∈ ℤ. The class also considered these equivalent integer formulations:
- **2 | (n − 1):** unfolding gives ∃k ∈ ℤ, n − 1 = 2k, which rearranges to n = 2k + 1.
- **2 | (n + 1):** if n = 2k + 1, then n + 1 = 2(k + 1); conversely, n + 1 = 2j gives n = 2(j − 1) + 1. The witness shifts by one, but remains an integer.
- **2 ∤ n:** n is not divisible by 2. This uses the integer parity fact that each integer is exactly one of even or odd. That fact was discussed as familiar, with tools to justify it to come later; it was not proved here.

The second worksheet statement was translated, not proved:

∀m,n ∈ ℤ, (Odd(m) ∧ Odd(n)) ⇒ Odd(mn).

The symbol ∧ means **and**, so both oddness assumptions are required. The notation ∀m,n ∈ ℤ abbreviates two successive universal quantifiers over the same set. Its direct expansion is

∀m,n ∈ ℤ, [(∃x ∈ ℤ, m = 2x + 1) ∧ (∃y ∈ ℤ, n = 2y + 1)] ⇒ (∃z ∈ ℤ, mn = 2z + 1).

The witnesses x and y are separate because m and n need not be the same odd integer. The conclusion has its own witness z. The instructor corrected an attempted expansion that moved quantifiers: for mechanical unfolding, expand each occurrence in place rather than reorganizing the quantifiers. A logically equivalent rewrite, when justified, is a different operation from direct expansion. No odd-product proof was completed during this exercise.

## What a proof is: arbitrary elements, witnesses, and cases

A **proof** is a structured argument establishing why a statement is true. Its steps must follow from assumptions, definitions, known facts, or earlier justified steps. A program is structured text telling a computer what to compute; learning to write proofs similarly involves learning a language and its rules. Unlike Python syntax, ordinary written proofs mix English and mathematical notation and allow many valid presentations. In the paper-proof setting discussed here, there is no ordinary run-and-check interpreter to substitute for checking the argument yourself.

### Universal example: every integer square is non-negative
The statement is ∀n ∈ ℤ, n² ≥ 0. A finite truth-table-style enumeration of integers cannot prove it: there are infinitely many integers, just as infinitely many possible inputs cannot all be covered by finitely many tests.

Instead, let n be an **arbitrary integer**: an unspecified integer, not a specially selected example. The only initial restriction is n ∈ ℤ. Every integer is either non-negative (n ≥ 0) or negative (n < 0), so these cases exhaust all possibilities.
1. If n ≥ 0, multiplying 0 ≤ n by non-negative n preserves the inequality, giving 0 ≤ n².
2. If n < 0, multiplying n < 0 by negative n reverses the inequality, giving n² > 0. A strictly positive number is also ≥ 0, so the required conclusion follows here too.

Both cases reach n² ≥ 0. Because no special integer was chosen and all possibilities were handled, the conclusion holds for every integer. An alternative valid proof could use three exhaustive cases: n > 0, n = 0, and n < 0. Notice the conclusion is **non-negative**, not positive: n = 0 has square 0.

### Existential example: an integer whose square is 16
The statement is ∃n ∈ ℤ, n² = 16. An existential proof needs a **witness**, a particular value that makes the statement true. Choose n = 4. It belongs to ℤ, and n² = 4² = 16. That proves existence; it does not claim uniqueness or require listing every possible solution.

If the witness is not immediately obvious, use **rough work** to work backward from the desired equation and discover a candidate. The finished proof should then choose that candidate, check its domain, and verify the desired property from established facts.

## Choose proof structure from the goal

Before writing a proof, identify the **outermost logical operator**: the quantifier or connective governing the whole statement. Apply its rule first, then inspect the remaining goal. This creates **proof obligations**, the claims still needing justification. A template organizes a proof; it does not make a false claim true.

| Goal | How to proceed, and why |
|---|---|
| ∀x ∈ S, P(x) | Let x be an arbitrary element of S, then prove P(x) without imposing extra restrictions. The reasoning must work for any permitted x. |
| ∃x ∈ S, P(x) | Choose a witness x, check x ∈ S, and prove P(x). One successful value establishes existence. |
| P ⇒ Q | Assume P, the **hypothesis**, and prove Q, the **conclusion**, under that assumption. This establishes the promised conditional relation, not P itself. |
| P ⇔ Q | Prove P ⇒ Q and Q ⇒ P. A **bi-implication**, or “if and only if,” requires both directions; one direction alone is insufficient. |
| P ∧ Q | Prove both P and Q, usually separately. Both claims must be true for the conjunction to hold. |
| P ∨ Q | Prove at least one of P or Q. A **disjunction** is inclusive “or”: one true alternative suffices, and both may be true. |

The conjunction example was Even(4) ∧ IsPrime(4). **Prime** means an integer greater than 1 whose only positive divisors are 1 and itself. Although 4 is even, 4 is not prime because 2 is another positive divisor. Thus the conjunction cannot be proved: proving only its evenness is not enough.

For Even(13) ∨ Odd(13), choose the oddness branch. The witness 6 gives 13 = 2·6 + 1, establishing Odd(13), and therefore the disjunction. There is no need to prove the evenness branch, which is false.

The implication/bi-implication slide also used Even(10) ⇒ 2 | 10 and Odd(11) ⇔ ¬Even(11). The first follows by unfolding evenness; the second requires both directions if proved as a bi-implication. Here ¬ means **not**. These illustrate structures, not an instruction to infer a converse whenever an implication is known.

## Quantifier order controls which choices are allowed

The proof-obligation exercise illustrated why variables must be introduced in the order specified by the statement.

### 1. ∀n ∈ ℤ, ∃k ∈ ℤ, n = 1·k
Start with an arbitrary integer n. Now choose k = n. This is an integer because n is an integer. Finally, 1·k = 1·n = n, as required.

The witness is a specific expression in a previously introduced variable, not necessarily a numerical constant. No single fixed choice such as k = 1 or k = 5 works for every n. Choosing k = n is permitted because n is already available, or “in scope.” As with a Python function parameter, its exact value can be unknown while still being usable.

### 2. ∃n ∈ ℤ, ∀m ∈ ℤ, m + n = 0
The required opening is: choose an integer n first; then let m be an arbitrary integer; then prove m + n = 0. Starting with m and choosing n in response reverses the quantifiers and changes the claim. In this statement, n cannot depend on m because m has not yet been introduced.

The statement is false: one fixed n cannot cancel every integer m. For example, m = 0 would require n = 0, but with that same n, m = 1 would give m + n = 1, not 0. This calculation makes explicit the lecture's reason that no fixed choice works. The false statement still has a recognizable proof template, but its final obligation cannot be completed.

Reversing the quantifiers gives a different, true statement: ∀m ∈ ℤ, ∃n ∈ ℤ, m + n = 0. Here the permitted dependent choice is n = −m, and m + (−m) = 0. The negative sign is essential.

### 3. ∀n ∈ ℤ, n > 2 ⇒ n² > 4
Let n ∈ ℤ and assume n > 2. The remaining goal is n² > 4. Because n > 2 > 0, multiplication preserves the needed inequalities: n² > 2n > 4. Positivity is what makes this reasoning valid; squaring an inequality indiscriminately is not a general rule. Induction is unnecessary here.

**Reading clarification — 4.6, Alternating quantifiers, revisited**: an existentially quantified variable may be assigned a value depending on variables defined before it. It cannot depend on a variable introduced only later. The formal domains in these examples are integers.

## Using assumptions is different from proving goals

A proof accumulates facts: some come from a hypothesis temporarily assumed when proving an implication; others are already established mathematical facts. The rule for **using** a fact is not necessarily the rule for **proving** a statement with the same shape.

### Using a conjunction
From a known P ∧ Q, both P and Q are available, separately and as often as needed. For the example

∀n ∈ ℤ, (Even(n) ∧ IsPrime(n)) ⇒ n = 2,

the opening is: let n be an arbitrary integer and assume Even(n) ∧ IsPrime(n). The proof may then use both evenness and primality. This example illustrated access to the two assumptions; the lecture did not complete the proof that n = 2.

### Using a disjunction
From a known P ∨ Q, you cannot choose whichever alternative is convenient and treat it as established. You know at least one holds, but not which; both could hold. To derive a conclusion R by cases, show that P leads to R and that Q leads to R. These cases cover every possibility allowed by the assumption, including overlap.

For

∀n ∈ ℤ, ((4 | n) ∨ (8 | n)) ⇒ 2 | n,

first let n ∈ ℤ and **assume (4 | n) ∨ (8 | n)**. The instructor explicitly corrected the slide's missing assumption. The case structure is:
- Case 1: assume 4 | n; establish 2 | n.
- Case 2: assume 8 | n; establish 2 | n.

The slide supplied the structure, not the arithmetic details. To see why each branch supports the same conclusion: in Case 1, n = 4r = 2(2r) for some integer r, so 2r is a witness to 2 | n; in Case 2, n = 8s = 2(4s), so 4s is a witness. These are explanatory completions of the displayed obligations. It is harmless that some n satisfy both cases; disjunction does not require mutually exclusive alternatives.

The earlier square proof used this same rule with the known fact n ≥ 0 ∨ n < 0. Those two cases cover every integer and both lead to n² ≥ 0.

**Key distinction:** to prove P ∨ Q, one successful branch suffices. To use P ∨ Q to establish another claim by cases, the desired conclusion must follow whichever branch holds.

## Using existential assumptions and choosing new witnesses

From an assumption ∃x ∈ S, A(x), you may introduce a name, such as x₀, for a value in S satisfying A(x₀). This is **witness extraction**: existence is already known, so you name a value guaranteed to exist. You do not get to assign it an arbitrary convenient numerical value. You know its domain and its stated property, not its exact value. A fresh name helps avoid confusing this witness with other variables.

Contrast this with proving ∃x ∈ S, A(x): there you choose a witness and must verify it works. An existential assumption resembles an “or” over possible values: you know some value works, but are not told which one.

### Worked example: transforming an existential property
Prove

∀n ∈ ℤ, (∃k ∈ ℤ, n = 3k + 2) ⇒ (∃k ∈ ℤ, n² = 3k + 1).

Let n ∈ ℤ and assume the hypothesis. Extract an integer k₀ such that n = 3k₀ + 2. We cannot choose k₀'s value; it must fit the given n. We can, however, use it to build a witness for the conclusion.

Choose **k = 3k₀² + 4k₀ + 1**, an integer. Then

n² = (3k₀ + 2)²
= 9k₀² + 12k₀ + 4
= 3(3k₀² + 4k₀ + 1) + 1
= 3k + 1.

The first equality uses the extracted witness's property. Expanding the square and regrouping isolates a multiple of 3 plus 1, exactly the form needed for the conclusion. Thus the chosen k works. The repeated letter k in the original statement did not require the input and output witnesses to coincide; renaming the first to k₀ makes their different roles explicit.

### Model proof: the sum of two even integers is even
The statement is

∀m,n ∈ ℤ, (Even(m) ∧ Even(n)) ⇒ Even(m + n).

A clear proof is:
1. Let m,n ∈ ℤ be arbitrary, and assume both are even.
2. By the two evenness assumptions, take k₁,k₂ ∈ ℤ with m = 2k₁ and n = 2k₂.
3. Choose **k = k₁ + k₂**, which is an integer.
4. Then m + n = 2k₁ + 2k₂ = 2(k₁ + k₂) = 2k. Therefore Even(m + n).

### Reading the model proof
- “Let m,n ∈ ℤ” introduces the universally quantified variables.
- Assuming m and n are even introduces the implication's hypothesis, a conjunction.
- The names k₁ and k₂ come from the existential statements hidden in Even(m) and Even(n). Their existence is justified by those assumptions, not by guessing numerical values. They need not be equal.
- k₁ + k₂ is the chosen witness for the conclusion ∃k ∈ ℤ, m + n = 2k.

The original model proof left the conclusion's witness implicit in its last calculation. The instructor said it was valid but less ideal to read and mark. For this course, explicitly choose the witness and state that it belongs to the required domain. Here it is enough to state that k₁ + k₂ is an integer; the closure of integers under addition is straightforward.

## Using implications and universal facts

### Using an implication
If P ⇒ Q is already known and P is also established, infer Q. This is different from proving an implication, where assuming P is part of the proof structure. When using an existing implication, you cannot invent its hypothesis as a fact about the current situation.

The lecture example gives x > 3 ⇒ x² > 9 and x = 5. Since 5 > 3, the hypothesis holds, so x² > 9 follows. Without information establishing x > 3, the implication alone does not establish x² > 9. For instance, x could be 0: the implication is still true because its hypothesis is false, while the conclusion 0² > 9 is false. This shows precisely why the extra premise matters.

There is another valid way to use the information: P ⇒ Q is equivalent to ¬P ∨ Q. If useful for a larger proof, reason by cases on ¬P and Q and establish the desired target in both cases. This does not allow concluding Q from the ¬P branch. The instructor emphasized that establishing P and then inferring Q is the more common use.

### Using a universally quantified fact
If ∀x ∈ S, A(x) is known, then A(a) follows for any available a whose membership in S is established. This is **instantiation**: applying a general statement to a particular permitted value. The value can itself be an arbitrary variable already introduced in the proof. Like calling a function, the application must respect its domain.

To prove ∀a,b ∈ ℤ, a² + b² ≥ 0, let a,b ∈ ℤ be arbitrary. Use the earlier theorem ∀n ∈ ℤ, n² ≥ 0 first with n = a to get a² ≥ 0, then with n = b to get b² ≥ 0. Adding these inequalities yields a² + b² ≥ 0 + 0 = 0. One universal fact can be used more than once.

**Existence caveat:** a universal statement does not guarantee that its domain contains an element. If S is empty, ∀x ∈ S, A(x) is true because there are no elements violating it. Therefore you cannot extract a new element of S merely from that universal statement. To instantiate it, you need an available value known to belong to S. This differs from an existential assumption, which does guarantee a witness exists.

## Direct proof: a positive divisor cannot exceed the number

A **direct proof** begins with the given assumptions and reasons forward to the conclusion. The worked theorem was

∀n,d ∈ ℤ⁺, d | n ⇒ d ≤ n.

Both n and d are **positive integers**. Expanding divisibility gives

∀n,d ∈ ℤ⁺, (∃k ∈ ℤ, n = dk) ⇒ d ≤ n.

**Proof.** Let n,d ∈ ℤ⁺ and assume d | n. Extract k ∈ ℤ such that n = dk. We must establish d ≤ n.
1. Since d > 0, it is nonzero, so n = dk gives k = n/d. Since n > 0 and d > 0, this quotient is positive: k > 0.
2. Since k is an integer, k > 0 implies k ≥ 1. This step uses discreteness of the integers: there is no integer strictly between 0 and 1.
3. Multiply 1 ≤ k by the positive number d to obtain d ≤ dk. Multiplication preserves the inequality because d is positive.
4. Substitute dk = n from the assumption, obtaining d ≤ n.

The opening follows mechanically from the universal quantifiers, implication, and existential definition. The remaining reasoning uses positivity and integrality to connect the assumptions to the required inequality. The result is not a theorem about unrestricted integers: the proof's sign conditions are doing real work.

The instructor showed three acceptable presentations of the same reasoning: separate lines, a paragraph, and a table with a conclusion and justification for each step. Line-by-line presentation was preferred for clarity; a paragraph or table can still be valid if it contains the same justified argument.

## Direct proof: divisibility survives multiplication

Exercise 4's completed statement was

∀n,d,a ∈ ℤ, d | n ⇒ d | an.

All three variables range over **all integers**, so zero and negative values are allowed. Expanding both uses of divisibility gives

∀n,d,a ∈ ℤ, (∃k₁ ∈ ℤ, n = dk₁) ⇒ (∃k₂ ∈ ℤ, an = dk₂).

The structural steps are:
1. Let n,d,a ∈ ℤ be arbitrary.
2. Assume ∃k₁ ∈ ℤ, n = dk₁, and name such an integer k₁. Assuming existence and introducing its witness may be combined into one sentence.
3. The remaining goal is existential, so choose a value for k₂ and verify an = dk₂.

The useful choice is **k₂ = ak₁**. It is an integer because a and k₁ are integers. Now reason forward from the known equation:

an = a(dk₁) = d(ak₁) = dk₂.

Thus k₂ witnesses d | an. This works for negative values and for d = 0 as well: the assumption then forces n = 0, and the same calculation remains valid.

### Rough work versus a finished proof
To discover the witness, the instructor worked backward from the desired equation an = dk₂, substituted n = dk₁, and arrived at the candidate k₂ = ak₁. This was marked as rough work, separate from the official proof. The instructor then corrected a presentation that began by treating the target equation as established: in the finished direct proof, start with what is known and end with what must be shown. A derivation from an assumed target does not by itself establish that target.

**Mathematical caution:** cancellation of d in the displayed rough work would require d ≠ 0, which is not assumed here. Do not use that cancellation as a proof step. The candidate k₂ = ak₁ nevertheless works for the full stated domain, as the forward multiplication-and-substitution calculation verifies without division. Working backward can suggest a good candidate even when a particular exploratory manipulation needs an extra condition; the final verification must work under the actual hypotheses.

## Disproving a statement: the assigned follow-up

To **disprove** a statement is to prove its negation. The closing exercise asks students to disprove

∀n,d,a ∈ ℤ, d | an ⇒ ((d | a) ∨ (d | n)).

It claims that whenever an integer divides a product, it must divide at least one factor. The worksheet identifies this claim as false. The instructor assigned this part for homework; no completed disproof was presented in the recording.

**Reading clarification — Exercise 4: Proving and Disproving Divisibility Statements**: first negate the statement, then prove the negation. Disproving a universal statement is often called finding a **counterexample**, a permitted instance for which the asserted property fails. Here failure requires the implication's hypothesis to be true and its conclusion to be false. Since that conclusion is an “or,” both factor-divisibility claims must fail. An example where d does not divide the product would not refute the implication at all.

This explains the task without supplying a lecture solution that was not given. The distinction is important: proving d | n ⇒ d | an in class does not justify reversing or otherwise strengthening the direction of that implication.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
