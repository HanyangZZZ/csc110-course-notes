# CSC110: Function specifications and property-based testing — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Specifications and correctness

6 practice marks

For max_length(strings: set[str]) -> int with precondition strings != set() and the description 'return the maximum string length': (a) identify its input requirements and two result requirements; (b) explain caller and implementer responsibilities; (c) explain why an error on max_length(set()) alone does not refute correctness.

Answer requirements:

- Judge correctness relative to the stated specification.

<details><summary>Hints</summary>

An annotation is part of the contract.

An implication with a false premise is vacuously true.

</details>

<details><summary>Worked solution and rubric</summary>

(a) The argument must be a set whose members are strings, and the set must be nonempty. The result must be an integer and equal the greatest length among those strings. (b) The caller must supply valid arguments. The implementer may assume those requirements and must produce the specified result for every valid call. (c) An empty set violates the precondition, so this call is outside the guaranteed domain. Its error does not show a failure on a valid input.

- 1 mark: set of strings requirement.
- 1 mark: nonempty requirement.
- 1 mark: integer result and maximum-length meaning.
- 1 mark: caller obligation.
- 1 mark: implementer assumption and conditional result obligation.
- 1 mark: invalid empty input gives no correctness counterexample.

</details>

### Collection annotations

8 practice marks

Give suitable specific annotations for (a) ordered song names with possible repeats, (b) one RGB colour represented as a list, (c) a row of such colours, (d) float keys mapped to lists of integers. Explain the outer structure in (a) and the difference between (b) and (c). Finally, give two reasons to use plain list instead.

Answer requirements:

- Do not use tuples for the requested RGB representation.

<details><summary>Hints</summary>

Each additional collection layer contributes another pair of type brackets.

The code may not care about element types.

</details>

<details><summary>Worked solution and rubric</summary>

(a) list[str]; a list preserves order and permits repeated song names. (b) list[int]; one colour contains integer components. (c) list[list[int]]; the row contains colours, each already a list of integers. (d) dict[float, list[int]]. Plain list is appropriate when contents can have different types or when the function does not depend on their types, as with counting elements. The RGB annotation alone does not specify component count or range.

- 1 mark each for the four annotations (4 total).
- 1 mark: playlist order and repeats.
- 1 mark: one colour versus outer row explanation.
- 1 mark: heterogeneous contents reason.
- 1 mark: contained types irrelevant reason.

</details>

### Precondition syntax and model limitations

6 practice marks

Write a Preconditions block for the demonstrated pay function: integer hours between 0 and 23 inclusive, start strictly before end, and float pay_rate at least 15.0. Give separate and combined hour expressions. Explain whether start=end and a 22-to-6 shift are valid under your block.

Answer requirements:

- Treat 15.0 as an exercise constraint, not current law.
- Use the strict ordering explicitly requested here.

<details><summary>Hints</summary>

Python supports chained comparisons.

The inputs contain hours but no dates.

</details>

<details><summary>Worked solution and rubric</summary>

A combined block is:
```text
Preconditions:
  - 0 <= start < end <= 23
  - pay_rate >= 15.0
```
The separate hour conditions are start < end, 0 <= start <= 23, and 0 <= end <= 23. They must all hold. start=end fails the strict inequality, so zero-hour work is excluded by this specification. A 22-to-6 shift also fails ordering; without dates this model does not represent crossing midnight.

- 1 mark: correct heading and dash-prefixed syntax.
- 1 mark: all three separate hour restrictions.
- 1 mark: equivalent combined chain.
- 1 mark: pay_rate >= 15.0.
- 1 mark: equality excluded by strict ordering.
- 1 mark: overnight limitation explained.

</details>

### Writing a generated property test

6 practice marks

Write the imports and a Hypothesis test for the property that is_even(2*x) is true for every integer x. Explain what supplies x and which x exposes an implementation that incorrectly returns False only when value == 4.

Answer requirements:

- Use integers and given.
- Assume is_even is available.

<details><summary>Hints</summary>

The generated parameter and the function argument are different expressions.

</details>

<details><summary>Worked solution and rubric</summary>

