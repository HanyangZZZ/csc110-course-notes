# CSC110: Propositional logic, negation, and predicates — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Basic operators and inclusive OR

6 practice marks

For each assignment (p,q) = (True,True), (True,False), (False,True), (False,False), evaluate ¬p, p ∧ q, and p ∨ q. Then explain the difference between inclusive and exclusive OR, using one row of your table.

Answer requirements:

- Treat p and q as Boolean variables.

<details><summary>Hints</summary>

AND needs both parts true; OR needs at least one.

Which row distinguishes allowing both from requiring exactly one?

</details>

<details><summary>Worked solution and rubric</summary>

In the requested row order, the triples (¬p, p ∧ q, p ∨ q) are (False,True,True), (False,False,True), (True,False,True), and (True,False,False). Inclusive OR permits one or both true inputs. Exclusive OR permits exactly one. On the True/True row, inclusive OR is True but exclusive OR would be False.

- 1 mark: correct ¬p column.
- 1 mark: correct AND column.
- 1 mark: correct OR column.
- 1 mark: inclusive OR explained as at least one true.
- 1 mark: exclusive OR explained as exactly one true.
- 1 mark: correct True/True comparison.

</details>

### Implication translations and negation scope

8 practice marks

Let p mean all cats are fluffy and q mean all dogs are fluffy. Translate p ⇒ q; identify its hypothesis and conclusion; then give its converse and contrapositive in English. Explain why all dogs are not fluffy is not an acceptable replacement for not all dogs are fluffy.

Answer requirements:

- Keep the scope of all and not explicit.

<details><summary>Hints</summary>

Converse swaps the sides. Contrapositive swaps and negates both whole sides.

Consider a population containing both a fluffy dog and a non-fluffy dog.

</details>

<details><summary>Worked solution and rubric</summary>

The implication is: if all cats are fluffy, then all dogs are fluffy. Its hypothesis is all cats are fluffy; its conclusion is all dogs are fluffy. Its converse is: if all dogs are fluffy, then all cats are fluffy. Its contrapositive is: if not all dogs are fluffy, then not all cats are fluffy. Not all dogs are fluffy requires at least one non-fluffy dog, but allows fluffy dogs too. All dogs are not fluffy, in the classroom interpretation, says no dog is fluffy. A population with one of each makes the first true and the second false, so they are not equivalent.

- 1 mark: original implication translation.
- 1 mark: hypothesis identified.
- 1 mark: conclusion identified.
- 1 mark: converse translation.
- 2 marks: contrapositive has reversed sides and correct whole-statement negations.
- 1 mark: explains at least one versus none.
- 1 mark: mixed-population counterexample with correct outcomes.

</details>

### English, logic, and Python

6 practice marks

Let p mean the program runs and q mean the program passes all tests; Python p and q contain Boolean values. (a) Translate p ∧ ¬q into English and Python. (b) Translate if the program runs, then it passes all tests into logic and Python. (c) Translate not p or not q into logic and English.

Answer requirements:

- Use Boolean expressions, not Python if statements.
- Do not replace does not pass all tests with fails every test.

<details><summary>Hints</summary>

Python implication uses an equivalent OR expression.

OR remains inclusive when its inputs are negated.

</details>

<details><summary>Worked solution and rubric</summary>

(a) The program runs and does not pass all tests; `p and not q`. (b) `p ⇒ q`; `not p or q`. The Python expression is true when p is false, or when its required conclusion q is true. (c) `¬p ∨ ¬q`; the program does not run, or does not pass all tests, or both. Both means that p and q are both false.

- 1 mark: (a) English.
- 1 mark: (a) Python.
- 1 mark: (b) logical notation.
- 1 mark: (b) Python.
- 1 mark: (c) logical notation.
- 1 mark: (c) English preserving inclusive OR and not-all scope.

</details>

### Predicates, domains, and initial quantifier meanings

8 practice marks

Define E(x) to mean x is even, for integer x. Define Q(x,y) to mean x ≤ y, for real x and y. State each predicate's number of inputs and output type; evaluate E(4), E(5), Q(2,2), and Q(3,2). Explain how E differs from a propositional variable p. Finally, translate ∀x ∈ S, E(x) and ∃x ∈ S, E(x), where S is a set of integers.

Answer requirements:

- Use the explicitly defined ≤ comparison.
- Only translate the quantifiers; no quantifier-negation rules are needed.

<details><summary>Hints</summary>

Inputs can be numbers even though outputs are Boolean.

Substitution turns a predicate instance into a proposition.

</details>

<details><summary>Worked solution and rubric</summary>

