# CSC110: Quantifiers, filtering, and nested statements

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Predicates, domains, and the two quantifiers

## Predicates describe inputs
A **predicate** is a function that returns a Boolean value: `True` or `False`. Its **domain** is the set of allowed inputs. Examples include P(x), meaning that animal type x is cute; E(x), meaning that integer x is even; and Q(x, y), meaning x < y for real numbers x and y. Predicates can take more than one input, and an ordinary comparison such as x < y can itself be viewed as a predicate.

Supplying particular inputs gives a statement whose truth can be evaluated. A **quantifier** instead specifies how to combine a predicate's truth values across a domain:
- **Universal quantification**, ∀x ∈ S, P(x), means P(x) is true for every element of S.
- **Existential quantification**, ∃x ∈ S, P(x), means P(x) is true for at least one element of S. It does not mean exactly one.

The domain matters: a claim about everyone in S need not be a claim about people outside S. Existential quantification is not simply the negation of universal quantification; both can be true when everyone in a nonempty set satisfies the predicate.

## Expansion makes the meaning visible
A universal quantifier behaves like a large **conjunction**—an AND of all the instances. An existential quantifier behaves like a large **disjunction**—an OR of all the instances.

In this course, ℕ = {0, 1, 2, …}. Thus:
- ∀x ∈ ℕ, x ≥ 0 expands to (0 ≥ 0) ∧ (1 ≥ 0) ∧ (2 ≥ 0) ∧ … . Every instance is true, so the statement is true.
- ∀x ∈ {0, 1}, x > 5 expands to (0 > 5) ∧ (1 > 5). It is false. A single false instance would already be enough to make a universal statement false.
- ∃x ∈ ℕ, x ≤ 0 expands to (0 ≤ 0) ∨ (1 ≤ 0) ∨ (2 ≤ 0) ∨ … . The instance x = 0 is true, so the whole statement is true even though the later instances are false. Such an input establishing an existential statement is called a **witness**.

For infinite domains, these expansions explain the mathematics; they are not instructions to finish an infinite computation.

## Translating English and logic
Let S be a set of people, with predicates IsCool and LikesDogs.
- IsCool(Paul), assuming Paul ∈ S: Paul is cool.
- ∀p ∈ S, IsCool(p): every person in S is cool.
- ∃p ∈ S, IsCool(p): at least one person in S is cool.
- ∀p ∈ S, (IsCool(p) ∨ LikesDogs(p)): every person in S is cool, likes dogs, or both. Mathematical OR is **inclusive**: both properties may hold. An English phrase suggesting exactly one of the two would change the meaning.
- At least one person in S is cool and does not like dogs: ∃p ∈ S, (IsCool(p) ∧ ¬LikesDogs(p)). Both requirements must hold for the same person.

A quantified predicate can contain other logical connectives: AND (∧), OR (∨), NOT (¬), implication (⇒, if … then), and biconditional (⇔, if and only if).

## Sentences and the scope of a quantifier

A **formula** combines variables, predicates, logical operators, and possibly quantifiers. A **sentence** is a formula with no unquantified variable occurrences. This makes it a complete claim rather than a claim still requiring an unspecified input.

The **scope** of a quantifier is the part of the formula it controls. For nested quantifiers, read the expression after a quantifier's comma as its body:

∀x ∈ S, (∀y ∈ S, P(x, y)).

It is not enough that a quantifier uses the right variable name somewhere in the formula: the occurrence must actually lie within that quantifier's scope.

## The four classroom cases
1. **∀x ∈ S, P(x, y): not a sentence.** The x occurrence is quantified, but y is unspecified.
2. **∀x ∈ S, (∀y ∈ S, P(x, y)): a sentence.** Both inputs to P are controlled by their quantifiers.
3. **Q(y) ∧ (∀x ∈ S, P(x)): not a sentence.** The y in Q(y) is unquantified. Similarly, Q(y) ∧ (∀y ∈ S, P(y)) is still not a sentence: the second y quantifier controls P(y), not the earlier Q(y).
4. **(∀y ∈ ℕ, Q(y)) ∧ (∀x ∈ S, P(x)): a sentence.** Each variable occurrence is inside its respective quantifier's scope. The instructor also considered interpreting the second quantified expression inside the first one's body; either interpretation still leaves all variable occurrences quantified. This observation concerns sentence status, not a general licence to ignore parentheses.

