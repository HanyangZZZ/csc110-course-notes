# CSC110: Propositional logic, negation, and predicates

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Why study formal logic in computer science?

A **Boolean expression** represents a condition whose value is True or False. **Formal logic** is a mathematical language for writing such conditions precisely. This lecture begins Chapter 3 and connects three ways of expressing the same condition: English, mathematical notation, and Python.

Logic has two main uses in this course:
1. **Writing precise program conditions.** An English requirement can become ambiguous when it combines several conditions. Expressing its logical structure first helps us translate it into the right Boolean expression in a program. Negating conditions will also be useful when working with conditional statements and loops.
2. **Expressing program correctness.** A claim that a statement, function, or program behaves correctly can itself be expressed logically. Later, formal proofs will be used to establish such claims; this lecture introduces the language, not those proof methods.

Examples include the true arithmetic statement `3 + 2 = 5`, the claim that Python's `sorted` function is correct, and the conditional claim that a program is correct assuming `sorted` is correct. The motivation slide also mentions sets being more efficient than lists for certain operations. This is a qualified example of a claim, not a claim that sets are faster for every operation.

**Reading clarification — 3.1 Propositional Logic.** The official notes explain why even a simple output type such as `bool` needs careful study: the definitions and conditions being represented can be complex, even though the final expression has only two possible truth values.

## Propositions, formulas, and the basic operators

A **proposition** is a statement that is either True or False. A **propositional variable**, such as `p` or `q`, stands for a whole proposition and can take only a Boolean value. A **propositional operator**, **logical operator**, or **logical connective** combines or modifies propositions; these names refer to the same idea. A **propositional formula** is an expression built from propositional variables and these operators.

For example, `3 + 2 = 5` is a proposition, and it is True. The statement that Python's `sorted` function is correct is also being treated as a proposition. A proposition need not be a simple arithmetic equality.

The first three operators are:
- **Negation**, written `¬p`, means that `p` is not true. It flips the truth value. Python: `not p`.
- **Conjunction**, written `p ∧ q`, means `p` and `q`. It is True only when both parts are True. Python: `p and q`.
- **Disjunction**, written `p ∨ q`, means `p` or `q`, or both. It is True when at least one part is True. Python: `p or q`.

A **truth table** lists the result for every possible assignment of truth values to the input variables. Two Boolean variables have four assignments: each of the two choices for `p` can be paired with each of the two choices for `q`.

| p | q | ¬p | p ∨ q | p ∧ q |
|---|---|---|---|---|
| True | True | False | True | True |
| True | False | False | True | False |
| False | True | True | True | False |
| False | False | True | False | False |

This table makes the difference between conjunction and disjunction explicit: one false part is enough to make an AND false, but an OR becomes false only when both parts are false.

**Lecture translation example.** Let `p` mean dogs are cute and `q` mean cats are cute. Then `¬p` means dogs are not cute; `p ∨ q` means dogs are cute or cats are cute, or both; and `p ∧ q` means dogs are cute and cats are cute.

Logical OR is **inclusive OR**: both parts may be true. **Exclusive OR** would instead require exactly one true part. Do not silently interpret `∨` or Boolean Python `or` as exclusive.

## Implication: conditions, vacuous truth, and causation

An **implication**, also called a **conditional**, has the form `p ⇒ q`: if `p`, then `q`. Its left-hand part, `p`, is the **hypothesis**; its right-hand part, `q`, is the **conclusion**. It says that whenever the hypothesis is true, the conclusion must be true too. It does not assert that the hypothesis actually is true.

For the program example, the condition that Python's `sorted` function is correct is the hypothesis, and the claim that my program is correct is the conclusion. The English phrase *my program is correct, assuming Python's sorted function is correct* therefore puts the assumption on the left of the arrow. With `p` meaning dogs are cute and `q` meaning cats are cute, `p ⇒ q` means if dogs are cute, then cats are cute.

| p | q | p ⇒ q |
|---|---|---|
| True | True | True |
| True | False | False |
| False | True | True |
| False | False | True |

The only false case is a true hypothesis with a false conclusion: this is exactly the situation that breaks the condition. When `p` is false, the implication is true regardless of `q`. These two cases are called **vacuous truth** cases. The implication places a requirement on what happens when `p` is true, so a case in which `p` is false does not violate it.

