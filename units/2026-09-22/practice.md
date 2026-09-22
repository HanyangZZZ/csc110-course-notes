# CSC110: Quantifiers, filtering, and nested statements — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Domains, expansion, and sentence status

8 practice marks

Let ℕ = {0,1,2,…} and ℤ⁺ = {1,2,3,…}. Expand A: ∀n∈ℤ⁺, P(n); B: ∀n∈ℕ, P(n); C: ∃n∈ℕ, P(n+1). Relate A and B. Is ∃x∈ℕ, P(n) a sentence? Explain.

Answer requirements:

- Use iterated AND/OR notation, showing the first three terms.

<details><summary>Hints</summary>

The expression passed to P can differ from the quantified variable.

Check which occurrence is actually controlled by the quantifier.

</details>

<details><summary>Worked solution and rubric</summary>

A = P(1) ∧ P(2) ∧ P(3) ∧ … . B = P(0) ∧ P(1) ∧ P(2) ∧ … . C = P(1) ∨ P(2) ∨ P(3) ∨ … . B is equivalent to P(0) ∧ A: it adds the zero requirement to A. The final formula is not a sentence, because n is free; quantifying x does not bind n.

- 2 marks: correct A expansion and starting point.
- 2 marks: correct B expansion and B ≡ P(0) ∧ A relation.
- 2 marks: correct C expansion, including OR and shifted inputs.
- 2 marks: identifies nonsentence and explains free n.

</details>

### all/any, set duplicates, empty collections

8 practice marks

For numbers = {1, 2, 3, 5, 49}, give the result of [x < 50 for x in numbers], {x < 50 for x in numbers}, and all applied to each. Explain why removing duplicate Booleans preserves all and any. Give all([]) and any([]), with reasons.

Answer requirements:

- Reason from Boolean values; do not rely on execution.

<details><summary>Hints</summary>

Multiplicity differs, but the presence of True or False does not.

An empty collection has neither a counterexample nor a witness.

</details>

<details><summary>Worked solution and rubric</summary>

The list contains five True values; the set is {True}. Applying all to either gives True. Duplicate removal preserves whether False is present, which determines failure of all, and whether True is present, which determines success of any. all([]) is True because no element violates the universal requirement. any([]) is False because no witness exists.

- 2 marks: both intermediate collections.
- 2 marks: both all results.
- 2 marks: explains duplicate removal for both all and any.
- 2 marks: correct empty results with witness/counterexample explanations.

</details>

### Filtering versus Boolean transformation

8 practice marks

Explain the different results of {x for x in range(1, 7) if x % 2 == 0} and {x % 2 == 0 for x in range(1, 7)}. Then, for strings = ['hi', 'hello', 'goodbye'], write a list comprehension keeping strings containing 'e' and converting them to uppercase. Give its result.

Answer requirements:

- Use a filtering list comprehension for the string task.

<details><summary>Hints</summary>

The build expression determines what is stored.

The trailing if determines which inputs contribute.

</details>

<details><summary>Worked solution and rubric</summary>

The first set is {2, 4, 6}: the condition selects even inputs and the build expression stores the integers. The second is {False, True}: it stores each input's evenness Boolean, and duplicates collapse. The string expression is [x.upper() for x in strings if 'e' in x], producing ['HELLO', 'GOODBYE']. 'hi' is excluded before transformation because it contains no 'e'.

- 2 marks: first set and filtering explanation.
- 2 marks: second set and Boolean-build explanation.
- 2 marks: correct string comprehension.
- 2 marks: correct output and explanation of exclusion/transformation.

</details>

### English translations and quantified negation

8 practice marks

Let S be a set of people. Translate 'Every person in S is cool or likes dogs, or both' into logic, and explain the OR. Negate your formula, pushing the negation to individual predicates. Translate 'At least one person in S likes pineapple on pizza' and its negation, using LikesPineapple.

Answer requirements:

- Keep the domain S in every quantified formula.

<details><summary>Hints</summary>

Flip the quantifier and then apply De Morgan's law.

Not everyone satisfies a property means some person fails it.

</details>

<details><summary>Worked solution and rubric</summary>

The first formula is ∀p∈S, (IsCool(p) ∨ LikesDogs(p)). OR is inclusive, so either property or both may hold. Its negation is ∃p∈S, (¬IsCool(p) ∧ ¬LikesDogs(p)): at least one person has neither property. The pineapple statement is ∃p∈S, LikesPineapple(p). Its negation is ∀p∈S, ¬LikesPineapple(p), meaning no person in S likes pineapple on pizza.

