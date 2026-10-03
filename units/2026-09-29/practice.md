# CSC110: Introduction to proofs — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Definitions and domains

10 practice marks

Define Even(n), Odd(n), and d | n over the integers. Determine whether 0 | 0, 0 | 7, and −3 | 12 hold, giving a witness or explaining why none exists. Explain why 0 | 0 does not define 0/0.

Answer requirements:

- Use integer witnesses in the definitions.
- Distinguish a predicate from an arithmetic operation.

<details><summary>Hints</summary>

For d | n, ask whether n = dk for some integer k.

Every product 0·k is zero.

</details>

<details><summary>Worked solution and rubric</summary>

Even(n) means ∃k∈ℤ, n=2k; Odd(n) means ∃k∈ℤ, n=2k+1; d|n means ∃k∈ℤ, n=dk, with n,d∈ℤ. We have 0|0, witnessed by k=0. We do not have 0|7 because 0·k=0 for every integer k. We have −3|12, witnessed by k=−4. Divisibility asserts the existence of an integer multiplier and has a Boolean value. Arithmetic 0/0 asks for a division result and is undefined; no division was used to establish 0|0.

- 1 mark: correct Even definition and integer domain.
- 1 mark: correct Odd definition and integer domain.
- 2 marks: correct divisibility definition, argument order, and domains.
- 2 marks: 0|0 with a valid witness.
- 1 mark: 0∤7 with the zero-product reason.
- 1 mark: −3|12 with witness −4.
- 2 marks: clear predicate/arithmetic distinction and no claim that 0/0 is defined.

</details>

### Translation and unfolding

8 practice marks

Translate “For all integers m and n, if both are odd, then mn is odd” using Odd. Then expand every occurrence of Odd without changing the surrounding logical structure. Explain why the assumption witnesses should not be forced to coincide.

Answer requirements:

- This task asks for translation, not a proof that the product is odd.
- Keep each existential quantifier in the predicate occurrence it replaces.

<details><summary>Hints</summary>

The hypothesis is a conjunction.

Use separate names x, y, and z.

</details>

<details><summary>Worked solution and rubric</summary>

The predicate version is ∀m,n∈ℤ, (Odd(m)∧Odd(n))⇒Odd(mn). The expanded version is ∀m,n∈ℤ, [(∃x∈ℤ, m=2x+1)∧(∃y∈ℤ, n=2y+1)]⇒(∃z∈ℤ, mn=2z+1). The two assumptions assert existence separately. For example, m=3 and n=5 require x=1 and y=2, respectively. Requiring a single shared value would exclude these permitted odd inputs and change the hypothesis.

- 2 marks: correct predicate-level quantifiers and implication.
- 1 mark: conjunction of the two oddness assumptions.
- 3 marks: three correct expansions, each with integer domain and appropriate argument.
- 1 mark: preserves the original scopes and structure.
- 1 mark: explains independent witnesses with a valid reason or example.

</details>

### Proof obligations

10 practice marks

Give the required opening and remaining goal for each statement: (a) ∀n∈ℤ, ∃k∈ℤ, n=1·k; (b) ∃n∈ℤ, ∀m∈ℤ, m+n=0; (c) ∀n∈ℤ, n>2⇒n²>4. Complete (a), and explain why merely giving the opening for (b) cannot establish its truth.

Answer requirements:

- Respect the displayed quantifier order.
- Do not choose the n in (b) as a function of m.

<details><summary>Hints</summary>

Treat the outermost operator first.

In (a), n is already available when k is chosen.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Let n∈ℤ be arbitrary. Choose k=n, which is an integer. The remaining goal is n=1·k, and it follows from 1·k=1·n=n. (b) One would have to choose a fixed integer n first, then let m∈ℤ be arbitrary, and prove m+n=0. This cannot be completed: m=0 would force n=0, but m=1 would then fail. An opening is only a structural obligation, not a proof. (c) Let n∈ℤ be arbitrary and assume n>2. The remaining goal is n²>4.

- 2 marks: correct universal-then-existential structure in (a).
- 2 marks: choice k=n, integer membership, and verification.
- 2 marks: correct existential-then-universal structure and remaining goal in (b).
- 2 marks: explains falsity of (b) and why the template is insufficient.
- 2 marks: correct opening and goal in (c).

</details>

### Reading a proof and identifying witnesses

10 practice marks