E has one integer input and a Boolean output. Q has two real-number inputs and a Boolean output. E(4) is True, E(5) is False, Q(2,2) is True because equality satisfies ≤, and Q(3,2) is False. A propositional variable p stands for a whole Boolean-valued proposition; E is a function whose Boolean result depends on its integer input. The universal statement says every element of S is even. The existential statement says at least one element of S is even, not necessarily exactly one.

- 1 mark: E input count/domain and output type.
- 1 mark: Q input count/domain and output type.
- 1 mark: both E evaluations.
- 1 mark: both Q evaluations.
- 2 marks: distinguishes Boolean propositional variable from input-dependent predicate and explains substitution.
- 1 mark: universal translation.
- 1 mark: existential translation.

</details>

## Review quiz

### Vacuous truth, converse, and contrapositive

10 practice marks

Original written self-test. Let p mean a person lives in Toronto and q mean the person lives in Ontario. A student claims p ⇒ q and q ⇒ p are logically equivalent. (a) Give a possible residence situation that disproves the claim, with p, q, and both implication values. (b) Explain why p ⇒ q is true in that situation without establishing p. (c) Give the contrapositive in symbols and English, and explain why it is equivalent to the original.

Answer requirements:

- Do not treat this as a claim about the current assessment format.
- One counterexample is sufficient for part (a), but explain its significance.

<details><summary>Hints</summary>

A person can live in Ontario outside Toronto.

An implication fails only from a true hypothesis to a false conclusion.

</details>

<details><summary>Worked solution and rubric</summary>

(a) A person living in another Ontario city has p=False and q=True. Then p ⇒ q is True and q ⇒ p is False. Because equivalent formulas must agree on every assignment, this one differing case refutes the claim. (b) The original implication is vacuously true: with p false, it places no requirement on q. Truth of the implication does not assert that the person lives in Toronto. (c) The contrapositive is ¬q ⇒ ¬p: if the person does not live in Ontario, then the person does not live in Toronto. It excludes the same forbidden case as the original—living in Toronto without living in Ontario—so it has the same value on every assignment.

- 1 mark: possible Ontario-outside-Toronto situation.
- 1 mark: correct p and q values.
- 2 marks: correct values of both implications.
- 1 mark: explains why one differing assignment disproves equivalence.
- 2 marks: explains vacuous truth and why it does not establish p.
- 1 mark: symbolic contrapositive.
- 1 mark: English contrapositive.
- 1 mark: equivalence explanation based on the shared false case.

</details>

### Verifying logical equivalence

8 practice marks

Original written self-test. Construct truth-table columns for p ⇔ q, ¬p ⇔ ¬q, and (p ∧ q) ∨ (¬p ∧ ¬q), using all four assignments to p and q. State what the table establishes. Explain why the two conjunctions in the third formula cannot both be true, even though its OR is inclusive.

Answer requirements:

- Show all four rows, not just one matching example.

<details><summary>Hints</summary>

The biconditional asks whether two truth values match.

The first conjunction requires p true; the second requires p false.

</details>

<details><summary>Worked solution and rubric</summary>

Using row order (True,True), (True,False), (False,True), (False,False), each of the three columns is True, False, False, True. The formulas therefore agree on every possible assignment and are logically equivalent. The conjunction p ∧ q requires both variables true, while ¬p ∧ ¬q requires both false. No Boolean assignment can satisfy both requirements. Inclusive OR permits both disjuncts to be true when possible, but does not guarantee that their contents make it possible.

- 1 mark: lists all four input assignments.
- 3 marks: one per correct formula column.
- 2 marks: identifies equivalence and justifies it by exhaustive agreement.
- 1 mark: explains incompatible requirements of the conjunctions.
- 1 mark: distinguishes inclusive permission from simultaneous satisfiability.

</details>

### Worked negation derivations

10 practice marks

Original written self-test. Let p mean the program runs and q mean it passes all tests. Derive a fully expanded simplification of (a) ¬(p ⇒ q) and (b) ¬(p ⇔ q). Name or describe the rule at each step and translate both final results into English.

Answer requirements:

- Final formulas may use only p, q, ¬, ∧, ∨, and parentheses.
- Give both branches of the biconditional negation.

<details><summary>Hints</summary>

Rewrite implication as ¬p ∨ q.

A biconditional is the conjunction of two opposite implications.

</details>

<details><summary>Worked solution and rubric</summary>

(a) ¬(p ⇒ q) ≡ ¬(¬p ∨ q), by implication rewriting; ≡ ¬¬p ∧ ¬q, by De Morgan's law; ≡ p ∧ ¬q, by double negation. English: the program runs and does not pass all tests. (b) ¬(p ⇔ q) ≡ ¬((p ⇒ q) ∧ (q ⇒ p)), by biconditional expansion; ≡ ¬(p ⇒ q) ∨ ¬(q ⇒ p), by De Morgan's law; ≡ (p ∧ ¬q) ∨ (q ∧ ¬p), applying part (a) to each direction. English: either the program runs and does not pass all tests, or it passes all tests and does not run. These are exactly the two ways p and q can differ.