The instructor said test examples would use parentheses to make the intended grouping clearer.

### Reading clarification — Writing sentences in predicate logic
The official reading calls quantified occurrences **bound** and unquantified occurrences **free**. Being a sentence does not mean being true: ∀x ∈ ℕ, (∀y ∈ ℕ, x² > y) is a sentence, but it is false, for example at x = 0 and y = 0. Sentence status asks whether the inputs are specified by quantification; truth asks whether the resulting claim holds.

### Reading guidance — Commas: avoid them!
A comma is not a substitute for AND or implication. Use ∧ or ⇒ explicitly. Valid commas occur after a quantification, between variables sharing a quantification, and between predicate arguments. For example, ∀x, y ∈ ℕ, P(x, y) uses commas in these legitimate roles.

## Expanding quantifiers, comparing formulas, and empty domains

## Worked expansion exercise
Here ℤ⁺ = {1, 2, 3, …}, whereas ℕ = {0, 1, 2, …}. Let P be a predicate on the relevant numbers.

| Part | Formula | Expansion or diagnosis |
|---|---|---|
| A | ∀n ∈ ℤ⁺, P(n) | P(1) ∧ P(2) ∧ P(3) ∧ … |
| B | ∀x ∈ ℕ, P(x) | P(0) ∧ P(1) ∧ P(2) ∧ … |
| C | ∀y ∈ ℕ, P(y + y) | P(0) ∧ P(2) ∧ P(4) ∧ … |
| D | ∃m ∈ ℤ⁺, P(m) | P(1) ∨ P(2) ∨ P(3) ∨ … |
| E | ∃x ∈ ℕ, P(n) | Not a sentence: n is free. Quantifying x does not quantify n. |
| F | ∃n ∈ ℕ, P(n + 1) | P(1) ∨ P(2) ∨ P(3) ∨ … |

B is equivalent to P(0) ∧ A: it requires all the positive-number cases from A and also the zero case. C checks every even nonnegative integer, including zero. Substituting y = 0, 1, 2, … produces exactly 0, 2, 4, …; it does not check odd inputs.

D and F are **equivalent formulas**, meaning they have the same truth value for every allowed interpretation of P. Although their quantified domains differ, the inputs actually supplied to P are exactly the same positive integers. In F, adding one maps each natural number to a positive integer and reaches every positive integer. Their identical OR expansions show why they are equivalent. Merely happening to have the same truth value for one particular P would not establish equivalence.

## Quantifying over the empty set
For S = ∅:
- ∀x ∈ S, P(x) is **true**. A universal statement fails only if some element violates P; there is no element available to violate it. This is an empty conjunction, an AND of zero statements.
- ∃x ∈ S, P(x) is **false**. There is no element available to be a witness. This is an empty disjunction, an OR of zero statements.

Python agrees:
```python
all([])  # True
any([])  # False
```
These results also matter when filtering removes every element from a collection.

## Quantifiers in Python: build Boolean collections, then aggregate

An **aggregation** combines a collection into one result. For a collection of Booleans:
- `all(bools)` returns whether every Boolean is `True`.
- `any(bools)` returns whether at least one Boolean is `True`.

Use `all` for ∀ and `any` for ∃. A **comprehension** builds the collection of predicate results that the aggregation needs.

## Worked design: every number is below 50
Translate ∀x ∈ numbers, x < 50 for:
```python
numbers = {1, 2, 3, 5, 49}
```
1. The universal quantifier determines the outer function: `all`.
2. `for x in numbers` visits the domain.
3. `[x for x in numbers]` is an **identity comprehension**: it collects the original integers without changing them. That is not the collection of predicate results we need.
4. Put the predicate in the build expression:
```python
[x < 50 for x in numbers]
# [True, True, True, True, True]

all([x < 50 for x in numbers])
# True
```
All five values pass the comparison, so the Boolean collection consists of five `True` values.