```python
from hypothesis import given
from hypothesis.strategies import integers

@given(x=integers())
def test_is_even_2x(x: int) -> None:
    assert is_even(2 * x)
```
integers() supplies a strategy, and given connects its generated values to parameter x. The failing generated value is x=2, because the call then supplies 4. The strategy samples inputs, so writing this test does not guarantee the bug will be found in every run.

- 1 mark: both imports.
- 1 mark: matching given keyword and function parameter.
- 1 mark: integers() strategy.
- 1 mark: correct assertion calling is_even.
- 1 mark: explanation of strategy/decorator generation.
- 1 mark: x=2 with multiplication explanation.

</details>

## Review quiz

### Original self-test: interpreting contract behavior

8 practice marks

Without automatic checking, a bad pay call returns a number and max_length(set()) raises an error inside max. After adding PythonTA, a float rate 10.0 is rejected for violating the stated minimum-rate precondition. Explain (a) why the first two outcomes differ without Python enforcing the contracts, (b) the import and decorator needed, and (c) whose responsibility the last violation is, including the necessary assumption.

Answer requirements:

- This is an original self-test, not a verified assessment format.
- Do not infer that a numeric result makes an invalid call valid.

<details><summary>Hints</summary>

Separate body execution from checking the specification.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Docstrings and annotations do not automatically validate calls. Arithmetic can run on invalid pay inputs and return a number. An empty max_length call instead reaches max with an empty iterable, and that operation raises an error. Neither outcome is caused by Python enforcing the written precondition. (b) Use from python_ta.contracts import check_contracts and put @check_contracts immediately above the function header. (c) Supplying 10.0 violates pay_rate >= 15.0, so the caller failed its obligation, assuming the specification and its checker expressions are written properly.

- 1 mark: ordinary annotations/docstrings are not automatically enforced.
- 1 mark: explains numeric arithmetic despite invalid inputs.
- 1 mark: identifies underlying max failure.
- 1 mark: distinguishes operation failure from contract checking.
- 1 mark: correct import.
- 1 mark: decorator and placement.
- 1 mark: caller responsibility.
- 1 mark: correctly written specification/check assumption.

</details>

### Original self-test: postcondition diagnosis

7 practice marks

A decorated calculate_pay has checks $return_value >= 0 and $return_value <= 24 * pay_rate. Its body is accidentally (start - end) * pay_rate. For calculate_pay(3, 5, 15.0), compute the result and identify the failing check. Then explain why restoring a result within both bounds does not alone establish correct pay. Give one wrong result that would pass both checks for this call. Also explain what `$return_value` denotes.

Answer requirements:

- Use the demonstrated valid-input specification.
- Your passing-but-wrong result must satisfy both inequalities.

<details><summary>Hints</summary>

The correct pay is two hours times 15.0.

Bounds do not identify a unique result.

</details>

<details><summary>Worked solution and rubric</summary>

The buggy body computes (3 - 5) * 15.0 = -30.0, violating the nonnegative check. The upper bound is 24 * 15.0 = 360.0, so the negative result does not violate that upper bound. Correct pay is 30.0. A result such as 0.0 passes 0.0 >= 0 and 0.0 <= 360.0 but is not 30.0. These checks are necessary partial properties, not a full statement of the exact calculation. $return_value is PythonTA's docstring notation for the returned result.

- 1 mark: -30.0 calculation.
- 1 mark: identifies nonnegative failure.
- 1 mark: upper bound 360.0 and its check not failing.
- 1 mark: correct pay 30.0.
- 1 mark: valid passing-but-wrong example.
- 1 mark: explains partial versus full postcondition.
- 1 mark: explains $return_value.

</details>

### Original self-test: multiple generated parameters

7 practice marks

Using the lecture's divides implementation, write a Hypothesis test for ∀n,d ∈ ℤ, d | d*n. Explain why d=0 is permitted and why replacing the call to divides with a built-in arithmetic assertion would not test this implementation.

Answer requirements:

- Assume given, integers, and divides are already imported.
- Do not restrict the strategies to positive integers.

<details><summary>Hints</summary>

For d=0, what is the second argument?

A test must execute the code whose bugs it is intended to find.

</details>

<details><summary>Worked solution and rubric</summary>

