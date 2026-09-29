# CSC110: Function specifications and property-based testing

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Function specifications, correctness, and the caller and implementer contract

A **function specification** describes which inputs are valid and what the function must return for those inputs. A **predicate** is a statement or expression that is true or false. **Preconditions** are predicates that valid inputs must satisfy; **postconditions** describe what must be true of the result, usually in relation to the inputs. Multiple preconditions must all hold, and multiple postconditions must all hold: each group is a logical AND.

An **implementation** is the code in the function body. It is **correct with respect to its specification** when, for every input satisfying the preconditions, its return value satisfies the postconditions. Correctness therefore depends on the specification, not just whether the code runs without raising an error.

The lecture slide writes `∀x ∈ D, Pre(x) ⇒ Post(x)`, where `D` is the set of possible inputs and `∀` means “for every.” Read this slide’s `Post(x)` as shorthand for the required outcome associated with input `x`, not as a claim that the input is the returned value. More explicitly, if `f` is the implementation and the postcondition relates input and output, the meaning is `∀x ∈ D, Pre(x) ⇒ Post(x, f(x))`. If the postcondition concerns only the output, it can instead be written `Post(f(x))`. Inputs and outputs need not generally have the same type.

The implication `Pre(x) ⇒ ...` performs **logical filtering**: only inputs for which `Pre(x)` is true impose an obligation on the implementation. When `Pre(x)` is false, the implication is **vacuously true**—true because its premise is false, regardless of its conclusion. Consequently, the correctness contract gives no promised result for an invalid call. Such a call might return a meaningless result or raise an error; neither outcome alone disproves correctness on valid inputs.

| Contract part | Function implementer: writes the body | Function caller: supplies arguments |
|---|---|---|
| Preconditions | May assume them when implementing the function. Invalid cases do not need handling under this specification. | Must supply arguments satisfying them. |
| Postconditions | Must make them true for valid calls. | May rely on them for valid calls to a correct implementation. |

Preconditions make implementation easier but calling more restrictive. For example, if a pay function requires ordered, valid clock hours and a sufficient pay rate, its body need not add branches to handle negative rates or reversed hours.

A general contract error could be the caller’s or implementer’s responsibility. A violated precondition is the caller’s responsibility **assuming the specification and its checking code are written properly**. A violated postcondition on valid inputs indicates that the implementation failed its obligation. An incorrectly written contract is a different problem, so a message about a contract should be interpreted rather than blamed mechanically.

**Supplementary clarification — 4.1 Function specification and preconditions:** the official reading explicitly distinguishes the specification from the implementation and emphasizes that valid inputs satisfy both the type annotations and all additional preconditions. At this stage, specifications concern return values; other function effects are later material.

## Writing preconditions and understanding max_length

Parameter type annotations are themselves preconditions: they restrict valid arguments. A return type annotation is a postcondition: it restricts what the function should return. Additional restrictions go in the docstring, which is the descriptive string at the beginning of the function body.

The lecture’s initial example used the general annotation `set`:
```python
def max_length(strings: set) -> int:
    """Return the maximum length of a string in strings.

    Preconditions:
      - strings != set()
    """
    return max({len(s) for s in strings})
```
Here `set()` creates an empty set, so `strings != set()` requires a nonempty set. The expression `{len(s) for s in strings}` collects the lengths of its members, and `max` selects the greatest length. Duplicate lengths do not matter when finding the maximum. If there are no strings, there is no greatest length for this implementation to return, which motivates the nonempty restriction.

The initial `set` annotation requires the outer collection to be a set, but does not encode the requirement that its members be strings. The description supplies that intent; the next section shows how to express it in the annotation itself. The return annotation `int` specifies the result’s type, while the description specifies its meaning: the maximum string length. Merely returning some integer would not satisfy that meaning.

Use the exact heading `Preconditions:` followed by separate dash-prefixed lines. Whenever possible, write each restriction as a valid Python expression evaluating to `bool`, rather than only in English. This is more precise and enables automatic checking. All listed restrictions apply together.

Postconditions are usually implicit in the description of what the function returns. The instructor did not require a separate `Postconditions:` block by default. Explicit partial postconditions can nevertheless be useful checks, as shown later.

