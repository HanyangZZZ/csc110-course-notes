# CSC110: Programming and proofs — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Quantifier order and witnesses

10 practice marks

Translate A: ∀m∈ℤ ∃n∈ℤ Even(m+n) and B: ∃n∈ℤ ∀m∈ℤ Even(m+n). Explain witness dependence. Prove A by expanding Even, then negate B and prove that negation.

Answer requirements:

- Introduce variables in quantifier order.
- State the integer domain of each witness.

<details><summary>Hints</summary>

Even(t) means ∃p∈ℤ, t=2p.

For the negation of B, make the sum odd.

</details>

<details><summary>Worked solution and rubric</summary>

A says every m has a suitable n, which may depend on m. B says one fixed n works for all m. For A, let m∈ℤ; choose n=m∈ℤ and p=m∈ℤ. Then m+n=2m=2p. The negation of B is ∀n∈ℤ ∃m∈ℤ ∀p∈ℤ, m+n≠2p. Let n∈ℤ, choose m=n+1∈ℤ, and let p∈ℤ be arbitrary. Then m+n=2n+1 is odd and 2p is even, so they differ. Thus B is false.

- 2 marks: accurate translations and dependence distinction.
- 3 marks: A proof with arbitrary m, both integer witnesses, and equality.
- 2 marks: complete negation including universal p.
- 3 marks: permitted integer witness m=n+1 and argument excluding every p.

</details>

### Universal instantiation and existence

8 practice marks

Let S={n∈ℕ | IsPrime(n) ∧ 2∣n ∧ n>2}. Identify the hypothesis and conclusion of (∀n∈S,2∣n) ⇒ (∃n′∈S,2∣n′). Evaluate the claim and explain the flaw in choosing n′ to be 'such an n' from the hypothesis.

Answer requirements:

- Distinguish the truth of the hypothesis from the truth of the whole implication.

<details><summary>Hints</summary>

Could an even integer greater than 2 be prime?

Does a universal assertion guarantee a member exists?

</details>

<details><summary>Worked solution and rubric</summary>

The hypothesis is that every member of S is even; the conclusion is that some member of S is even. S is empty: any even n>2 has a divisor 2 distinct from 1 and n, so it is not prime. The universal hypothesis is vacuously true, while the existential conclusion is false. Therefore the implication is false. The proposed selection assumes an element exists without justification. Universal instantiation can give a property of an already supplied member, but cannot supply a member of an empty set.

- 2 marks: identify hypothesis and conclusion.
- 2 marks: explain why S is empty using the prime definition.
- 2 marks: correct premise, conclusion, and implication truth values.
- 2 marks: identify the unjustified existence step.

</details>

### Evaluating parity arguments

10 practice marks

The target is ∀n∈ℤ, Even(n²)⇒Even(n). In each argument, k is an integer witness; the witnesses in different arguments are independent. A assumes n=2k and proves n²=2(2k²). B assumes n²=2k, writes n=√(2k), and concludes n is even because the expression has a factor √2. C assumes n=2k+1 and obtains n²=2(2k²+2k)+1. Explain what A proves, two faults in B, and why C proves the target.

Answer requirements:

- Do not infer that the target is false merely because an argument is invalid.

<details><summary>Hints</summary>

Compare converse with contrapositive.

What form must an integer have to be even?

</details>

<details><summary>Worked solution and rubric</summary>

A proves the converse, Even(n)⇒Even(n²), not the target; a converse is not generally equivalent to the original. B first loses the possible negative sign: |n|=√(2k), not necessarily n=√(2k). Also, a factor √2 does not express n as twice an integer; that final inference fails independently of the sign issue. Here k≥0 does follow from n²=2k. C shows odd n has odd square, since 2k²+2k∈ℤ. Assuming every integer is exactly one of even and odd, this is ¬Even(n)⇒¬Even(n²), the contrapositive, which is equivalent to the target.

- 2 marks: identify A's converse and explain non-equivalence.
- 2 marks: identify B's sign error correctly.
- 2 marks: explain why √2 does not meet the evenness definition.
- 2 marks: justify C's odd-square conclusion with an integer witness.
- 2 marks: state contrapositive equivalence and the required parity fact.

</details>

### Function specification and cases

8 practice marks

For integers x,y,r, let AbsDiff(x,y,r) mean (x≥y ∧ r=x−y) ∨ (x<y ∧ r=y−x). Prove ∀x,y,r∈ℤ, AbsDiff(x,y,r)⇒r≥0, including the boundary x=y.

Answer requirements:

- Assume the implication's hypothesis before using it.
- Consider both branches.