- 1 mark: implication rewritten correctly.
- 1 mark: correct De Morgan step in (a).
- 1 mark: correct final (a) after double negation.
- 1 mark: correct English for (a).
- 1 mark: biconditional expanded into two implications.
- 1 mark: correct De Morgan step in (b).
- 2 marks: correct two failure branches in final (b), one mark each.
- 2 marks: complete and correctly grouped English for (b).

</details>

### Inequality complements and tautology

8 practice marks

Original written self-test. For real a, b, c, simplify (a) ¬((a ≤ 1) ∧ (a > 1)) and (b) ¬(a ≥ b ≥ c). Explain why the result of (a) is true for every permitted a, explicitly discussing a = 1. Is the result of (b) also true for every real triple? Justify your answer.

Answer requirements:

- Preserve the real-number domain.
- Show how the chain is expanded.

<details><summary>Hints</summary>

A failed conjunction needs at least one false part.

Try a = b = c when checking whether part (b) is always true.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Applying De Morgan gives ¬(a ≤ 1) ∨ ¬(a > 1), hence (a > 1) ∨ (a ≤ 1). Every real number is either greater than 1 or at most 1. At a=1, the second disjunct is true, so the boundary is included. This is always true on the stated domain, matching the lecture's tautology example. (b) ¬((a ≥ b) ∧ (b ≥ c)) ≡ (a < b) ∨ (b < c). This is not always true: with a=b=c=0, both strict comparisons are false, and the original chain holds.

- 2 marks: correct De Morgan step and final comparisons in (a).
- 1 mark: exhaustive real-number alternatives explained.
- 1 mark: equality boundary handled.
- 1 mark: chain expanded correctly in (b).
- 1 mark: correct OR of strict comparisons.
- 1 mark: valid counterexample to always-true claim.
- 1 mark: explains counterexample values and result.

</details>

## Challenge quiz

### Applying negation rules to compound parts

10 practice marks

Let p mean the program runs, q mean it passes all tests, and r mean the assignment is submitted. Simplify ¬((p ∨ r) ⇒ (q ∧ r)), pushing all negations to individual variables. Translate the result into Python and clearly grouped English. Find one satisfying assignment, and explain why it violates the original implication.

Answer requirements:

- Use only the lecture's propositional operations.
- Final logical formula must have no implication or negation around a compound formula.
- Python p, q, and r are Boolean.

<details><summary>Hints</summary>

Treat p ∨ r as one hypothesis and q ∧ r as one conclusion.

The negation requires the hypothesis to be true and the conclusion false.

</details>

<details><summary>Worked solution and rubric</summary>

Let H=(p ∨ r) and C=(q ∧ r). Then ¬(H ⇒ C) ≡ H ∧ ¬C gives (p ∨ r) ∧ ¬(q ∧ r), and De Morgan gives (p ∨ r) ∧ (¬q ∨ ¬r). Python: `(p or r) and (not q or not r)`. English: at least one of the program running and the assignment being submitted holds, and at least one of passing all tests and submitting the assignment fails. For example, p=True, q=True, r=False satisfies both disjunctions. The original hypothesis is true because p is true, but its conclusion q ∧ r is false because r is false, so the implication is violated.

- 2 marks: identifies whole H and C and applies H ∧ ¬C.
- 2 marks: correct De Morgan result with preserved grouping.
- 2 marks: correct Python translation.
- 2 marks: English preserves both grouped inclusive disjunctions and their conjunction.
- 1 mark: satisfying assignment.
- 1 mark: explains original true-hypothesis/false-conclusion failure.

</details>

### Compound biconditional and a scope error

10 practice marks

Let F = ¬((p ∧ q) ⇔ r). A student proposes G = ((p ∧ q) ∧ ¬r) ∨ ((¬p ∧ q) ∧ r). Give the correct fully expanded form of F. Explain the student's scope error and provide a truth assignment on which F and G differ, evaluating both. Then translate your correct result using p = the program runs, q = it passes all tests, r = the assignment is submitted.

Answer requirements:

- Negations in the final formula must apply only to individual variables.
- A counterexample must include both formula values.

<details><summary>Hints</summary>

The second branch requires ¬(p ∧ q), not ¬p ∧ q.

Try making q false while r is true.

</details>

<details><summary>Worked solution and rubric</summary>