## Specific collection annotations and choosing a data representation

A **homogeneous collection** contains values of the same type. A **heterogeneous collection** contains values of different types. Square brackets let an annotation specify the contained types, not just the outer collection.

| Annotation | Meaning and example |
|---|---|
| `set[T]` | A set whose elements have type `T`; a set of strings has type `set[str]`. |
| `list[T]` | A list whose elements have type `T`; `[1, 2, 3]` has type `list[int]`. |
| `dict[T1, T2]` | A dictionary with keys of type `T1` and associated values of type `T2`; string keys mapped to integers have type `dict[str, int]`. The key type comes first. |

`T`, `T1`, and `T2` stand for the types you choose, such as `str` or `int`; they are not literal requirements to write those names in an ordinary annotation.

The worksheet’s completed value classifications were `list[int]`, `set[str]`, `dict[float, bool]`, and `dict[float, list[int]]`. The last annotation is **nested**: the dictionary keys are floats, each value is a list, and the elements of each such list are integers. It does not describe lists as keys or floats inside those lists.

Choosing an annotation also requires choosing a suitable collection:
- **Study playlist:** `list[str]`. Strings represent song names; a list preserves their order and allows repeated songs.
- **One RGB24 colour represented as a list:** `list[int]`. The instructor initially wrote `list[list[int]]`, then corrected it because the question asks for one colour. A row of pixels would add an outer list and have type `list[list[int]]`. The textbook’s tuple representation is a different representation from the list requested here. The annotation alone does not express the number of colour components or their permitted range.
- **Grocery names and quantities:** `dict[str, int]`. Each food name is associated with a quantity. This answer assumes whole-number quantities, not quantities such as 1.5 tomatoes.
- **Distinct names with no order:** `set[str]`. A set represents uniqueness without a meaningful sequence order.

The course guidance is to use specific collection annotations for homogeneous collections. Use a general `list`, `set`, or `dict` when the collection may be heterogeneous or the operation does not depend on the contained types. For example, `len` counts elements without needing to know whether they are integers or strings. These are two separate reasons for a general annotation; a function can ignore element types even when a particular input happens to be homogeneous.

**Supplementary clarification — 4.2 Collection type annotations:** the reading improves the earlier header to `def max_length(strings: set[str]) -> int:` while retaining `strings != set()`. This is an improvement to the recorded generic header, not its original spelling. `set[str]` requires strings as members, but still permits an empty set, so it does not replace the nonempty precondition.

## Worked specification: calculate_pay and its limitations

The exercise’s function has parameters `start: int`, `end: int`, and `pay_rate: float`, and returns a `float`. The hours use a 24-hour clock from 0 through 23 inclusive. The exercise requires `pay_rate >= 15.0`; the instructor explicitly identified this wage value as outdated. It is this programming exercise’s constraint, not a statement of current minimum-wage law.

The final classroom choice was:
```python
def calculate_pay(start: int, end: int, pay_rate: float) -> float:
    """Return pay for the given work hours and hourly rate.

    Preconditions:
      - 0 <= start < end <= 23
      - pay_rate >= 15.0
    """
    return (end - start) * pay_rate
```
The body subtracts the start hour from the end hour to obtain the number of hours worked, then multiplies by the hourly rate. Because valid calls already meet the restrictions, the implementation need not repeat them as validation branches.

The combined hour condition is equivalent to requiring all three of these:
```python
start < end
0 <= start <= 23
0 <= end <= 23
```
Python supports **chained comparisons**. For example, `0 <= start <= 23` means `0 <= start and start <= 23`. In the longer chain, `0 <= start`, `start < end`, and `end <= 23` together also imply the remaining individual bounds: the end must be above a nonnegative start, and the start must be below an end no greater than 23.

Important specification choices and limitations:
- `start < end` was a reasonable inferred restriction, not an explicit requirement in the supplied description. The instructor retained it for the demonstration but said omitting it in a test situation would be acceptable if the description did not state it.
- Allowing `start == end` would represent zero hours and could reasonably be chosen under a different specification. The demonstrated final chain excludes it.
- This model does not support overnight shifts. For instance, starting at 22 and ending at 6 violates the final ordering requirement; the inputs also contain no dates to say which day each hour belongs to. The instructor suggested dates/date-time information as a possible extension, without teaching that implementation.