<details><summary>Hints</summary>

In the second branch you can prove something stronger than nonnegativity.

</details>

<details><summary>Worked solution and rubric</summary>

Let x,y,r∈ℤ and assume AbsDiff(x,y,r). Its definition gives two cases. If x≥y and r=x−y, then x−y≥0, so r≥0. In particular, x=y gives r=0 in this branch. If x<y and r=y−x, then y−x>0, so r>0 and hence r≥0. These are all cases permitted by the assumption, and each yields the conclusion. Thus the implication holds for all integer x,y,r.

- 2 marks: arbitrary integer variables and explicit AbsDiff assumption.
- 2 marks: first branch, correct inequality, and equality boundary.
- 2 marks: second branch, strict positivity, and implication of nonnegativity.
- 2 marks: explain exhaustive case split and close the universal proof.

</details>

## Review quiz

### Using universal assumptions

8 practice marks

Original self-test, not a reconstruction of Test 1. Prove ∀x∈ℝ, (∀ε∈ℝ, ε>0⇒x<ε)⇒x+1/2<1. Then explain why replacing the conclusion by x<0 would be invalid.

Answer requirements:

- Show both universal instantiation and use of the resulting implication.
- Give a mathematical counterexample, not merely a checker error.

<details><summary>Hints</summary>

Use ε=1/2 for the proof.

Try x=0 for the proposed stronger conclusion.

</details>

<details><summary>Worked solution and rubric</summary>

Let x∈ℝ and assume H: ∀ε∈ℝ, ε>0⇒x<ε. Since 1/2∈ℝ, instantiate H at ε=1/2 to obtain 1/2>0⇒x<1/2. The hypothesis 1/2>0 holds, so x<1/2. Adding 1/2 gives x+1/2<1. The stronger conclusion x<0 fails at x=0: for every positive ε, 0<ε, so H is true, but 0<0 is false.

- 2 marks: introduce arbitrary x and assume H.
- 2 marks: legal instantiation and explicit positive hypothesis.
- 2 marks: derive x<1/2 and the target.
- 2 marks: counterexample x=0 verifying both premise and failed conclusion.

</details>

### Divisibility implementation correctness

10 practice marks

Original self-test. A function returns n==0 when d==0, and n%d==0 otherwise. Using d∣n ⇔ ∃k∈ℤ, n=dk, justify the zero branch. State the equivalence required for the other branch and explain how quotient-remainder connects zero remainder to divisibility. Mention the sign caveat for negative Python divisors.

Answer requirements:

- Keep d≠0 explicit when discussing the remainder branch.
- Do not claim Python's remainder is always nonnegative.

<details><summary>Hints</summary>

For zero divisibility, prove both directions.

The theorem gives a unique pair with 0≤r<|d|.

</details>

<details><summary>Worked solution and rubric</summary>

If 0∣n, then n=0k=0. Conversely, if n=0, k=0 witnesses 0∣n. Thus returning n==0 is correct and avoids modulo by zero. For d≠0 the required equivalence is d∣n iff n%d==0. In the theorem, n=qd+r with 0≤r<|d|. If r=0, q witnesses divisibility. If n=dk, then (k,0) is a permitted quotient-remainder pair, so uniqueness forces r=0. Python's nonzero remainder has the divisor's sign, so for negative d it need not be the theorem's nonnegative remainder. The zero-remainder condition still agrees with divisibility.

- 3 marks: both zero-branch directions and integer witness.
- 1 mark: state the required nonzero-branch equivalence.
- 2 marks: theorem equation, bounds, and nonzero-divisor condition.
- 2 marks: justify both directions of zero remainder versus divisibility.
- 2 marks: correct Python sign distinction without rejecting the zero test.

</details>

### Prime predicates and Python

10 practice marks

Original self-test. Write IsPrime(p) for p∈ℤ using quantifiers and divisibility. Explain why its divisor condition uses implication and 'or'. Prove that 4 is not prime, and justify why a direct implementation can restrict candidate divisors to range(1,p+1) when p>1.

Answer requirements:

- Retain p>1.
- Include a divisibility witness in the counterexample.

<details><summary>Hints</summary>

A single natural divisor violating the allowed alternatives defeats the universal claim.

</details>

<details><summary>Worked solution and rubric</summary>

IsPrime(p) means p>1 ∧ (∀d∈ℕ, d∣p⇒(d=1∨d=p)). The implication restricts the demand to divisors; a nondivisor need not equal 1 or p. The disjunction says each individual divisor may be either allowed value, not both simultaneously. For p=4, d=2 is natural and divides 4 because 4=2·2, but 2 is neither 1 nor 4. Hence 4 fails the universal condition. For p>1, 0 cannot divide p, and every positive divisor is at most p. Thus 1 through p cover all possible natural divisors, and range(1,p+1) includes p because its stop is excluded.