- 2 marks: universal formula and inclusive-OR explanation.
- 3 marks: negation with flipped quantifier, AND, and both predicates negated.
- 1 mark: original pineapple formula.
- 2 marks: negated pineapple formula and English meaning.

</details>

## Review quiz

### Original self-test: forall-implies and counterexamples

10 practice marks

A student translates 'Every natural number greater than 3 satisfies n²+n≥20' as ∀n∈ℕ, (n>3 ∧ n²+n≥20). Explain the mistake with a counterexample. Give two correct translations, one changing the domain and one not. Explain why the intended statement is true and evaluate the implication at n=1.

Answer requirements:

- This is an original self-test, not a claimed quiz-format match.
- Include zero in ℕ.

<details><summary>Hints</summary>

Does AND exclude n=0, or demand that it exceed 3?

For n≥4, bound n² and n separately.

</details>

<details><summary>Worked solution and rubric</summary>

The attempted formula demands that every natural number exceed 3. At n=0, the conjunction is false, so the universal formula is false. Correct forms are ∀n∈{x∈ℕ | x>3}, n²+n≥20 and ∀n∈ℕ, (n>3 ⇒ n²+n≥20). If n≥4 then n²≥16 and n≥4, so n²+n≥20. For n≤3 the implication is true because its hypothesis is false. In particular, at n=1, 1>3 is false and 1²+1≥20 is false, but their implication is true.

- 2 marks: explains overclaim and supplies n=0 counterexample.
- 2 marks: correct restricted-domain translation.
- 2 marks: correct implication translation.
- 2 marks: general argument for every qualifying n, not only n=4.
- 2 marks: correct n=1 evaluation and vacuous-truth reasoning.

</details>

### Original self-test: constrained filtering implementation

10 practice marks

Implement longest_cool_string(strings: list) -> int using a filtering comprehension, assuming at least one input contains 'cool'. It returns the greatest length among those strings. Give the result for ['cool beans', 'hello', 'David is cool']. Also write an any expression that tests whether at least one string contains 'cool', and explain why equality with 'cool' is insufficient.

Answer requirements:

- Use max with a filtering comprehension for the function.
- Use an explicit list or set comprehension in any.
- This is an original self-test; its constraint comes from the classroom exercise.

<details><summary>Hints</summary>

Filter strings before computing their lengths.

A substring need not equal the whole string.

</details>

<details><summary>Worked solution and rubric</summary>

```python
def longest_cool_string(strings: list) -> int:
    return max([len(x) for x in strings if 'cool' in x])
```

The example returns 13. The qualifying lengths are 10 and 13, so max selects 13; 'hello' is excluded. Existence can be tested with any(['cool' in x for x in strings]). Equality would miss strings such as 'David is cool'. The assumption guarantees at least one qualifying length for max; no no-match result is specified by this function's task.

- 4 marks: correct iteration, substring filter, len transformation, and outer max, 1 each.
- 2 marks: result 13 and explanation using qualifying lengths.
- 2 marks: correct any expression with explicit comprehension.
- 1 mark: explains substring/equality distinction.
- 1 mark: preserves and explains the nonempty-match assumption.

</details>

### Original self-test: equivalent translations and support

10 practice marks

Over a set P of programs, use Python(x) and Correct(x). Give a universal-implication formula and a negated-existential formula for 'No Python program is correct'. Derive their equivalence. Explain why ∀x∈P, (Python(x) ∧ ¬Correct(x)) is not the intended translation.

Answer requirements:

- Show the intermediate OR formula.
- This is an original self-test, not a predicted assessment item.

<details><summary>Hints</summary>

Negate the claim that a correct Python program exists.

A non-Python member should not make the intended statement fail.

</details>

<details><summary>Worked solution and rubric</summary>

The formulas are ∀x∈P, (Python(x) ⇒ ¬Correct(x)) and ¬(∃x∈P, (Python(x) ∧ Correct(x))). Starting with the second, quantifier negation gives ∀x∈P, ¬(Python(x) ∧ Correct(x)); De Morgan gives ∀x∈P, (¬Python(x) ∨ ¬Correct(x)); implication equivalence gives the first formula. The AND attempt says every program is Python and incorrect. For example, with one incorrect Python program and one non-Python program, no Python program is correct, but the AND formula fails at the non-Python member.