**Supplementary examples — Exercise 1: Reviewing preconditions and type annotations:** the worksheet gives `calculate_pay(3, 5, 15.5) == 31.0`, since two hours at 15.5 per hour gives 31.0, and `calculate_pay(9, 21, 22.0) == 264.0`, since twelve hours at 22.0 gives 264.0. These reading examples illustrate the same calculation.

## Runtime contract checking with PythonTA

Ordinary Python does not automatically enforce restrictions written in docstrings or type annotations. The lecture demonstrated three different consequences of invalid calls:
- `calculate_pay(-5, 30, -12.0)` still produced a numeric result. Executing the arithmetic is not evidence that these are valid work hours or a valid rate.
- `max_length(set())` reached the underlying `max` operation and raised a `ValueError` because its iterable was empty. This was not Python reading and checking the documented precondition.
- Passing a list of integers to `max_length` was not rejected at the type annotation. Execution instead failed when the body tried to apply `len` to an integer.

PythonTA, previously used for style checking, can also add runtime contract checks:
```python
from python_ta.contracts import check_contracts

@check_contracts
def calculate_pay(start: int, end: int, pay_rate: float) -> float:
    """Return pay for the given work hours and hourly rate.

    Preconditions:
      - 0 <= start < end <= 23
      - pay_rate >= 15.0
    """
    return (end - start) * pay_rate
```
An **import-from statement** imports a particular name from a module, so the code can use `check_contracts` directly. A **decorator** is written immediately above a function header with `@`; it adds behavior to that function. Here it arranges for PythonTA to check contracts when the function is called. The instructor applied it to both `calculate_pay` and `max_length`.

In the live demonstration, checks initially did not work as intended. The instructor removed old commented alternatives from the docstring’s precondition block, after which the demonstrated violations were detected. Keep that block clean and follow the required heading-and-dashes format: PythonTA reads those lines to find the conditions. This observed repair does not establish that every comment in every docstring always breaks PythonTA.

The successful checks distinguished several invalid calls:
- A rate of `10.0` violated `pay_rate >= 15.0`.
- An end hour before the start violated the combined hour condition.
- An integer rate `10` was rejected for not matching the annotated `float` type in this demonstration. Numeric arithmetic being possible does not itself satisfy the checked type contract.

Contract checking detects violations for calls actually made. It does not make invalid inputs valid or establish correctness for all possible valid inputs.

## Explicit postconditions: useful checks, not a complete specification

PythonTA can check a docstring block headed `Postconditions:` with dash-prefixed expressions. Its special name `$return_value` refers to the returned result. This is PythonTA docstring syntax, not an ordinary Python variable name.

The pay demonstration added these checks:
```text
Postconditions:
  - $return_value >= 0
  - $return_value <= 24 * pay_rate
```
For the demonstrated valid inputs, hours worked are positive and the rate is positive, so the result must be nonnegative. A function limited to work within one day should also not produce pay exceeding 24 hours times the hourly rate. This is a safe upper bound, not a claim that 24 hours can be represented by the specific 0-through-23 inputs.

The instructor deliberately changed the body from `(end - start) * pay_rate` to `(start - end) * pay_rate`. Calling `calculate_pay(3, 5, 15.0)` then produced `-30.0`: the reversed subtraction gives `-2` hours, and multiplying by `15.0` preserves that negative sign. PythonTA detected the violated nonnegative-result postcondition. The body was then restored to `(end - start) * pay_rate`.

These are **partial postconditions**: necessary properties of a correct result, but not enough to characterize the entire correct result. A violation reveals a definite problem for a valid call. Passing the bounds does not show that the precise pay is correct, because many numbers lie between those bounds. Writing a full executable postcondition may be difficult without duplicating the calculation itself.

The default course practice is not to require explicit postcondition blocks. The docstring’s description still specifies the required behavior. Add partial checks when useful, rather than confusing them with a complete replacement for that description. For now, the lecture focused on properties of returned values; checking changes to mutable arguments was deferred to later in the course.

## Unit tests, property-based tests, and the is_even demonstration