## Why a set also works here
```python
{x < 50 for x in numbers}       # {True}
all({x < 50 for x in numbers})  # True
```
Sets remove duplicate values, so five `True` results become one. This does not change `all` or `any`: `all` cares whether a `False` is present, and `any` cares whether a `True` is present, not how many copies occur. A Boolean set can preserve both `True` and `False` when both occur. List and set comprehensions therefore give the same quantified result here; this is not a claim that discarding duplicates preserves every aggregation.

## Biconditional example
Assume `is_prime` and `is_special` are already defined Boolean-returning functions. The statement

∀x ∈ numbers, (is_prime(x) ⇔ is_special(x))

becomes:
```python
all({is_prime(x) == is_special(x) for x in numbers})
```
A biconditional is true when its two sides have the same truth value: both true or both false. Equality `==` compares those Boolean results. This is not a one-way implication, and it does not assert that every number is prime.

### Reading clarification — Python built-ins: any and all
The official reading emphasizes that Python collections are computationally limited. A finite comprehension over `numbers` does not check all of an infinite mathematical domain such as ℕ or ℝ. Keep the domain of your Python statement aligned with the collection actually supplied.

## Restricting universal statements: forall-implies

A **condition** restricts which elements of a domain a claim concerns. For example:

∀n ∈ ℕ, n² + n ≥ 20

is false: n = 0 gives 0 ≥ 20, which is false. This one **counterexample**, an input that violates the claim, is enough to refute a universal statement.

But every natural number **greater than 3** does satisfy the inequality. The smallest qualifying natural number is 4, and 4² + 4 = 20. For any n ≥ 4, n² ≥ 16 and n ≥ 4, so n² + n ≥ 20. This explains the result for all qualifying numbers, rather than merely checking the first one.

## Two correct ways to express the restriction
We can change the domain:

∀n ∈ {x ∈ ℕ | x > 3}, n² + n ≥ 20.

The set-builder expression means the set of natural numbers x satisfying x > 3. Alternatively, we can keep ℕ as the domain and use implication:

∀n ∈ ℕ, (n > 3 ⇒ n² + n ≥ 20).

## Why AND is the wrong connective here
The attempted translation

∀n ∈ ℕ, (n > 3 ∧ n² + n ≥ 20)

requires every natural number both to exceed 3 and to satisfy the inequality. At n = 0 the first condition is false, so that conjunct is false and the entire universal statement fails. This does not filter out the small numbers; it incorrectly demands that they pass the filter.

## Why implication works
In H ⇒ C, H is the **hypothesis** and C is the **conclusion**. An implication is false only when H is true and C is false. When H is false, the implication is true regardless of C; this is the **vacuous truth** case.

Expand the correct formula as an AND of implications:

(0 > 3 ⇒ 0² + 0 ≥ 20) ∧ (1 > 3 ⇒ 1² + 1 ≥ 20) ∧ … .

- For n = 0, 1, 2, 3, the hypothesis n > 3 is false. Each implication is therefore true. For example, at n = 1 both the hypothesis and conclusion are false, but the implication is true.
- For n ≥ 4, the hypothesis is true, so the implication requires the inequality to hold. At n = 4 the sum is 20; at n = 5 it is 30; the general bound above handles every larger natural number.

These cases exhaust ℕ: each natural number either exceeds 3 or does not. The excluded cases contribute `True` to the outer AND and cannot make it fail, while every included case must satisfy the conclusion.

## General pattern
**Forall-implies** expresses that every member of S satisfying P also satisfies Q:

∀x ∈ S, (P(x) ⇒ Q(x))

is equivalent to

∀x ∈ {u ∈ S | P(u)}, Q(x).

The quantified domain in the first formula is still S. The implication restricts where Q is required to hold. If no elements satisfy P, the statement is true, just like universal quantification over an empty filtered domain.

## Restricting existential statements: exists-and

The unrestricted statement

∃n ∈ ℕ, n² + n ≥ 20