Read: “Let m,n∈ℤ and assume both are even. Take k₁,k₂∈ℤ with m=2k₁ and n=2k₂. Then m+n=2(k₁+k₂), so m+n is even.” Identify the universal-variable introduction, hypothesis, source of k₁ and k₂, and conclusion witness. Rewrite the ending to follow the instructor's clarity guidance.

Answer requirements:

- Do not assign unsupported numerical values to k₁ and k₂.
- Explicitly state the chosen conclusion witness's domain.

<details><summary>Hints</summary>

Evenness is an existential statement.

The sum of two integers is an integer.

</details>

<details><summary>Worked solution and rubric</summary>

“Let m,n∈ℤ” introduces arbitrary values for the universal quantifiers. The hypothesis is Even(m)∧Even(n), assumed by “assume both are even.” Unfolding those two assumptions guarantees integer witnesses k₁ and k₂; their values are unknown and need not agree. The conclusion witness is k₁+k₂. A clearer ending is: Choose k=k₁+k₂, which is an integer. Then m+n=2k₁+2k₂=2(k₁+k₂)=2k. Hence ∃k∈ℤ, m+n=2k, so m+n is even.

- 1 mark: identifies the universal introduction.
- 2 marks: identifies the conjunction hypothesis and where it is assumed.
- 3 marks: explains extraction from two existential assumptions without choosing values or identifying witnesses.
- 1 mark: identifies k₁+k₂ as the conclusion witness.
- 3 marks: explicit choice, integer membership, and correct verifying calculation.

</details>

## Review quiz

### Original self-test: universal proof and testing

10 practice marks

Prove ∀n∈ℤ, n²≥0 by cases. Explain why checking a million integer inputs would not replace this proof, and why the claim n²>0 for every integer is false.

Answer requirements:

- Original self-test; no real-test format or difficulty calibration is claimed.
- Explain why your cases cover every integer.

<details><summary>Hints</summary>

Use non-negative versus negative.

Multiplication by a negative number reverses an inequality.

</details>

<details><summary>Worked solution and rubric</summary>

Let n∈ℤ be arbitrary. Every integer satisfies either n≥0 or n<0, so these cases exhaust the domain. If n≥0, multiplying 0≤n by non-negative n yields 0≤n². If n<0, multiplying n<0 by negative n reverses the inequality and gives n²>0, hence n²≥0. Both cases establish the goal, so it holds for all integers. A million inputs form a finite subset of the infinite integer domain; they leave other inputs unchecked. Finally, n=0 refutes the stronger strict claim because 0²=0 is not greater than 0.

- 1 mark: arbitrary integer introduction.
- 2 marks: exhaustive cases and explanation.
- 2 marks: non-negative case with valid multiplication reasoning.
- 2 marks: negative case with inequality reversal and connection to ≥0.
- 1 mark: universal conclusion.
- 1 mark: finite-versus-infinite testing explanation.
- 1 mark: zero counterexample to strict positivity.

</details>

### Original self-test: using assumptions

12 practice marks

Assess and repair these arguments. (a) “I know P∨Q, so I choose to assume P and derive R; therefore R.” (b) “I know x>3⇒x²>9, so I assume x>3 and conclude x²>9 for the current x.” (c) “I know ∀x∈S,A(x), so there exists an element a∈S with A(a).” State what extra work or information would make each approach valid.

Answer requirements:

- Original self-test; no real-test calibration is claimed.
- Explain the logical error rather than merely labelling each argument invalid.

<details><summary>Hints</summary>

For (a), Q might be the only true alternative.

For (c), consider S empty.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Invalid: P∨Q does not tell us that P holds. To use cases, derive R from P and also derive R from Q. Then whichever alternative holds supports R. Alternatively, establish P independently. (b) Invalid: assuming an implication's hypothesis is a rule for proving a conditional, not for establishing its conclusion about an already given x. Establish x>3 independently, for example from x=5, then infer x²>9. With x=0 the implication is true but its conclusion is false, so the implication alone is insufficient. (c) Invalid: the universal may be true with S empty. Supply an actual element a with a∈S, or independent information giving such an element; then instantiate the universal to obtain A(a).

- 2 marks: explains why P cannot be selected from an assumed disjunction.
- 2 marks: repairs (a) with both cases or independent evidence of P.
- 2 marks: distinguishes proving from using an implication in (b).
- 2 marks: valid repair and x=0 counterexample to the unsupported inference.
- 2 marks: explains empty-domain possibility in (c).
- 2 marks: requires an independently available domain element before instantiation.

</details>

### Original self-test: direct divisibility proof

10 practice marks