- 2 marks: correct universal-implication formula.
- 2 marks: correct negated-existential formula.
- 3 marks: quantifier flip, De Morgan step, implication step, 1 each.
- 3 marks: explains excess requirement and supplies a separating example with both truth values.

</details>

### Original self-test: table interpretation and quantifier order

12 practice marks

Use A={Breanna, Malena, Patrick, Ella}, B={Sophia, Thelonious, Stanley, Laura}, and the following rows in B order: Breanna=[F,T,T,F], Malena=[F,T,T,T], Patrick=[F,F,T,F], Ella=[F,F,T,T]. Evaluate and explain: (a) ∀a∈A, Loves(a,Stanley); (b) ∀b∈B, Loves(Ella,b); (c) ∀a∈A, ∀b∈B, Loves(a,b); (d) ∃a∈A, ∃b∈B, Loves(a,b); (e) ∀a∈A, ∃b∈B, Loves(a,b); (f) ∃b∈B, ∀a∈A, Loves(a,b). Translate (e) and (f) into English.

Answer requirements:

- Use rows for A and columns for B.
- This is an original self-test based on the worked worksheet, not a quiz prediction.

<details><summary>Hints</summary>

A universal over b scans a fixed person's row.

One witness per row differs from one shared column.

</details>

<details><summary>Worked solution and rubric</summary>

(a) True: all four Stanley-column entries are true. (b) False: Ella's row has false entries for Sophia and Thelonious. (c) False: every cell would have to be true, but Patrick–Thelonious is false. (d) True: Breanna–Thelonious is one true pair. (e) True: everyone in A loves someone in B; every row has a true entry. (f) True: someone in B is loved by everyone in A; Stanley's column is all true. In (e), the person chosen from B may depend on a; in (f), one fixed b must work for all a.

- 2 marks: (a) truth value and correct column reasoning.
- 2 marks: (b) truth value and row counterexample.
- 2 marks: (c) truth value and all-cells explanation.
- 2 marks: (d) truth value and witness pair.
- 2 marks: (e) truth value, English meaning, and row criterion.
- 2 marks: (f) truth value, English meaning, and shared Stanley witness.

</details>

## Challenge quiz

### Separating mixed quantifiers with a minimal table change

10 practice marks

Start with the Loves table whose columns, in order, are Sophia, Thelonious, Stanley, Laura, and whose rows are Breanna=[F,T,T,F], Malena=[F,T,T,T], Patrick=[F,F,T,F], Ella=[F,F,T,T]. Change exactly one cell so ∀a∈A, ∃b∈B, Loves(a,b) remains true but ∃b∈B, ∀a∈A, Loves(a,b) becomes false. Prove both claims using all relevant rows/columns. Explain why deleting Patrick's love for Stanley would not achieve the goal.

Answer requirements:

- Change only one Boolean value.
- Do not assume checking Stanley alone rules out every shared witness.

<details><summary>Hints</summary>

Break the only all-true column without emptying any row.

Inspect what alternatives remain in the changed row.

</details>

<details><summary>Worked solution and rubric</summary>

Change Loves(Malena, Stanley) from True to False. Every row still has a true entry: Breanna–Thelonious, Malena–Laura, Patrick–Stanley, and Ella–Stanley are witnesses. No column is all true: Sophia has false entries throughout; Thelonious fails at Patrick and Ella; Stanley now fails at Malena; Laura fails at Breanna and Patrick. These four columns exhaust B, so there is no shared witness. Deleting Patrick–Stanley instead would leave Patrick's entire row false, also destroying the ∀a∃b statement.

- 2 marks: valid single-cell change.
- 3 marks: accounts for witnesses in all four rows.
- 3 marks: rules out all four columns and concludes no shared witness.
- 2 marks: explains why Patrick's row would become empty of true entries.

</details>

### Nested quantifiers, dependence, and precise negation

12 practice marks

For ℕ containing zero, compare A: ∃x∈ℕ, ∀y∈ℕ, x≥y and B: ∀y∈ℕ, ∃x∈ℕ, x≥y. Give their meanings and truth values with general arguments. Negate each, pushing negations to the comparison. Diagnose the arguments 'A is false just because ℕ is infinite' and 'B requires x to be strictly greater than y'.

Answer requirements:

- Use witnesses/counterexamples that work for arbitrary relevant inputs.
- Preserve the displayed comparison ≥.

<details><summary>Hints</summary>

For any proposed maximum x, consider x+1.

To satisfy ≥, equality is allowed.