```python
@given(n=integers(), d=integers())
def test_divides_multiple(n: int, d: int) -> None:
    assert divides(d, d * n)
```
When d=0, d*n=0, so the call is divides(0, 0); its explicit zero branch returns True. The predicate avoids an actual modulo-by-zero operation. A built-in arithmetic assertion that never calls divides cannot reveal a defect in the divides body, even if it expresses a related mathematical fact.

- 1 mark: two matching parameters.
- 1 mark: unrestricted integer strategy for each.
- 2 marks: correct call and argument order in assertion.
- 1 mark: d=0 gives divides(0,0) and True.
- 1 mark: distinguishes predicate branch from division/modulo by zero.
- 1 mark: explains why the implementation must be called.

</details>

### Original self-test: domain and counterexamples

9 practice marks

A test uses unrestricted integers for n and d and asserts not divides(d, n) or d <= n. Evaluate it at (d,n)=(2,-4) and (1,0). State the intended domain, repair the strategies, and give an equivalent conditional-assertion body with an explanation of the false-premise case.

Answer requirements:

- Use both lecture counterexamples.
- Do not replace the implication with an unconditional assertion.

<details><summary>Hints</summary>

An implication fails for a true premise and false conclusion.

</details>

<details><summary>Worked solution and rubric</summary>

For (2,-4), divides(2,-4) is True and 2 <= -4 is False, so the assertion is False. For (1,0), divides(1,0) is True and 1 <= 0 is False, so it also fails. The intended domain is positive integers for both variables:
```python
@given(n=integers(min_value=1), d=integers(min_value=1))
```
An equivalent body is:
```python
if divides(d, n):
    assert d <= n
```
When divisibility is false, the assertion is skipped and the execution passes, matching vacuous truth. Neither listed counterexample refutes the positive-domain property because neither has positive n.

- 2 marks: both truth values and failure for (2,-4).
- 2 marks: both truth values and failure for (1,0).
- 1 mark: positive domain for both variables.
- 1 mark: both corrected strategies.
- 1 mark: correct conditional body.
- 1 mark: false-premise/vacuity explanation.
- 1 mark: counterexamples are outside intended domain.

</details>

## Challenge quiz

### Strength of properties versus input sampling

8 practice marks

A programmer replaces is_even with a function that returns True for every integer. Would the property test assert is_even(2*x) find this bug, even if it could check every integer x? Would the lecture's unit test at 3 find it? Explain two different reasons why a property test can pass despite an incorrect implementation, using this example and the lecture's value-4 bug.

Answer requirements:

- Stay within the taught evenness properties and tests.
- Distinguish a weak property from a missed generated input.

<details><summary>Hints</summary>

The property says what happens to even arguments, not odd ones.

</details>

<details><summary>Worked solution and rubric</summary>

The always-True implementation satisfies is_even(2*x) for every integer x, so even exhaustive checking of that property would not expose it. The property tests only required True answers on even arguments; it does not prohibit True answers on odd arguments. The unit test assert not is_even(3) fails because the function returns True at 3. This illustrates an insufficient property: wrong behavior outside what the property constrains remains possible. The value-4 bug illustrates a different limitation: the property would reject the implementation at x=2, but a generated sample may omit that input. Better coverage cannot repair a logically insufficient property by itself, and a suitable property can still miss a bug through sampling.

- 1 mark: property passes for always-True implementation.
- 1 mark: holds even with exhaustive x checking.
- 2 marks: explains even-only constraint and missing odd behavior.
- 1 mark: unit assertion at 3 fails, with reason.
- 1 mark: identifies property insufficiency.
- 1 mark: identifies sampling omission for value-4 bug.
- 1 mark: gives x=2 and distinguishes the two limitations.

</details>

### Constructing a counterexample to sufficient checking

8 practice marks

Suppose calculate_pay always returns 0.0 but retains the lecture's valid-input preconditions, float return annotation, and the two partial postconditions. Show that it passes these postconditions for every valid input, then refute its correctness with a concrete valid call. Explain why a contract checker need not report this implementation error.

Answer requirements:

- Use 0 <= start < end <= 23 and pay_rate >= 15.0.
- Separate the complete intended specification from the explicit partial checks.

<details><summary>Hints</summary>

A nonnegative lower bound accepts zero.

Use the demonstrated 3-to-5 shift.