- 3 marks: full predicate with domain, conjunction, implication, and disjunction.
- 2 marks: explain implication and disjunction.
- 3 marks: d=2, divisibility witness, and failed consequent for 4.
- 2 marks: justify the finite range and Python endpoint.

</details>

### Off-by-one errors and proof obligations

10 practice marks

Original self-test. Compare candidate ranges range(2,floor(sqrt(p))+1) and range(2,floor(sqrt(p))) in a prime checker. Give a counterexample to the second, identify which direction of the strict-interval primality equivalence fails, and explain why testing p>1 in a later return statement does not protect an earlier sqrt(p) call.

Answer requirements:

- Distinguish a biconditional from its forward implication.
- Discuss integer inputs p≤1.

<details><summary>Hints</summary>

Use p=4.

Python executes the range assignment before the return statement.

</details>

<details><summary>Worked solution and rubric</summary>

The first range includes floor(√p), while the second omits it. At p=4, the correct range tests 2 and detects 2∣4. The faulty range is empty, so all its nondivisibility checks pass and the checker returns True even though 4 is not prime. In the strict-interval claim, 'p>1 and no divisor with 2≤d<√p implies prime' fails; prime implies no divisor in that interval remains true. A later p>1 check cannot undo a prior sqrt(p) evaluation, which is invalid over the reals for negative p. Return False for p≤1 before constructing the range. For nonsquare p, the buggy range also omits floor(√p) even though that integer is strictly below √p.

- 2 marks: distinguish included and excluded endpoints.
- 3 marks: trace p=4 and explain its false positive.
- 2 marks: identify the failed reverse direction without declaring the forward implication false.
- 2 marks: explain execution order and early domain guard.
- 1 mark: note the nonsquare difference between the buggy range and a strict mathematical interval.

</details>

## Challenge quiz

### Repairing a bound and proving finite search suffices

12 practice marks

The lecture displayed the unqualified bound d∣n⇒|d|≤|n| and searched multipliers k in range(-abs(n),abs(n)+1). Find a counterexample to the bound as stated. Repair the bound, then justify the multiplier search for all integer d,n, including both zero cases.

Answer requirements:

- Do not confuse the divisor input d with the searched multiplier k.
- Separate n=0, n≠0 with d=0, and n≠0 with d≠0.

<details><summary>Hints</summary>

Every integer divides 0.

For a nonzero integer a, |a|≥1.

</details>

<details><summary>Worked solution and rubric</summary>

Take n=0,d=1: 1∣0 but |1|≤|0| is false. A repaired bound assumes n≠0. If n=dk≠0, then d,k are nonzero integers, so |d|≥1 and |k|≥1. From |n|=|d||k|, both |d|≤|n| and |k|≤|n| follow. For the search: if n=0, its range is {0}, and k=0 witnesses 0=d·0 for every d. If n≠0 and d=0, all checks fail, correctly, because 0·k cannot equal n. If n,d are both nonzero and d∣n, an integer witness k satisfies |k|≤|n|, so it lies in the searched interval. Conversely, any successful check supplies an integer k with n=dk, directly proving divisibility. Thus the search is correct despite the unqualified displayed bound needing correction.

- 2 marks: valid zero-target counterexample.
- 2 marks: repaired nonzero-target condition and divisor bound.
- 2 marks: derive the multiplier bound without conflating d and k.
- 2 marks: correct n=0 behavior including d=0.
- 1 mark: correct n≠0,d=0 behavior.
- 2 marks: completeness of the search when n,d are nonzero.
- 1 mark: successful search implies divisibility by definition.

</details>

### Justifying an assumed parity fact

10 practice marks

Assume the quotient-remainder theorem. Prove that every integer is exactly one of even and odd. Explain how this supports replacing ¬Even(n) by Odd(n) in the contrapositive proof of Even(n²)⇒Even(n).

Answer requirements:

- Prove both exhaustiveness and exclusivity.
- Use the stated definitions n=2k and n=2k+1 with integer k.

<details><summary>Hints</summary>

Apply the theorem with d=2.

If both representations existed, what would uniqueness say?

</details>

<details><summary>Worked solution and rubric</summary>