Prove that for all positive integers n and d, d|n implies d≤n. Identify exactly where positivity and the integer domain of the divisibility witness enter the proof.

Answer requirements:

- Original self-test; no real-test calibration is claimed.
- Do not assume the witness is positive without justifying it.

<details><summary>Hints</summary>

From n=dk, write k=n/d.

A positive integer is at least 1.

</details>

<details><summary>Worked solution and rubric</summary>

Let n,d∈ℤ⁺ be arbitrary and assume d|n. Take k∈ℤ with n=dk. Since d>0, division by d is allowed; since n>0 and d>0, k=n/d>0. Since k is an integer, k≥1. Multiplying 1≤k by positive d gives d≤dk. Substituting dk=n yields d≤n. Thus positivity permits division, makes k positive, and preserves the inequality when multiplying by d; integrality strengthens k>0 to k≥1.

- 2 marks: arbitrary positive integers and divisibility assumption.
- 2 marks: integer witness with n=dk.
- 2 marks: justifies k=n/d>0 using both positive inputs.
- 2 marks: k≥1 with explicit integrality reason.
- 2 marks: valid positive multiplication, substitution, and conclusion.

</details>

### Original self-test: extracted and chosen witnesses

10 practice marks

Prove ∀n∈ℤ, (∃r∈ℤ, n=3r+2)⇒(∃s∈ℤ, n²=3s+1). Explain which witness is supplied by an assumption and which you choose.

Answer requirements:

- Original self-test; no real-test calibration is claimed.
- Use distinct names for the two witness roles.
- Check the chosen witness's domain.

<details><summary>Hints</summary>

Expand (3r+2)².

Group the result as three times an integer, plus one.

</details>

<details><summary>Worked solution and rubric</summary>

Let n∈ℤ be arbitrary and assume ∃r∈ℤ, n=3r+2. Take such an integer r. Its existence comes from the assumption; we cannot set it to an arbitrary convenient number. Choose s=3r²+4r+1, which is an integer. Then n²=(3r+2)²=9r²+12r+4=3(3r²+4r+1)+1=3s+1. Hence the required existential conclusion holds. Unlike r, s is our chosen witness, built from an already available value and verified by calculation.

- 2 marks: arbitrary n and correct hypothesis.
- 2 marks: extracts r with its domain/property and explains it is not freely assigned.
- 2 marks: chooses s=3r²+4r+1 and states integer membership.
- 3 marks: correct substitution, expansion, and regrouping.
- 1 mark: explicitly concludes existence and distinguishes the chosen witness.

</details>

## Challenge quiz

### Application: complete a translated statement

12 practice marks

The lecture translated but did not prove the odd-product statement. As an original extension using only its proof rules, prove ∀m,n∈ℤ, (Odd(m)∧Odd(n))⇒Odd(mn). Explain why using one shared assumption witness would lose generality.

Answer requirements:

- This worked answer is an original exercise solution, not a claim about a proof completed in class.
- Use the defining form 2k+1, not an unproved parity multiplication rule.

<details><summary>Hints</summary>

Write m=2x+1 and n=2y+1 separately.

Factor 2 out of every term of the product except the final 1.

</details>

<details><summary>Worked solution and rubric</summary>

Let m,n∈ℤ be arbitrary and assume both are odd. Extract x,y∈ℤ with m=2x+1 and n=2y+1. Choose z=2xy+x+y, which is an integer. Then mn=(2x+1)(2y+1)=4xy+2x+2y+1=2(2xy+x+y)+1=2z+1. Therefore Odd(mn). The two hypotheses guarantee witnesses separately, not a common witness: m=3 and n=5, for example, require x=1 and y=2. Forcing x=y would restrict to equal inputs and would not prove the statement for all odd pairs.

- 2 marks: arbitrary integer inputs and conjunction hypothesis.
- 2 marks: separate extracted integer witnesses.
- 2 marks: correct chosen z and domain check.
- 3 marks: full correct product calculation.
- 1 mark: uses the definition to conclude oddness.
- 2 marks: explains loss of generality from a shared witness with a valid example.

</details>

### Application: prove and disprove quantifier variants

12 practice marks

Prove ∀m∈ℤ, ∃n∈ℤ, m+n=0. Then write and prove the negation of ∃n∈ℤ, ∀m∈ℤ, m+n=0. Your disproof should work for any proposed fixed n, rather than testing only a few choices.

Answer requirements:

- Respect quantifier order in both proofs.
- Treat this as an original completion of the lecture's quantifier-order discussion.

<details><summary>Hints</summary>