A **unit test**, in this lecture’s comparison, checks expected behavior for one specific input: an input–output case. A **test suite** is a collection of tests. Doctest and pytest are tools for running tests, rather than guarantees of complete coverage.

Correctness requires the specification to hold for every valid input. Usually the input space is far too large to enumerate through unit tests. There are exceptions: if the only possible input is one Boolean value, testing both `True` and `False` can exhaust that domain. Integers and many collection inputs do not offer that small finite case list.

A **property-based test** checks a general rule about behavior on many generated inputs instead of listing only particular expected input–output pairs. Such a rule may be a partial postcondition, such as nonnegative pay, or an input–output relationship. Preconditions determine which generated inputs belong in the valid domain; they are not a substitute for the behavior being tested.

Consider the correct evenness predicate:
```python
def is_even(value: int) -> bool:
    return value % 2 == 0
```
`%` computes a remainder, so this returns `True` exactly when the integer has remainder zero on division by 2. The lecture then deliberately introduced a bug:
```python
def is_even(value: int) -> bool:
    if value == 0:
        return False
    return value % 2 == 0
```
The two unit assertions `assert is_even(2)` and `assert not is_even(3)` still passed. They check the answers at 2 and 3, neither of which enters the buggy zero branch. Thus passing these tests does not establish that zero or other untested values are handled correctly.

The property chosen was `∀x ∈ ℤ, is_even(2*x)`: every integer multiple of two is even, including zero and negative multiples. Here `ℤ` denotes the integers. The generated parameter `x` is not necessarily the argument supplied to `is_even`; that argument is `2*x`.

**Supplementary clarification — 4.4 Property-based testing and Hypothesis strategies:** good properties narrow down the permitted behavior of the function. One weak property can leave many wrong implementations undetected, even apart from the fact that only a sample of inputs is generated.

## Hypothesis setup, interpreting failures, and sampling limits

Writing a test parameter does not by itself tell pytest which inputs to supply:
```python
def test_is_even_2x(x: int) -> None:
    assert is_even(2 * x)
```
Without additional setup, the demonstration produced a pytest input/fixture error, not a counterexample to evenness. An annotation such as `x: int` describes an intended type; it does not generate integers for the test.

**Hypothesis** is a library that supplies generated examples to property-based tests. A **strategy** describes the kind of values to generate. The function `integers()` returns a strategy for integers. Use the `given` decorator to connect strategies to test parameters:
```python
from hypothesis import given
from hypothesis.strategies import integers

@given(x=integers())
def test_is_even_2x(x: int) -> None:
    assert is_even(2 * x)
```
The keyword `x` in `@given` must match the test parameter. The decorator arranges for the assertion to be checked with generated integer values. An `assert` fails when its expression is false; if the test finishes without an assertion failure or other error, that execution passes.

With the deliberate bug at zero, the property test failed while the original two unit tests passed. Hypothesis reported a **falsifying example**, meaning a concrete generated input for which the assertion failed: `x=0`. This makes `2*x` equal to zero, and the buggy function returns `False`. The displayed `assert False` was the test runner explaining the evaluated failure, not a replacement source-code assertion.

The instructor next moved the bug to `value == 4`. Several runs passed all three tests, and a later run failed. To expose this bug, the property test must generate `x=2`, because it calls `is_even(2*x)`. Generating `x=4` instead would test 8. The lecture corrected this distinction explicitly.

Generated testing samples inputs; it does not check every integer. The demonstration described generation as random while noting that common small inputs such as zero are also tried. A rare or specific failing case can be missed. Repeating a passing run is not a proof, and there is no guarantee that another run will find a particular bug. A concrete failure, however, can decisively refute a claimed universal property when the input is in its stated domain.

Strategies can be constrained to the intended domain, as with `integers(min_value=1)` for positive integers. When using multiple parameters, supply a strategy for each. The instructor said the two warnings seen in these particular pytest/Hypothesis demonstrations could be ignored; that guidance is not a rule to ignore unrelated warnings.

For test organization, implementation and tests normally belong in separate files; the demonstration combined them for simplicity. If using a `pytest.main` call with a file path, that path must point to the actual test file on your computer, not the instructor’s path.

## Testing divides: function calls, multiple strategies, and zero