The correct form is ((p ∧ q) ∧ ¬r) ∨ ((¬p ∨ ¬q) ∧ r). The second branch needs the negation of the entire left side of the biconditional. De Morgan turns ¬(p ∧ q) into ¬p ∨ ¬q; negating just p while still requiring q misses cases where q is false. Take p=True, q=False, r=True. Then p ∧ q is False, which differs from r, so F=True. In G, the first branch is false because p ∧ q is false, and the second is false because ¬p ∧ q is false. Thus G=False. English: either the program runs and passes all tests while the assignment is not submitted, or the assignment is submitted and the program does not run or does not pass all tests, or both of those failures occur.

- 2 marks: correct expanded formula.
- 2 marks: explains whole-conjunction negation and the required OR.
- 1 mark: valid counterexample assignment.
- 1 mark: correct evaluation of F.
- 1 mark: evaluates both G branches and concludes G=False.
- 1 mark: connects differing values to non-equivalence.
- 2 marks: complete English translation preserving both branches and inner OR.

</details>

### Evaluating the support supplied by an implication

10 practice marks

Let p mean a program runs and q mean it passes all tests. Suppose the condition p ⇒ q is true. (a) If you additionally know ¬q, must ¬p hold? Explain by ruling out the other Boolean possibility for p. (b) If instead you additionally know q, must p hold? Give a counterexample. (c) What stronger single connective between p and q would support both directions, and what Python Boolean expression represents it?

Answer requirements:

- Use only the supplied truth conditions; do not assume extra facts about actual programs.
- Explain why the alternatives considered exhaust the Boolean possibilities.

<details><summary>Hints</summary>

For (a), ask whether p=True could coexist with q=False and a true implication.

For (b), the vacuous truth rows matter.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Yes. Given ¬q, q=False. If p were True, p ⇒ q would be False, contradicting the supplied true condition. Because a Boolean p is either True or False and True has been ruled out, p must be False, so ¬p holds. This is the contrapositive direction. (b) No. The assignment p=False, q=True makes both q and p ⇒ q true while p is false. Knowing the conclusion does not establish the hypothesis; doing so would require the converse. (c) The biconditional p ⇔ q requires both p ⇒ q and q ⇒ p. For Boolean variables, Python represents it as `p == q`. It rules out both unequal-value cases, including the counterexample in (b).

- 1 mark: correct yes answer in (a).
- 2 marks: shows p=True would violate the supplied implication.
- 1 mark: explains exhaustive Boolean alternatives and concludes p=False.
- 1 mark: correct no answer in (b).
- 2 marks: counterexample with all relevant values explained.
- 1 mark: identifies unsupported converse direction.
- 1 mark: correct biconditional and two-direction meaning.
- 1 mark: correct Python expression.

</details>

### Mathematical equivalence versus requested answer form

10 practice marks

A student answers a request to expand ¬(p ⇔ q) with p ⇔ ¬q. Determine whether these two formulas are mathematically equivalent by considering the equal-value and unequal-value cases. Explain why the answer still does not meet a request to expand the biconditional and push negations to individual variables. Supply the requested final form, its Python translation, and one assignment showing that p ⇔ q itself is not equivalent to p ⇔ ¬q.

Answer requirements:

- Distinguish ¬(p ⇔ q) from the unnegated p ⇔ q.
- Use Boolean p and q for Python.
- Do not stop at a biconditional in the requested expanded answer.

<details><summary>Hints</summary>

If p and q differ, p agrees with ¬q.

An equivalent answer can still fail an explicit answer-form requirement.

</details>

<details><summary>Worked solution and rubric</summary>

They are equivalent. If p and q have equal values, p ⇔ q is true and its negation is false; p disagrees with ¬q, so p ⇔ ¬q is also false. If p and q have unequal values, ¬(p ⇔ q) is true and p agrees with ¬q, so p ⇔ ¬q is true. Equal or unequal exhausts the possibilities. However, p ⇔ ¬q still contains a biconditional and therefore does not satisfy the request to expand it. The requested form is (p ∧ ¬q) ∨ (q ∧ ¬p), represented by `(p and not q) or (q and not p)`. For p=True and q=True, the unnegated p ⇔ q is True while p ⇔ ¬q is False, showing that those two unnegated-side comparisons are not equivalent.

- 2 marks: correct equal-value case analysis.
- 2 marks: correct unequal-value case analysis.
- 1 mark: explains exhaustive cases and concludes equivalence.
- 1 mark: distinguishes equivalence from compliance with expansion requirement.
- 1 mark: correct expanded logical formula.
- 1 mark: correct Python expression.
- 2 marks: valid assignment and both values demonstrating non-equivalence of p ⇔ q and p ⇔ ¬q.

</details>