is true: n = 4 is a witness because 4² + 4 = 20.

Now restrict the claim to natural numbers **less than 4**:

∃n ∈ {x ∈ ℕ | x < 4}, n² + n ≥ 20.

This is false. The allowed inputs are exactly 0, 1, 2, 3, and their values of n² + n are 0, 2, 6, 12. None reaches 20. Checking these four inputs is sufficient because they exhaust the restricted domain.

## Why implication fails for this restriction
Try keeping ℕ as the domain and writing:

∃n ∈ ℕ, (n < 4 ⇒ n² + n ≥ 20).

This is an OR of implications. At n = 4, the hypothesis 4 < 4 is false, so the implication is true by vacuous truth. One true instance makes the whole existential statement true. Thus an input that should have been excluded supplies a witness to the wrong formula. Every n ≥ 4 causes the same problem.

## Why AND works
The correct unrestricted-domain form is:

∃n ∈ ℕ, (n < 4 ∧ n² + n ≥ 20).

Its OR expansion has two kinds of terms:
- For n = 0, 1, 2, 3, the filtering condition is true, but the inequality is false. Each conjunction is false.
- For n ≥ 4, the filtering condition is false. Each conjunction is false regardless of the inequality.

Again these cases exhaust ℕ. Every disjunct is false, so the existential statement is false, exactly as intended.

## General pattern and comparison
**Exists-and** expresses that some member of S satisfies both the restriction P and the desired property Q:

∃x ∈ S, (P(x) ∧ Q(x))

is equivalent to

∃x ∈ {u ∈ S | P(u)}, Q(x).

For a universal statement, excluded inputs must contribute `True` to the large AND, so use implication. For an existential statement, excluded inputs must contribute `False` to the large OR, so use conjunction. This difference—not simply memorizing two symbols—explains why the two filtering patterns work.

## Filtering comprehensions: select, transform, and aggregate

**Filtering** keeps only those elements of a collection that satisfy a Boolean condition. The lecture motivated it with three tasks: sum only even integers; count points within one unit of (0, 0); and find the maximum length among strings containing `'Strawberry'`. In each task, first select the relevant data, then compute the required result.

## Syntax and evaluation
A normal set comprehension is:
```python
{expression for variable in collection}
```
A filtering comprehension adds an `if` condition at the end:
```python
{expression for variable in collection if condition}
```
For each input, test the condition. Only if it is true does that input contribute the build expression to the result. The condition can be a compound Boolean expression. The build expression can still transform the selected input.

### Reading clarification — Filtering collections in Python
The same structure works for lists and dictionaries:
```python
[expression for variable in collection if condition]
{key_expr: value_expr for variable in collection if condition}
```
The reading makes the evaluation order explicit: the build expression is evaluated only for inputs passing the condition. This distinguishes selection from merely computing a Boolean for every input.

## Worked example: even integers from 1 through 20
```python
{x for x in range(1, 21)}
# {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}

{x for x in range(1, 21) if x % 2 == 0}
# {2, 4, 6, 8, 10, 12, 14, 16, 18, 20}
```
`range(1, 21)` includes 1 through 20; its stop value 21 is excluded. `%` gives the remainder after division. An integer is even precisely when dividing by 2 leaves remainder 0. The filter tests that condition, while the build expression `x` keeps the number itself.

Compare:
```python
{x % 2 == 0 for x in range(1, 21)}
# {False, True}
```
This does not filter integers. It computes an evenness Boolean for every input and stores those Boolean results. Both even and odd inputs occur, so both Boolean values appear; duplicate results disappear in the set.

## Worked example: select strings, then change case
```python
strings = ['hi', 'hello', 'goodbye']
[x for x in strings]
# ['hi', 'hello', 'goodbye']

[x for x in strings if 'e' in x]
# ['hello', 'goodbye']

[x.upper() for x in strings if 'e' in x]
# ['HELLO', 'GOODBYE']
```
`'e' in x` tests whether the string contains that substring. `'hi'` fails and is omitted. Calling `x.upper()` changes the surviving strings to uppercase; it does not change which original strings pass the filter.