**Toronto–Ontario example.** Let `p` mean you live in Toronto and `q` mean you live in Ontario. If you live in Toronto, you live in Ontario. If you do not live in Toronto, you could live elsewhere in Ontario or outside Ontario altogether. Both are consistent with the implication. Thus, from a false hypothesis and this conditional alone, you cannot determine the conclusion's truth value.

The lecture also gives a **subset** interpretation: a subset is a collection entirely contained in another collection. Saying that everything with property P also has property Q describes the P collection as contained in the Q collection. An object outside P may still lie inside Q, or outside Q; the containment claim does not choose between those cases.

**Implication is not necessarily causation.** The example *if it is raining outside right now, then Python is a programming language* does not say that rain created Python. Under the lecture's stated assumption that it was not raining, the implication is vacuously true. Moreover, its conclusion is true, so the truth table makes this implication true whether or not it rains. A true logical conditional alone does not establish a causal connection.

## Logical equivalence, the contrapositive, and the converse

Two formulas are **logically equivalent** when they have the same truth value for every assignment to their variables. Agreement on just one assignment is not enough. We write `≡` in these notes to mean logical equivalence.

An implication has two particularly useful equivalent forms:

`p ⇒ q ≡ ¬p ∨ q ≡ ¬q ⇒ ¬p`.

The first equivalence follows directly from the two cases for `p`. If `p` is false, `¬p` is true, so `¬p ∨ q` is true just as the implication is. If `p` is true, `¬p` is false, so the disjunction has exactly the value of `q`, again matching the implication. These two cases exhaust the possibilities because `p` is Boolean.

For the Toronto example, `¬p ∨ q` says you do not live in Toronto, or you live in Ontario, or both.

The **contrapositive** of `p ⇒ q` is `¬q ⇒ ¬p`: reverse the two sides and negate both. If living in Toronto requires living in Ontario, then not living in Ontario rules out living in Toronto. Otherwise there would be a true hypothesis and a false conclusion in the original implication. Thus the contrapositive says: if you do not live in Ontario, then you do not live in Toronto. It is always logically equivalent to the original implication, a fact the instructor plans to use in later proofs.

The **converse** is `q ⇒ p`: reverse the sides without negating them. It is not generally equivalent to the original. Living in Ontario does not require living in Toronto; someone living in another Ontario city provides a case with `p = False` and `q = True`. The original implication is then True but its converse is False.

To **verify equivalence**, construct the truth-table column for each formula and compare all rows. To **disprove equivalence**, one row on which the columns differ is enough. Such an assignment is a **counterexample** to the claim of equivalence. The Toronto example shows that the converse need not follow; it does not say that an implication and its converse can never both be true.

## Biconditionals and the equivalence poll

A **biconditional**, also called a **bi-implication**, is written `p ⇔ q` and read *p if and only if q*. It requires both directions:

`p ⇔ q ≡ (p ⇒ q) ∧ (q ⇒ p)`.

For dogs and cats, it says dogs are cute if and only if cats are cute: dogs being cute implies cats being cute, and cats being cute implies dogs being cute. This is stronger than requiring just one direction.

A biconditional is True exactly when its two sides have the same truth value:

| p | q | p ⇔ q |
|---|---|---|
| True | True | True |
| True | False | False |
| False | True | False |
| False | False | True |

When both sides are true, both implications hold. When both sides are false, both implications hold vacuously. When the sides differ, the direction from the true side to the false side fails, so their conjunction fails.

**Worked poll reasoning.** The following forms were established as equivalent to `p ⇔ q`:
- `q ⇔ p`: swapping the sides does not change whether their values match.
- `¬p ⇔ ¬q`: flipping both values preserves agreement or disagreement. True/True becomes False/False, and False/False becomes True/True; the two unequal assignments remain unequal.
- `(p ∧ q) ∨ (¬p ∧ ¬q)`: the first conjunction covers exactly the both-true case, and the second covers exactly the both-false case. Those are precisely the two ways Boolean values can match. The two conjunctions cannot both be true on one assignment, even though the connecting OR is inclusive.

The final poll option, `(p ∨ q) ∧ (¬p ∨ ¬q)`, is **not** equivalent to `p ⇔ q`. With `p = True` and `q = False`, both disjunctions are True, so their conjunction is True; the biconditional is False because the inputs differ. One differing truth-table row is enough to disprove equivalence. In fact, this option is True exactly when one input is True and the other is False.

## Translating between English, mathematical logic, and Python

Translate the logical structure, not merely individual words. First decide what each propositional variable stands for, then identify the operator connecting the whole statements. For the following Python translations, `p` and `q` are Boolean variables.