Flip both quantifiers when moving the negation inward.

</details>

<details><summary>Worked solution and rubric</summary>

A says that one natural number is at least as large as every natural number. It is false: for any proposed x, y=x+1 is natural and makes x≥y false. B says that each natural y has a natural x at least as large as it. It is true: choose x=y, separately for each y. ¬A is ∀x∈ℕ, ∃y∈ℕ, x<y. ¬B is ∃y∈ℕ, ∀x∈ℕ, x<y. Infinitude alone is not the sufficient reason for A's failure; the successor argument supplies a larger natural for every candidate. B does not require strict inequality: x=y works, although x=y+1 works too. The witness can depend on y in B but must be fixed before all y in A.

- 3 marks: A meaning, false value, and general successor counterexample.
- 3 marks: B meaning, true value, and valid dependent witness.
- 2 marks: correct negation of A.
- 2 marks: correct negation of B.
- 2 marks: diagnoses infinitude-only reasoning and the strictness error, 1 each.

</details>

### Comparing intended and vacuously true existential conditions

12 practice marks

Let S={0,1,2,3,4}, H(n) mean n<4, and Q(n) mean n²+n≥20. Compare F=∃n∈S, (H(n)∧Q(n)) and G=∃n∈S, (H(n)⇒Q(n)). Give their truth values and reasons, then write correct Python for each over numbers={0,1,2,3,4}. Negate F in two equivalent forms: one using a universal OR and one using a universal implication.

Answer requirements:

- Use explicit list or set comprehensions with any.
- Do not replace the finite domain with ℕ.

<details><summary>Hints</summary>

n=4 is excluded by H, but what does a false hypothesis do to implication?

Negate AND into OR, then recognize an implication.

</details>

<details><summary>Worked solution and rubric</summary>

F is false: for n=0,1,2,3 the values 0,2,6,12 fail Q; at n=4, H is false. G is true: at n=4 the false hypothesis makes the implication true, supplying an unintended witness. F can be written any([n ** 2 + n >= 20 for n in numbers if n < 4]). G can be written any([not (n < 4) or n ** 2 + n >= 20 for n in numbers]). The negation of F is ∀n∈S, (¬H(n) ∨ ¬Q(n)), equivalently ∀n∈S, (H(n) ⇒ ¬Q(n)). In numeric form the implication conclusion is n²+n<20. Both negated forms assert that every qualifying input fails Q.

- 3 marks: F false with exhaustive treatment of included/excluded inputs.
- 2 marks: G true with n=4 and vacuous-truth explanation.
- 2 marks: correct Python for F.
- 2 marks: correct Python for G.
- 3 marks: correct universal OR, implication equivalent, and meaning, 1 each.

</details>

### Scope, vacuous truth, and an empty filtered collection

12 practice marks

A student writes Q(y) ∧ (∀y∈S, P(y)) and claims it is a sentence because y has a quantifier. Explain the error and give a sentence that says every member of S satisfies both P and Q. Then suppose S is empty: evaluate your sentence and ∃y∈S, (P(y)∧Q(y)). Finally, for numbers={0,1,2,3}, evaluate all([n**2+n >= 20 for n in numbers if n>3]) and any([n**2+n >= 20 for n in numbers if n>3]). Explain why neither result establishes that a qualifying number exists.

Answer requirements:

- Use parentheses to make scope explicit.
- Separate truth of a universal property from existence of inputs satisfying its restriction.

<details><summary>Hints</summary>

A quantifier does not reach backward outside its body.

First determine the filtered collection, then apply all or any.

</details>

<details><summary>Worked solution and rubric</summary>

The y occurrence in Q(y) is outside the scope of ∀y and remains free, so the original formula is not a sentence. The requested sentence is ∀y∈S, (P(y) ∧ Q(y)). If S is empty it is true, because there is no failing member; the existential formula is false, because no witness exists. In the Python expressions no element of numbers exceeds 3, so each comprehension is []. The all expression is True and the any expression is False. The universal result imposes no existence requirement, while the existential result explicitly fails to provide a qualifying witness; neither establishes that there is a number passing the filter.

- 2 marks: identifies free occurrence and scope error.
- 2 marks: correct repaired sentence with explicit scope.
- 2 marks: empty-domain universal and existential values with reasons.
- 2 marks: identifies empty filtered collection and correct all result.
- 2 marks: correct any result and witness explanation.
- 2 marks: clearly separates universal truth from existence of qualifying inputs.

</details>