## Filtering quantified computations
The instructor recommended filtering comprehensions in Python rather than unnecessarily encoding the filtering effect with logical connectives. For a finite collection `numbers`, the universal condition can be written:
```python
all([n ** 2 + n >= 20 for n in numbers if n > 3])
```
An equivalent direct implication translation is:
```python
all([not (n > 3) or n ** 2 + n >= 20 for n in numbers])
```
The latter uses H ⇒ C ≡ ¬H ∨ C. For existential filtering, either use a filtered `any` or put the restriction and property together with `and`. These are logical alternatives, but the comprehension's `if` often expresses the programming task more conveniently.

### Reading example — Combining filtering with aggregation
For `numbers = {1, 2, 3, 4, 5}`, the official reading gives:
```python
sum({n for n in numbers if n % 2 == 0})  # 6
```
The filter retains 2 and 4, then `sum` adds them. Selection determines the inputs to the aggregation; it is not a replacement for the aggregation.

## Worked filtering exercise: programs and strings

## Translating claims about Python programs
Let P be a set of programs intended to solve the same task. Define Python(x) to mean that x is written in Python, and Correct(x) to mean that x solves the task correctly. The domain can include non-Python programs as well as incorrect programs.

**Every Python program is correct:**

∀x ∈ P, (Python(x) ⇒ Correct(x)).

This imposes correctness on the Python members, without demanding that other programs be written in Python or be correct.

**No Python program is correct:**

∀x ∈ P, (Python(x) ⇒ ¬Correct(x)).

Every Python member must be incorrect. Two equivalent forms discussed were:
- ∀x ∈ P, (Correct(x) ⇒ ¬Python(x)). This is the **contrapositive** of the implication: if H implies C, then not-C implies not-H. Here H is Python(x) and C is ¬Correct(x).
- ¬(∃x ∈ P, (Python(x) ∧ Correct(x))). The inner expression says that a correct Python program exists; negating it says that none exists.

The attempted translation ∀x ∈ P, (Python(x) ∧ ¬Correct(x)) is too strong. It asserts that every program is Python and incorrect. A non-Python program makes it false, even though that program has no bearing on whether any Python program is correct. The negation section below derives the equivalence of the correct forms.

## Implementing longest_cool_string
The exercise required completing the doctest and implementing the function **using a filtering comprehension**. Its explicit assumption is that at least one input string contains `'cool'`.

```python
def longest_cool_string(strings: list) -> int:
    """Return the length of the longest string containing 'cool'.

    Assume at least one string in strings contains 'cool'.

    >>> longest_cool_string(['cool beans', 'hello', 'David is cool'])
    13
    """
    return max([len(x) for x in strings if 'cool' in x])
```
Read the computation from the selection outward:
1. Iterate through `strings`.
2. Retain strings satisfying `'cool' in x`: `'cool beans'` and `'David is cool'`.
3. Compute their lengths, 10 and 13. Spaces count as characters.
4. `max` accepts the collection of lengths and returns its largest element, 13.

The function returns a length, not the longest string itself. The stated assumption ensures that the filtered collection has a value for `max` to select; the exercise did not specify a result when no string qualifies.

## Testing whether a matching string exists
At least one string in `strings` contains the substring `'cool'`:
```python
any({'cool' in x for x in strings})
```
A list comprehension works too. Substring inclusion is not equality: `'David is cool'` contains `'cool'` but is not equal to `'cool'`.

The instructor also described alternatives:
```python
any([True for x in strings if 'cool' in x])
len([x for x in strings if 'cool' in x]) > 0
```
The first produces one `True` per match. If there are no matches, it passes an empty list to `any`, which returns `False`. The second checks whether the filtered list has any elements. Both express existence, not the number of matches.

For this course treatment, keep an explicit list or set comprehension inside `all`/`any`. The instructor acknowledged a Python feature allowing the collection brackets to be omitted but explicitly left that feature outside this treatment.

## Negating quantified statements

A **negation** says that a statement is false. Negating a quantified statement requires changing the quantifier as well as negating its body, while preserving its domain:

¬(∀x ∈ S, P(x)) ≡ ∃x ∈ S, ¬P(x)

¬(∃x ∈ S, P(x)) ≡ ∀x ∈ S, ¬P(x).

Here ≡ means that the formulas are logically equivalent.

## Why the rules work
A universal statement is an AND of all its instances. Its negation says that at least one instance fails, so it becomes an OR of the negated instances. An existential statement is an OR; to make it false, every instance must fail, so its negation becomes an AND of the negated instances. These are the quantified versions of **De Morgan's laws**:

¬(H ∧ C) ≡ ¬H ∨ ¬C, and ¬(H ∨ C) ≡ ¬H ∧ ¬C.

- Everyone in the class is cool is negated by at least one person in the class is not cool. Saying everyone is not cool demands too much: one non-cool person already refutes the original statement.
- At least one person in the class likes pineapple on pizza is negated by no one in the class likes pineapple on pizza, or equivalently every person in the class does not like it.

## Worked derivation: no correct Python programs
Start with the negated existential form:

¬(∃x ∈ P, (Python(x) ∧ Correct(x))).

1. Move the negation through the quantifier, flipping ∃ to ∀:
   ∀x ∈ P, ¬(Python(x) ∧ Correct(x)).
2. Apply De Morgan's law inside:
   ∀x ∈ P, (¬Python(x) ∨ ¬Correct(x)).
3. Recognize H ⇒ C ≡ ¬H ∨ C, with H = Python(x) and C = ¬Correct(x):
   ∀x ∈ P, (Python(x) ⇒ ¬Correct(x)).

Thus saying that no correct Python program exists is equivalent to saying that every Python program is incorrect. Each step preserves meaning; it is not merely that both formulas happened to evaluate the same way for one example set.

### Reading clarification — Manipulating negation
The official reading explains why pushing negations inward makes formulas easier to interpret. In particular, ¬(H ⇒ C) ≡ H ∧ ¬C: an implication fails precisely when its hypothesis holds but its conclusion does not. This is useful when negating the lecture's forall-implies statements: a counterexample must pass the restriction and fail the required property.

## Nested quantifiers: order controls dependence

With **nested quantifiers**, one quantified statement is inside another. Read from the outside inward, keeping the scope clear. The order of different quantifiers can change the meaning and truth value.

## One number that works for every other number
Consider:

∃x ∈ ℕ, (∀y ∈ ℕ, x ≥ y).

The outer existential asks for one x that makes the entire inner universal statement true. Expanding the outer quantifier first gives:

(∀y ∈ ℕ, 0 ≥ y) ∨ (∀y ∈ ℕ, 1 ≥ y) ∨ … .

Expanding the inner quantifiers then gives:

[(0 ≥ 0) ∧ (0 ≥ 1) ∧ …] ∨ [(1 ≥ 0) ∧ (1 ≥ 1) ∧ (1 ≥ 2) ∧ …] ∨ … .

Each bracket tests one proposed greatest natural number. The first fails at y = 1; the second fails at y = 2. In general, any proposed x fails at y = x + 1, which is another natural number larger than x. Therefore every bracket is false, and the outer OR is false. There is no greatest natural number.

The decisive reason is the availability of x + 1 for every natural x, not infinitude alone: an infinite ordered set need not lack a greatest member.

## A number chosen separately for each input
Now reverse the quantifier order without changing their variables or the predicate:

∀y ∈ ℕ, (∃x ∈ ℕ, x ≥ y).

Its expansion is:

(∃x ∈ ℕ, x ≥ 0) ∧ (∃x ∈ ℕ, x ≥ 1) ∧ …

= [(0 ≥ 0) ∨ (1 ≥ 0) ∨ …] ∧ [(0 ≥ 1) ∨ (1 ≥ 1) ∨ …] ∧ … .

Every bracket has a true entry: for the bracket corresponding to y, choose x = y. Thus every inner existential is true and the outer universal is true.

The comparison is **≥**, so the exact meaning is that for every natural y there is a natural x **at least as large** as y. It does not require strict inequality. Choosing x = y + 1 would work as well, but equality already suffices.