Let n∈ℤ. Since 2≠0, quotient-remainder gives unique q∈ℤ and integer r with n=2q+r and 0≤r<2. The only integers in that interval are 0 and 1. If r=0, n=2q is even; if r=1, n=2q+1 is odd. This proves every integer is at least one. If n were both, there would be integers a,b with n=2a+0=2b+1. Both pairs obey the remainder bounds, but they have different remainders, violating uniqueness. Thus no integer is both. Consequently ¬Even(n)⇔Odd(n), and likewise for the integer n². Therefore proving Odd(n)⇒Odd(n²) is exactly proving the contrapositive of Even(n²)⇒Even(n).

- 2 marks: correct theorem application and domains.
- 2 marks: explain why 0 and 1 exhaust the remainders.
- 2 marks: connect each remainder to the appropriate definition.
- 2 marks: exclusivity using uniqueness.
- 2 marks: explain the logical connection to the contrapositive, including n² being an integer.

</details>

### Analyzing the reverse prime-criterion argument

12 practice marks

Suppose p is an integer with p>1 and no natural d satisfying 2≤d≤√p divides p. Let d₁∈ℕ divide p. Explain why d₁<2 or d₁>√p exhausts the possibilities. In the first case, prove d₁=1, explicitly ruling out 0. In the second case, write p=d₁k with k∈ℤ and justify every step needed to conclude k=1. Explain why positivity is indispensable.

Answer requirements:

- This is an original application of the supplied reading clarification, not a completed in-class solution.
- Establish that k is a natural number before applying the universal assumption to it.

<details><summary>Hints</summary>

A divisor cannot lie in the forbidden closed interval.

Compare k=p/d₁ with p/√p, and remember k is an integer.

</details>

<details><summary>Worked solution and rubric</summary>

Because d₁ divides p, it cannot satisfy 2≤d₁≤√p under the assumption. The complement of this interval is d₁<2 or d₁>√p, so those cases exhaust all possibilities. In the first case d₁ is 0 or 1; 0 cannot divide p>1, so d₁=1. In the second, divisibility supplies k∈ℤ with p=d₁k. Since p>1 and d₁>√p>0, k=p/d₁>0, so k is a positive integer and hence natural. Moreover, k<p/√p=√p. The equation p=kd₁ shows k∣p. Applying the no-small-divisor assumption to this natural divisor excludes 2≤k≤√p. Together with k<√p, this forces k<2. A positive integer below 2 must equal 1. Hence d₁=p. Without positivity, being an integer below 2 would also permit 0 and negative integers, so the final inference would be unjustified.

- 2 marks: derive and explain the exhaustive interval complement.
- 2 marks: handle d₁<2, including exclusion of 0.
- 2 marks: integer companion factor and positivity/natural-domain justification.
- 2 marks: derive k<√p using positive quantities.
- 2 marks: establish k∣p and apply the assumption correctly.
- 2 marks: conclude k=1,d₁=p and explain why positivity is necessary.

</details>

### Biconditional property of a Python function

12 practice marks

Original extension within the lecture's proof methods; this worksheet proof was not completed in class. Prove ∀x,y,r∈ℤ, AbsDiff(x,y,r)⇒(r=0⇔x=y), where AbsDiff(x,y,r) is (x≥y ∧ r=x−y)∨(x<y ∧ r=y−x). Clearly separate the two directions.

Answer requirements:

- Keep AbsDiff as an assumption throughout.
- Prove both directions; do not assume the desired biconditional.

<details><summary>Hints</summary>

For r=0⇒x=y, use the two equalities supplied by the branch cases.

For x=y⇒r=0, determine which branch is possible.

</details>

<details><summary>Worked solution and rubric</summary>

Let x,y,r∈ℤ and assume AbsDiff(x,y,r). Forward: assume r=0. In the first AbsDiff case, r=x−y, so 0=x−y and x=y. In the second case, r=y−x, so 0=y−x also gives x=y; in fact this branch requires x<y and cannot occur with r=0 because y−x>0. Thus r=0 implies x=y. Reverse: assume x=y. The second branch condition x<y is false, so the AbsDiff assumption must hold through the first branch, yielding r=x−y=0. Both directions hold under AbsDiff, so r=0⇔x=y, as required for arbitrary integer x,y,r.

- 2 marks: arbitrary integer variables and explicit AbsDiff assumption.
- 1 mark: clearly label the forward direction and assume r=0.
- 2 marks: first-branch forward calculation.
- 2 marks: second-branch forward reasoning or explanation of impossibility.
- 1 mark: clearly label the reverse direction and assume x=y.
- 2 marks: rule out the second branch and obtain r=x−y=0.
- 2 marks: conclude the biconditional under AbsDiff and the universal statement.

</details>