| Logical form | English structure | Python expression |
|---|---|---|
| `¬p` | not p | `not p` |
| `p ∧ q` | p and q | `p and q` |
| `p ∨ q` | p or q, or both | `p or q` |
| `p ⇒ q` | if p, then q | `not p or q` |
| `p ⇔ q` | p if and only if q | `p == q` |

Python has no separate implication operator. Use `not p or q`, justified by the logical equivalence established earlier. For a biconditional, `==` compares the two Boolean values and is true exactly when they match. This correspondence is about Boolean inputs, not an unrestricted claim about every possible Python object.

**Worksheet Exercise 1.1: cats and dogs.** Let `p` mean all cats are fluffy and `q` mean all dogs are fluffy.
- `p ⇒ q`: if all cats are fluffy, then all dogs are fluffy.
- Hypothesis: all cats are fluffy.
- Conclusion: all dogs are fluffy.
- Converse, `q ⇒ p`: if all dogs are fluffy, then all cats are fluffy.
- Contrapositive, `¬q ⇒ ¬p`: if not all dogs are fluffy, then not all cats are fluffy.

The negation applies to each entire proposition. **Not all dogs are fluffy** means at least one dog is not fluffy; it leaves open whether other dogs are fluffy. **All dogs are not fluffy**, in the interpretation used in class, means no dog is fluffy. In an ordinary nonempty dog population, that is stronger. A population with one fluffy dog and one non-fluffy dog makes *not all are fluffy* true but *none are fluffy* false, demonstrating that they are not interchangeable. The instructor accepted *there is at least one dog that is not fluffy* as a translation of `¬q`, recommended literal translations for now, and explicitly warned that replacing *not all* with *all are not* would lose marks on a test or exam.

**Worksheet Exercise 1.2: program conditions.** Now redefine `p` as the program runs and `q` as the program passes all tests.
1. `p ∧ ¬q` translates to *the program runs and does not pass all tests*, and to `p and not q`. Not passing all tests does not mean failing every test.
2. *If the program runs, then it passes all tests* translates to `p ⇒ q`, and to `not p or q`. It is not a conjunction asserting both facts; when the program does not run, this implication is vacuously true.
3. `not p or not q` translates to `¬p ∨ ¬q`: *the program does not run, or it does not pass all tests, or both*. Here *both* refers to the two negated statements. Thus both disjuncts are true when `p` and `q` are both false, not when they are both true.

## Negation: double negation and De Morgan's laws

To negate a formula, put `¬` in front of the entire formula, using parentheses to show its scope. To simplify it, move the negation inward one operator at a time while preserving its truth value. The goal in the worksheet is to push negations as far inside as possible, rather than leave a negation around a large compound statement.

**Double negation:** `¬(¬p) ≡ p`. Negation flips a Boolean value, so applying it twice restores the original value: True becomes False then True; False becomes True then False. In the lecture's weather example, the opposite of *it is not hot outside* is *it is not not hot outside*, or simply *it is hot outside*. Keeping the same underlying property avoids replacing a logical negation with an imprecise everyday opposite such as cold.

**Negating a conjunction:** `¬(p ∧ q) ≡ ¬p ∨ ¬q`. To deny that both parts are true, at least one part must be false. There is no need for both to be false, though that is allowed.

Lecture example: the negation of *you are a U of T student and you are enrolled in the CS program* is *you are not a U of T student, or you are not enrolled in the CS program, or both*.

**Negating a disjunction:** `¬(p ∨ q) ≡ ¬p ∧ ¬q`. An inclusive OR is true if either part is true, so to make it false, both parts must be false.

Lecture example: the negation of *you are enrolled in CSC110 or you are enrolled in MAT148, or both* is *you are not enrolled in CSC110 and you are not enrolled in MAT148*.

**Reading clarification — Manipulating negation.** The official notes call the AND/OR rules **De Morgan's laws**. In both rules, moving a negation inside requires two changes together: negate each part and exchange AND with OR. Negating the parts without changing the connective generally changes the meaning. The rules apply when `p` and `q` stand for compound formulas as well as single variables.

## Negating implications and biconditionals

**Worksheet Exercise 2.1: negate an implication.** Rewrite the implication first, then apply the rules for OR and double negation:

`¬(p ⇒ q)`  
`≡ ¬(¬p ∨ q)` — rewrite the implication  
`≡ ¬(¬p) ∧ ¬q` — negate both disjuncts and change OR to AND  
`≡ p ∧ ¬q` — remove double negation.