For the first statement, choose the additive inverse.

For the negation, make m+n equal a fixed nonzero integer.

</details>

<details><summary>Worked solution and rubric</summary>

For the first statement, let m∈ℤ be arbitrary. Choose n=−m∈ℤ. Then m+n=m−m=0, proving the statement. The negation of ∃n∈ℤ,∀m∈ℤ,m+n=0 is ∀n∈ℤ,∃m∈ℤ,m+n≠0. To prove it, let n∈ℤ be arbitrary and choose m=1−n∈ℤ. Then m+n=(1−n)+n=1≠0. Thus every proposed fixed n fails for at least one integer m. Each chosen witness depends only on a variable introduced before it; that dependency is exactly what the quantifier order permits.

- 2 marks: correct first proof opening and choice n=−m.
- 2 marks: integer membership and equality verification.
- 3 marks: correct negation, including quantifier switches and ≠.
- 2 marks: arbitrary n and choice m=1−n.
- 2 marks: integer membership and nonzero-sum verification.
- 1 mark: explains why the dependency is permitted and rules out every fixed n.

</details>

### Application: proof audit and zero-divisor safety

12 practice marks

A proposed proof of ∀n,d,a∈ℤ, d|n⇒d|an says: “Take n=dk₁. We want an=dk₂, so adk₁=dk₂. Cancel d to get k₂=ak₁. Therefore the theorem is proved.” Identify two distinct justification problems and replace this with a valid proof for the entire stated domain. Explain the d=0 case.

Answer requirements:

- Do not add d≠0 to the theorem.
- Use the actual lecture witness k₂=ak₁.

<details><summary>Hints</summary>

A desired equation is not automatically a known equation.

The final verification need not divide by d.

</details>

<details><summary>Worked solution and rubric</summary>

First, cancelling d requires d≠0, which is absent from the integer domain. Second, working backward from an unproved desired equality only suggests a candidate; the proposed proof has not verified that candidate from the assumptions. A valid proof is: Let n,d,a∈ℤ be arbitrary and assume d|n. Take k₁∈ℤ with n=dk₁. Choose k₂=ak₁∈ℤ. Then an=a(dk₁)=d(ak₁)=dk₂. Hence d|an. If d=0, the assumption forces n=0, so an=0=0·k₂. The same witness and forward verification work without cancellation.

- 2 marks: identifies the unsupported nonzero condition for cancellation.
- 2 marks: distinguishes candidate discovery from proof of the target.
- 2 marks: correct universal opening, hypothesis, and extracted integer witness.
- 2 marks: explicitly chooses k₂=ak₁ and checks integer membership.
- 2 marks: forward verifying equality and divisibility conclusion.
- 2 marks: explains why d=0 is allowed and handled.

</details>

### Application: conditions, counterexamples, and support

12 practice marks

Audit two attempted generalizations of the positive-divisor theorem: (a) ∀n,d∈ℤ, d|n⇒d≤n; (b) for positive real n,d, if n=dk for some positive real k, then d≤n. Give a counterexample to each, verify the hypothesis and failure of the conclusion, and identify which step of the lecture proof is no longer justified.

Answer requirements:

- These are original domain-audit exercises, not claims taught as theorems.
- For (a), retain the integer-witness definition of divisibility.
- For (b), assess the stated multiplication condition rather than redefining the course's divisibility predicate.

<details><summary>Hints</summary>

For (a), every integer divides zero.

For (b), a positive real multiplier can lie strictly between 0 and 1.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Take n=0 and d=2. The hypothesis 2|0 is true with integer witness k=0, but 2≤0 is false. The lecture proof used n>0 and d>0 to infer k=n/d>0. Here n is not positive, and k=0 cannot be strengthened to k≥1. (b) Take n=1, d=2, and k=1/2. All three are positive reals and n=dk because 1=2·(1/2), but 2≤1 is false. In this case k>0 still holds, but k≥1 does not follow because k is not required to be an integer. Each example satisfies its generalized hypothesis and falsifies its conclusion, which is exactly what refutes a universal implication. Neither example refutes the original theorem, whose positive-integer conditions they violate.

- 2 marks: valid counterexample to (a).
- 2 marks: verifies integer divisibility and false inequality in (a).
- 2 marks: identifies failure of the positivity-to-k≥1 reasoning in (a).
- 2 marks: valid positive-real example and multiplication check in (b).
- 2 marks: identifies loss of the integer step k>0⇒k≥1.
- 2 marks: explains counterexample logic and why the original theorem remains unaffected.

</details>