</details>

<details><summary>Worked solution and rubric</summary>

The returned 0.0 has type float and satisfies 0.0 >= 0. For every valid rate, pay_rate >= 15.0, so 24 * pay_rate is positive and 0.0 <= 24 * pay_rate also holds. Thus these checks always pass on valid calls. But calculate_pay(3,5,15.0) should return (5-3)*15.0 = 30.0, not 0.0. This valid call refutes correctness with respect to the full pay description. A checker enforcing only the annotation and those partial expressions has no failing condition to report: it does not automatically turn the English description into a full executable specification.

- 1 mark: float annotation satisfied.
- 1 mark: nonnegative condition satisfied.
- 2 marks: upper bound holds for all valid rates, with reasoning.
- 1 mark: concrete call meets all preconditions.
- 1 mark: correct result 30.0 contrasted with 0.0.
- 1 mark: valid counterexample refutes full correctness.
- 1 mark: checker only enforces supplied executable checks.

</details>

### Vacuity and ineffective tests

8 practice marks

A broken divides function returns False for every pair. Consider two tests: A asserts divides(d, d*n) for generated integers; B asserts not divides(d,n) or d <= n for generated positive integers. Which detects this implementation, and why? Would B become adequate merely by generating more positive inputs? Relate your answer to the false-premise behavior of implication.

Answer requirements:

- Evaluate the actual returned Boolean values.
- Do not add future testing tools.

<details><summary>Hints</summary>

If divides always returns False, what does not divides return?

</details>

<details><summary>Worked solution and rubric</summary>

A fails for any generated pair, because its assertion calls divides and receives False, although d should divide d*n, including the zero case under this implementation's intended specification. B always passes: not divides(d,n) is True, so the or-expression is True regardless of d <= n. This is vacuous satisfaction of the implication through a false premise. Generating more positive inputs cannot expose this particular bug through B, since the same Boolean evaluation occurs for every pair. B is a useful necessary property of correct divisibility behavior, but alone it does not require the function ever to recognize a genuine divisor. The two tests therefore constrain different aspects of the behavior.

- 1 mark: A fails.
- 1 mark: explains assertion receives False.
- 1 mark: d divides d*n is expected, including zero case.
- 1 mark: B passes.
- 1 mark: not False makes the disjunction True.
- 1 mark: identifies vacuous truth.
- 1 mark: more generated inputs cannot fix this weakness.
- 1 mark: explains complementary constraints/insufficiency of B alone.

</details>

### Reviewing a proposed test repair

8 practice marks

After seeing the counterexamples (d,n)=(2,-4) and (1,0), a student proposes @given(n=integers(min_value=0), d=integers(min_value=1)) for the divisor-bound implication. Evaluate the repair. Supply a failing allowed input if one exists, give the lecture's final repair, and explain what a subsequent passing run establishes and does not establish. Also explain why keeping both equivalent assertion forms cannot allow one successful assertion to rescue a failure.

Answer requirements:

- Use the given divides behavior at zero.
- Preserve the distinction between nonnegative and positive integers.

<details><summary>Hints</summary>

min_value=0 still includes zero.

Every executed assertion must succeed.

</details>

<details><summary>Worked solution and rubric</summary>

The repair is insufficient: n=0 is still permitted, and d=1 is permitted. divides(1,0) is True but 1 <= 0 is False, so the implication fails. The final lecture repair is @given(n=integers(min_value=1), d=integers(min_value=1)), which generates positive rather than merely nonnegative values for both variables. A passing run supports the property for its sampled inputs and indicates that the demonstrated out-of-domain cases have been excluded; it is not proof over all positive integers or proof of every aspect of divides. If both equivalent assertion forms remain, any failure makes the execution fail. Tests do not combine assertion outcomes by accepting at least one success.

- 1 mark: proposed repair is insufficient.
- 1 mark: gives allowed n=0,d=1.
- 1 mark: explains true premise and false conclusion.
- 1 mark: correct strategy repair for both parameters.
- 1 mark: positive versus nonnegative distinction.
- 1 mark: passing run is sampled evidence.
- 1 mark: neither universal proof nor full implementation proof.
- 1 mark: all executed assertions must succeed; no rescue by another pass.

</details>