This result describes exactly the implication's one false case: its hypothesis is true but its conclusion is false. For `p` meaning the program runs and `q` meaning the program passes all tests, the English result is *the program runs and does not pass all tests*.

The pattern works for any hypothesis `H` and conclusion `C`, including compound formulas:

`¬(H ⇒ C) ≡ H ∧ ¬C`.

The instructor explicitly permits reusing this established equivalence in more complex exercises, assignments, and tests. For the extra example `¬((p ∧ r) ⇒ q)`, the whole hypothesis is `p ∧ r`, so the result is `(p ∧ r) ∧ ¬q`. Do not negate the hypothesis: a failed implication requires the hypothesis to hold.

**Worksheet Exercise 2.2: negate a biconditional.** Expand it into two implications:

`¬(p ⇔ q)`  
`≡ ¬((p ⇒ q) ∧ (q ⇒ p))`  
`≡ ¬(p ⇒ q) ∨ ¬(q ⇒ p)` — a conjunction fails if at least one direction fails  
`≡ (p ∧ ¬q) ∨ (q ∧ ¬p)` — apply the implication-negation rule to each direction.

This is true exactly when the two Boolean values differ. There are only two ways to differ: `p` true with `q` false, or `q` true with `p` false. For the program example: *either the program runs and does not pass all tests, or the program passes all tests and does not run*. Both branches must appear in the English translation to capture the whole formula. Whether both cases seem likely for an actual program does not change the propositional formula unless extra assumptions are supplied.

**Clarification of the classroom question.** A student suggested a form involving `p ⇔ ¬q`. Mathematically, that form is equivalent to `¬(p ⇔ q)`: `p` agrees with the opposite of `q` exactly when `p` and `q` differ. But it is not the requested expanded answer, because it retains a biconditional instead of pushing the negation down to the individual variables using AND and OR. Also, the unnegated `p ⇔ q` is not equivalent to `p ⇔ ¬q`; they have opposite truth values. Keep these two comparisons distinct.

## Compound hypotheses and conclusions: three-variable examples

For Worksheet Exercise 2.3, let `p` mean the program runs, `q` mean the program passes all tests, and `r` mean the assignment is submitted. First identify the whole hypothesis and whole conclusion, or the two whole sides of a biconditional. Apply the known rule to those complete parts, then simplify any remaining inner negations.

**(a) Negating `p ⇒ (q ∧ r)`.**

`¬(p ⇒ (q ∧ r)) ≡ p ∧ ¬(q ∧ r) ≡ p ∧ (¬q ∨ ¬r)`.

Here the conclusion requires both passing all tests and submitting the assignment. To violate it while the hypothesis holds, the program must run and at least one of those two requirements must fail.

English: *the program runs, and either it does not pass all tests or the assignment is not submitted, or both*. Parentheses matter: running is required in every case described by this negation.

**(b) Negating `(p ∧ q) ⇒ r`.**

`¬((p ∧ q) ⇒ r) ≡ (p ∧ q) ∧ ¬r`.

Now running and passing all tests are both in the hypothesis. A violation therefore requires both of them to hold while submission fails.

English: *the program runs and passes all tests, and the assignment is not submitted*. The instructor left this English translation for students to finish and compare with the sample solutions. Unlike part (a), not passing all tests cannot satisfy this negation, because it would make the hypothesis false.

**(c) Negating `(p ∧ q) ⇔ r`.**

`¬((p ∧ q) ⇔ r)`  
`≡ ((p ∧ q) ∧ ¬r) ∨ (¬(p ∧ q) ∧ r)`  
`≡ ((p ∧ q) ∧ ¬r) ∨ ((¬p ∨ ¬q) ∧ r)`.

The two sides of the biconditional must differ. Either the program both runs and passes all tests while the assignment is not submitted, or the assignment is submitted while the combined condition *runs and passes all tests* is false. In the second case, failure of that conjunction means the program does not run or does not pass all tests, or both.

English: *either the program runs and passes all tests but the assignment is not submitted, or the assignment is submitted and the program does not run or does not pass all tests (or both)*. Here *but* has the same logical role as AND; the wording contrasts the facts without changing their truth conditions.

The negation in the second branch must cover the **whole** `p ∧ q`, giving `¬p ∨ ¬q`, not just `¬p ∧ q`. This is the scope correction made during the worked solution.