## What dependence means
In ∀y ∃x, the witness x may depend on the already chosen y. We may choose x = 0 for y = 0, x = 1 for y = 1, and so on. No single x must handle every y.

In ∃x ∀y, x is chosen first and must remain fixed while y ranges over all of ℕ. Allowing a new x for every y would silently change the statement. An existential witness may depend on variables quantified outside it—to its left—but not on later variables whose quantifiers are inside its body.

## Loves table: checking single and multiple quantifiers

Let A = {Breanna, Malena, Patrick, Ella} and B = {Sophia, Thelonious, Stanley, Laura}. The two-input predicate Loves: A × B → {True, False} means that Loves(a, b) is true when person a loves person b. Here A × B is the set of pairs whose first member comes from A and second member comes from B.

Rows represent a ∈ A; columns represent b ∈ B:

| a / b | Sophia | Thelonious | Stanley | Laura |
|---|---|---|---|---|
| Breanna | False | True | True | False |
| Malena | False | True | True | True |
| Patrick | False | False | True | False |
| Ella | False | False | True | True |

For example, Loves(Breanna, Thelonious) is true, whereas Loves(Patrick, Thelonious) is false. Read the first input as the row and the second as the column; do not reverse the relationship.

## Worked evaluations
**1(a) ∀a ∈ A, Loves(a, Stanley): true.** Fix the Stanley column and vary a through all four rows. Breanna, Malena, Patrick, and Ella all love Stanley, so the four entries form a true conjunction.

**1(b) ∀b ∈ B, Loves(Ella, b): false.** Fix Ella's row and vary the column. Ella does not love Sophia or Thelonious. Either false entry alone disproves the claim that she loves everyone in B.

**2(a) ∀a ∈ A, (∀b ∈ B, Loves(a, b)): false.** This says everyone in A loves everyone in B. Every table entry would have to be true, but the table contains false entries, such as Patrick–Thelonious.

**2(b) ∃a ∈ A, (∃b ∈ B, Loves(a, b)): true.** This says there is at least one pair with a love relationship. Breanna–Thelonious is one witness pair. The formula would be false only if every entry were false; it does not require everyone to love someone.

**3(a) ∀a ∈ A, (∃b ∈ B, Loves(a, b)): true.** Everyone in A loves someone in B. Check for at least one true entry in each row. Witnesses may differ: Breanna can choose Thelonious, Malena can choose Laura, and Patrick and Ella can choose Stanley.

**3(b) ∃b ∈ B, (∀a ∈ A, Loves(a, b)): true.** Someone in B is loved by everyone in A. Now one fixed column must consist entirely of true entries. Stanley supplies that column.

The last two formulas happen to be true for this table, but they have different meanings. Finding someone for each row does not by itself establish a single person shared by every row.

### Reading addition — Alternating quantifiers
The official reading supplies a concrete way to separate the last two statements: change only Loves(Malena, Stanley) to false. Everyone still loves someone: Malena still loves Thelonious and Laura, and the other rows are unchanged. But no column is now entirely true. Sophia already has false entries; Thelonious has false entries for Patrick and Ella; Laura has false entries for Breanna and Patrick; and Stanley now has a false entry for Malena. These are all four possible people in B, so no shared witness remains. Thus ∀a ∃b stays true while ∃b ∀a becomes false. This is a reading explanation of the homework idea, not a change carried out in the recorded class discussion.

### Reading addition — Now, multiple variables
For fixed domains, consecutive quantifiers of the **same type** can exchange order:
- ∀a ∈ A, ∀b ∈ B, Loves(a, b) and ∀b ∈ B, ∀a ∈ A, Loves(a, b) both require every pair to satisfy Loves.
- ∃a ∈ A, ∃b ∈ B, Loves(a, b) and ∃b ∈ B, ∃a ∈ A, Loves(a, b) both require at least one satisfying pair.

In each case the same pairs are considered, and only their order of consideration changes. This does not justify swapping an existential and a universal quantifier.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