The exercise generalized evenness to **divisibility**. The notation `d | n` means “d divides n”; in the code, `d` is the first argument and `n` is the second. For nonzero `d`, the implementation checks whether division leaves remainder zero:
```python
def divides(d: int, n: int) -> bool:
    """Return whether d divides n."""
    if d == 0:
        return n == 0
    else:
        return n % d == 0
```
`divides(3, 9)` returns `True`, because 9 is a multiple of 3. `divides(3, 10)` returns `False`, because a remainder remains. The zero branch returns `True` for `divides(0, 0)` and `False` for `divides(0, n)` when `n` is nonzero. It avoids evaluating `%` with a zero divisor.

The **divides relation** is not the same operation as ordinary numerical division. Asking whether zero divides a number is supported by this predicate even though evaluating division by zero is invalid. The fuller mathematical definition and proofs were deferred to the next lecture; the supplied implementation explicitly includes this case now.

The first property was `∀n ∈ ℤ, 2 | 2n`:
```python
@given(n=integers())
def test_two_divides_multiple(n: int) -> None:
    assert divides(2, 2 * n)
```
A multiple of two should be divisible by two. Crucially, the assertion must call **the function under test**, here `divides`. Calling `is_even` or asserting something about built-in division would not execute the `divides` implementation and therefore could not expose bugs in it. The demonstrated run passed, supporting this property for its sampled inputs, not proving correctness.

The next property generalized the fixed divisor 2 to any integer divisor: `∀n, d ∈ ℤ, d | d*n`.
```python
@given(n=integers(), d=integers())
def test_divisor_divides_multiple(n: int, d: int) -> None:
    assert divides(d, d * n)
```
Both parameters have their own strategy, and both may be zero or negative. For nonzero `d`, `d*n` is a multiple of `d`, giving remainder zero. For `d=0`, the second argument also becomes zero, and the explicit branch returns `True`. Therefore there is no need to exclude zero from this property’s test. This demonstrated run also passed.

## Testing implications and preserving the positive-integer domain

The final property was:

`∀n, d ∈ ℤ⁺, d | n ⇒ d <= n`.

`ℤ⁺` means the positive integers, so both `n` and `d` must be at least 1. The property says that **if** a positive divisor divides a positive integer, that divisor cannot exceed the integer. It does not say that every positive `d` divides every positive `n`. The lecture tested this property and deferred its formal proof.

An implication `p ⇒ q` is equivalent to `not p or q`: it fails only when the premise `p` is true and the conclusion `q` is false. This gives the final corrected test:
```python
@given(n=integers(min_value=1), d=integers(min_value=1))
def test_positive_divisor_bound(n: int, d: int) -> None:
    assert not divides(d, n) or d <= n
```
Both strategy restrictions matter because the stated mathematical domain restricts both variables. A comment or docstring saying “positive” would not constrain generation; the strategies must implement that restriction.

An equivalent body uses a conditional:
```python
if divides(d, n):
    assert d <= n
```
If `divides(d, n)` is true, the conclusion must hold or the assertion fails. If it is false, the assertion is skipped and this execution passes, matching a vacuously true implication. The instructor left both equivalent forms in the demonstration. Multiple assertions may appear in one test, but **none may fail** for that execution to pass; one passing assertion does not cancel a failing one.

Initially, the instructor deliberately left both strategies as unrestricted `integers()`. Some runs passed, but the claim is false on that larger domain:
- **Classroom counterexample:** `d=2, n=-4`. `divides(2, -4)` is `True`, while `2 <= -4` is `False`. A true premise and false conclusion make the implication false. The correct comparison is that 2 is greater than -4.
- **Hypothesis counterexample:** `d=1, n=0`. `divides(1, 0)` is `True`, while `1 <= 0` is `False`. Thus merely excluding negative values would not be enough; zero also supplies a failure.

These are counterexamples to the unrestricted claim, not to the intended positive-integer property, because their `n` values are outside the intended domain. The initial passing runs had missed the failing combinations. A later run found the zero example.

The final repair used `integers(min_value=1)` for **both** parameters, and the displayed run passed six tests. This removes the demonstrated out-of-domain failures and matches the intended property. Passing generated cases remains sample evidence, not a universal proof. The lecture’s next topic, proofs, was presented as another way of establishing correctness.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