## Negating inequalities and recognizing a tautology

A chained inequality abbreviates a conjunction of adjacent comparisons. For example,

`x < y < z ≡ (x < y) ∧ (y < z)`.

The same breakdown applies to chains using `≤`, `>`, or `≥`. A chain asserts that every adjacent comparison holds, so its negation says that at least one of them fails.

For real numbers, negating an order comparison gives its complementary comparison:

| Comparison | Negation |
|---|---|
| `a < b` | `a ≥ b` |
| `a ≤ b` | `a > b` |
| `a > b` | `a ≤ b` |
| `a ≥ b` | `a < b` |

Equality must be placed on the correct side. For example, if `a` is not greater than `b`, it may be equal to `b`, so the negation of `a > b` is `a ≤ b`, not merely `a < b`. These rules here concern the mathematical real-number domain.

**Worksheet Exercise 2.4(a).**

`¬((a ≤ 1) ∧ (a > 1))`  
`≡ ¬(a ≤ 1) ∨ ¬(a > 1)`  
`≡ (a > 1) ∨ (a ≤ 1)`.

This is true for every real `a`. Every real number either exceeds 1 or does not; not exceeding 1 is exactly being less than or equal to 1. Those alternatives cover all real numbers, including the boundary `a = 1`. Equivalently, the original conjunction can never hold, because a real number cannot simultaneously be at most 1 and greater than 1.

The instructor calls an always-true logical statement a **tautology**: it is true no matter what allowed values are assigned to its variables. For this worked example, the stated domain is all real numbers; do not discard that qualification.

**Worksheet Exercise 2.4(b).**

`¬(a ≥ b ≥ c)`  
`≡ ¬((a ≥ b) ∧ (b ≥ c))`  
`≡ ¬(a ≥ b) ∨ ¬(b ≥ c)`  
`≡ (a < b) ∨ (b < c)`.

One failed adjacent comparison is enough to break the original chain; it is not necessary for both to fail. In particular, simply reversing the chain to `a < b < c` would incorrectly require both failures.

## Predicates and the closing quantifier preview

Propositional variables represent whole true-or-false statements. To express a property whose truth depends on a number, an animal type, or another non-Boolean input, we use a **predicate**: a function that takes one or more inputs and returns a Boolean value. Its **domain** specifies the allowed inputs.

A predicate avoids separately naming every instance of the same property. Instead of different propositional variables for dogs being cute, cats being cute, and fish being cute, define `P(x)` to mean that animals of type `x` are cute, where `x` is a type of animal. Choosing a particular animal type supplies the input to the same predicate. This becomes especially useful for an infinite domain: one cannot write a finite list of individual propositions covering every even natural number.

The lecture's other examples are:
- `E(x)`: `x` is even, with `x` an integer. The input is an integer, but the output is True or False.
- `Q(x, y)`: `x < y`, with real-number inputs `x` and `y`, is the two-input predicate shown on the slide. The spoken description also used `x ≤ y`. These are different predicates: at equal inputs, `<` is False and `≤` is True. Use the comparison specified in the problem rather than treating the two symbols as interchangeable.

**Reading clarification — 3.2 Predicate Logic.** The official notes explain substitution: `P(x)` meaning *x is a power of 2*, for natural-number `x`, has a truth value that depends on `x`. Substituting `8` yields the true proposition `P(8)`; substituting `7` yields the false proposition `P(7)`. The predicate's **codomain**, its specified output set, is `{True, False}`. The notes require the input domain as part of a predicate definition. This is important because the predicate describes a property of allowed inputs, not an unexplained free-standing Boolean variable.

**Closing slide preview: the two quantifiers.** A **quantifier** says how broadly a predicate holds over a specified set `S`. The final slide displays:
- `∀x ∈ S, P(x)`: for every element `x` of `S`, `P(x)` is True. This is the **universal quantifier**; it makes a claim about all elements of the stated domain.
- `∃x ∈ S, P(x)`: there exists an element `x` of `S` satisfying `P(x)`. This is the **existential quantifier**; at least one qualifying element suffices, and the wording does not require exactly one.

Here `x ∈ S` means that `x` belongs to the set `S`, and *satisfies P* means that the predicate evaluates to True at that input. These definitions were visible at the close; the instructor deferred using predicates and quantifiers to construct more complex statements until the next lecture. Quantifier-negation rules and nested-quantifier exercises are therefore not part of this lecture's worked material.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
