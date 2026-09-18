# CSC110: Scope, debugging and testing — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Separate local variables from the caller

5 practice marks

```python
def square(x: float) -> float:
    return x ** 2

x = 4.0
result = square(x + 1.0)
```
(a) [3 marks] Execution pauses just before `square` executes its return statement. Give the value of `x` in each active frame. Has `result` been assigned?

(b) [2 marks] After the call finishes, give the two module-level variable values and say whether the `square` frame remains active.

<details><summary>Hints</summary>

A parameter belongs to its function call. An assignment finishes after its right-hand side finishes.

</details>

<details><summary>Worked solution and rubric</summary>

(a) The module frame has `x = 4.0`; the active `square` frame has `x = 5.0`. `result` is not yet assigned: Python is still evaluating `square(x + 1.0)`.

(b) The module has `x = 4.0` and `result = 25.0`. The call returned `5.0 ** 2`, and its `square` frame is no longer active. The two bindings named `x` were separate, so the call did not change the module's `x`.

- (a) Module x=4.0 (1); square x=5.0 (1); result is not yet assigned (1).
- (b) Both final module values correct (1); square frame is no longer active (1).

</details>

### Choose the debugger action

5 practice marks

```python
def twice(n: int) -> int:
    return n * 2

answer = twice(6)
print(answer)
```
(a) [2 marks] A breakpoint on `answer = twice(6)` is ignored by **Run Python File**. Which execution mode should you use? When that line is highlighted in the debugger, has the assignment already executed?

(b) [2 marks] Starting at that highlighted line with no breakpoint inside `twice`, name the action to inspect its body and the action to execute the call without inspecting its body.

(c) [1 mark] If a breakpoint is added on `return n * 2`, can Step Over still pause inside `twice`? Explain.

<details><summary>Hints</summary>

A highlighted debugger statement is the next statement to execute. Consider separately whether a call executes and whether you inspect its body.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Use **Debug Python File** (or Start Debugging / F5 with the current Python-file configuration). The highlighted assignment has not executed yet.

(b) **Step Into** enters `twice`; **Step Over** executes its call and assignment without stopping to inspect the body when no breakpoint intervenes. Step Over still runs the function.

(c) Yes. An active breakpoint on the return statement can interrupt Step Over and pause inside `twice` before that return executes.

- (a) An appropriate Debug action (1); highlighted assignment has not executed (1).
- (b) Step Into for inspecting the body (1); Step Over for executing the call without inspecting the body (1).
- (c) Yes, because an active breakpoint can interrupt Step Over (1).

</details>

### Repair a dictionary doctest

5 practice marks

```python
def labels() -> dict:
    """Return the two labels.

    >>> labels()
    {'a': 1, 'b': 2}
    """
    return {'b': 2, 'a': 1}
```
Assume `doctest.testmod()` runs with its default options.

(a) [2 marks] Give the actual displayed dictionary and explain why this doctest fails even though the dictionaries are equal.

(b) [2 marks] Rewrite its two doctest lines to compare dictionary values without requiring a particular key display order.

(c) [1 mark] After the repair, change the runner call so it reports passing tests too.

<details><summary>Hints</summary>

Doctest compares text. A dictionary equality expression produces one Boolean result.

</details>

<details><summary>Worked solution and rubric</summary>

(a) The actual display is `{'b': 2, 'a': 1}`. Dictionaries preserve insertion order, so this literal's keys display in the order they were inserted. Dictionary equality ignores the ordering of matching key-value pairs, but doctest's default comparison checks the displayed text. The expected text has the keys in the opposite order.

(b)
```pycon
>>> labels() == {'a': 1, 'b': 2}
True
```

(c)
```python
doctest.testmod(verbose=True)
```
This reports passing examples as well. Without verbose output, a successfully run set of passing doctests is normally silent.

- (a) Actual display with b before a (1); text comparison can fail despite dictionary equality (1).
- (b) Valid equality expression using the required dictionary (1); expected True on the next line with correct doctest formatting (1).
- (c) doctest.testmod(verbose=True) (1).

</details>

### Write a test for decimal arithmetic

5 practice marks

`costs.py` contains:
```python
def add_costs(first: float, second: float) -> float:
    return first + second
```
Assume `costs` and `pytest` are importable.

(a) [4 marks] Write a complete `test_costs.py` containing top-level imports and one no-argument test function with a `-> None` annotation and short docstring. Check that adding `0.1` and `0.2` is approximately `0.3`, using a module-qualified call. A runner footer is not needed.

(b) [1 mark] Why would exact `== 0.3` fail for this input?

<details><summary>Hints</summary>

Put imports needed by a test at the top of the file. Compare the actual result with pytest.approx(expected).

</details>

<details><summary>Worked solution and rubric</summary>

(a)
```python
import costs
import pytest


def test_add_costs_decimals() -> None:
    """Test adding two decimal costs."""
    assert costs.add_costs(0.1, 0.2) == pytest.approx(0.3)
```

(b) Some decimal fractions cannot be represented exactly as binary floating-point numbers. Here `0.1 + 0.2` produces `0.30000000000000004`, so exact equality with `0.3` is false. Approximate comparison allows the small representation error.

- (a) Both necessary top-level imports (1); no-argument test_ function with -> None and a useful docstring (1); qualified costs.add_costs call with the specified arguments (1); assertion compares approximately with expected 0.3 (1).
- (b) Explains representation/rounding error after float arithmetic; exact decimal expansion need not be memorized (1).

</details>

## Review quiz

### Trace both helper calls

5 practice marks

```python
def square(x: float) -> float:
    return x ** 2

def calculate_distance(x1: float, y1: float, x2: float, y2: float) -> float:
    dx_squared = square(x1 - x2)
    dy_squared = square(y1 - y2)
    return round((dx_squared + dy_squared) ** 0.5, 2)

p = [3.0, 4.0]
distance = calculate_distance(0.0, 0.0, p[0], p[1])
```
(a) [2 marks] Give all four parameter bindings in `calculate_distance`.

(b) [2 marks] Pause before `square` returns on its **second** call. Give its local `x`, the value of `dx_squared`, and whether `dy_squared` and module-level `distance` have been assigned.

(c) [1 mark] Give the final value of `distance` and identify the only frame still active after the call returns.

<details><summary>Hints</summary>

Bind arguments in header order, then distinguish the first helper call from the second.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `x1 = 0.0`, `y1 = 0.0`, `x2 = 3.0`, `y2 = 4.0`. The header order controls binding, even if a debugger lists names alphabetically.

(b) The current `square` frame has `x = -4.0`, from `y1 - y2`. The first call used `-3.0` and has already returned, so `dx_squared = 9.0`. `dy_squared` is not yet assigned because the second call has not returned; module-level `distance` is also not yet assigned because the outer call has not returned.

(c) `distance = 5.0`, and only the module (`__main__`) frame remains active. The function rounds to two decimal places; Python displays this result as `5.0`.

- (a) Four correct parameter bindings (0.5 each, 2 total).
- (b) Current x=-4.0 (0.5); dx_squared=9.0 (0.5); dy_squared unassigned (0.5); distance unassigned (0.5).
- (c) Final distance=5.0 (0.5); only the module frame remains active (0.5).

</details>

### Separate test discovery from test failures

5 practice marks

`parity.py` incorrectly implements `is_even` as `return True`. This is the complete test content of `test_parity.py`:
```python
import parity

def test_even() -> None:
    assert parity.is_even(8) == True

def check_odd() -> None:
    assert parity.is_even(7) == False

def test_sum() -> None:
    assert 0.1 + 0.2 == 0.3
```
Assume the imports work and pytest uses default discovery.

(a) [2 marks] Which functions are collected, and which pass or fail?

(b) [1 mark] Rename `check_odd` so pytest collects it. Does that test pass or fail?

(c) [2 marks] Add the import and change the assertion needed to test the decimal sum approximately. Leave the parity tests unchanged.

<details><summary>Hints</summary>

Discovery determines which functions run; an assertion determines whether a collected test passes.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Pytest collects `test_even` and `test_sum`. `test_even` passes because the incorrect implementation returns `True` for 8. `test_sum` fails with `AssertionError` because the computed float is not exactly `0.3`. `check_odd` is not collected by default.

(b) Rename it `test_odd` (or another suitable name beginning `test_`). It fails: the incorrect function returns `True`, while the expected value for 7 is `False`.

(c) Add a top-level import and replace the float assertion:
```python
import pytest

# Inside test_sum:
assert 0.1 + 0.2 == pytest.approx(0.3)
```
After both repairs, three tests are collected: `test_even` and `test_sum` pass; `test_odd` still exposes the implementation bug.

- (a) Exactly test_even and test_sum collected (1); even passes and sum fails (0.5 each, 1 total).
- (b) A valid test_ name (0.5); resulting odd test fails (0.5).
- (c) Top-level import pytest (1); correct approximate assertion with expected 0.3 (1).

</details>

### Match imports and paths to the run mode

5 practice marks

The course project contains:
```text
CSC110/
    lectures/week02/even_function.py
    lectures/week02/test_even_function.py
```
`even_function.py` defines `is_even`. A fresh Shift+Enter console has the `CSC110` root on its import search path, but not `lectures/week02`; its working directory is also `CSC110`. Assume `pytest` is already imported.

(a) [3 marks] Correct these three lines for that console:
```python
import even_function
assert even_function.is_even(7) == False
pytest.main(['test_even_function.py'])
```
(b) [1 mark] In the lecture's whole-file setup, `test_even_function.py` can use `import even_function`. Write the qualified function call for input `7` in that setup.

(c) [1 mark] Give the alternative `from ... import ...` statement and unqualified call for that whole-file setup.

<details><summary>Hints</summary>

Use dots in import names and slashes in file-path strings. The full qualifier follows the import form.

</details>

<details><summary>Worked solution and rubric</summary>

(a)
```python
import lectures.week02.even_function
assert lectures.week02.even_function.is_even(7) == False
pytest.main(['lectures/week02/test_even_function.py'])
```
The dotted name starts from the given import root; the slash-separated test path starts from the given working directory.

(b)
```python
even_function.is_even(7)
```

(c)
```python
from even_function import is_even
is_even(7)
```
Both import styles were demonstrated. The lecture usually uses module-qualified calls. These paths follow the specified course setup, not a universal rule that Python always starts in a project root.

- (a) Correct dotted import (1); matching qualified assertion (1); correct project-relative path inside pytest.main list (1).
- (b) even_function.is_even(7) (1).
- (c) Correct from even_function import is_even and matching is_even(7) call (1).

</details>

### Choose meaningful test cases

5 practice marks

`ranking.rank_absolute_values(numbers)` takes a set of integers and returns a list of their absolute values in non-decreasing order. It retains one result for each input element: different integers may have the same absolute value. Assume `import ranking` has run.

(a) [4 marks] Write four separate `assert` statements with concrete inputs and expected lists, covering these cases: an empty set; only zero; several distinct positive values; and a positive/negative pair with the same magnitude.

(b) [1 mark] If all four tests pass, do they guarantee correctness for every set of integers? Explain briefly.

<details><summary>Hints</summary>

Test collection sizes and the boundary around zero; opposite signs can produce repeated absolute values.

</details>

<details><summary>Worked solution and rubric</summary>

(a) One suitable set of tests is:
```python
assert ranking.rank_absolute_values(set()) == []
assert ranking.rank_absolute_values({0}) == [0]
assert ranking.rank_absolute_values({8, 2, 5}) == [2, 5, 8]
assert ranking.rank_absolute_values({-3, 3}) == [3, 3]
```
`set()` constructs the empty set; `{}` would be an empty dictionary. The last case must retain both results because `-3` and `3` are different input elements.

(b) No. These tests cover useful categories and increase confidence, but many other integer sets remain untested. Passing representative cases does not guarantee the implementation handles every possible input.

- (a) One mark for each valid assertion matching its requested category and expected list (4 total); accept other suitable numbers. The empty argument must be a set, and the opposite-sign case must retain both magnitudes.
- (b) No guarantee, because inputs outside these four cases remain untested (1).

</details>

## Challenge quiz

### An unfinished assignment can retain an old value

5 practice marks

```python
def square(x: float) -> float:
    return x ** 2

def combine(x: float) -> float:
    first = square(x)
    second = square(x + 1.0)
    return first + second

x = 2.0
result = -1.0
result = combine(x)
```
Execution pauses just before the **second** call to `square` executes its return. Ignore function-name bindings.

(a) [3 marks] List every bound variable and value separately in the module, `combine`, and current `square` frames. Identify any assignment target in the running calls that is not yet bound.

(b) [1 mark] With no other active breakpoints, use Step Out once. Which function are you back in, and what new binding has completed?

(c) [1 mark] After execution finishes, give the module-level `x` and `result`.

<details><summary>Hints</summary>

Reassigning an existing name does not erase its old value while Python evaluates the new right-hand side.

</details>

<details><summary>Worked solution and rubric</summary>

(a)

| Active frame | Bound variables |
|---|---|
| Module (`__main__`) | `x = 2.0`, `result = -1.0` |
| `combine` | `x = 2.0`, `first = 4.0` |
| Current `square` | `x = 3.0` |

`second` is not yet bound: its call to `square` is still computing. The module's `result` already existed, so it keeps `-1.0` until the outer assignment completes. It is not deleted or temporarily changed to `None`. The earlier `square` call has returned and is no longer active.

(b) Step Out finishes the current `square` call and returns to `combine`. Its assignment finishes with `second = 9.0`; the next statement there returns `first + second`.

(c) The module ends with `x = 2.0` and `result = 13.0` (`4.0 + 9.0`).

- (a) Correct module bindings, including old result=-1.0 (1); correct combine bindings and second unassigned (1); correct current square binding x=3.0 (1).
- (b) Back in combine with new second=9.0 binding (1).
- (c) Final module x=2.0 (0.5) and result=13.0 (0.5).

</details>

### Construct a test that exposes a mistaken formula

5 practice marks

This function should add the absolute values of all list elements:
```python
def absolute_total(numbers: list) -> int:
    return abs(sum(numbers))
```
Tests on `[]` and `[2, 4]` pass.

(a) [3 marks] Give a two-element integer list on which it fails, the required result, and the result this body actually returns.

(b) [1 mark] Replace the return statement with a correct one using `sum` and a list comprehension.

(c) [1 mark] Assuming the function is in an imported module `totals`, write an assertion using your case that passes after the repair and fails before it.

Answer requirements:

- Use a list comprehension, not a loop statement.

<details><summary>Hints</summary>

Opposite signs can cancel before abs is applied. Choose a case that distinguishes abs of a sum from a sum of absolute values.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Use `[-2, 2]`. The required result is `4`, from `abs(-2) + abs(2)`. The supplied body returns `0`, since the elements cancel to zero before `abs` runs.

(b)
```python
return sum([abs(number) for number in numbers])
```
The comprehension takes each absolute value before summing.

(c)
```python
assert totals.absolute_total([-2, 2]) == 4
```
Accept any two-element integer list with opposite nonzero signs: give its corresponding correct and actual totals. The existing empty and positive-only cases do not expose cancellation.

- (a) Valid two-element failing input (1); correct required result (1); correct actual result for that input (1).
- (b) Sum of a list comprehension applying abs to each element (1).
- (c) Correct module-qualified assertion with the same input and intended result (1).

</details>

### Repair the import and keep both checks meaningful

5 practice marks

A test file contains:
```python
def test_arithmetic() -> None:
    assert 0.1 + 0.2 == pytest.approx(0.3)
    assert 2 + 3 == 6

if __name__ == '__main__':
    import pytest
    pytest.main(['test_arithmetic.py'])
```
Pytest imports this file as a module, so the shown `if` body is skipped.

(a) [2 marks] Name the first error when the test runs and move one statement to fix it. Explain why the skipped body matters.

(b) [2 marks] After only that repair, does the whole test pass? Identify the failing assertion and correct its expected value while retaining both assertions.

(c) [1 mark] Give an alternative first assertion using `math.isclose`, including its required import.

<details><summary>Hints</summary>

A statement in the skipped footer has not run. After repairing the missing import, check each assertion separately.

</details>

<details><summary>Worked solution and rubric</summary>

(a) The first error is `NameError` for `pytest`. Move `import pytest` to the top level before the test definition, leaving the runner call in its footer. When pytest imports this module, the given guard body is skipped, so an import placed only there never creates the module's `pytest` binding. The `if` statement does not itself create a separate local scope.

(b) The whole test still fails. The approximate float comparison passes, but `assert 2 + 3 == 6` fails with `AssertionError`. Retain it with the correct expectation:
```python
assert 2 + 3 == 5
```
A test containing multiple assertions passes only if all of them pass.

(c)
```python
import math

# Inside the test:
assert math.isclose(0.1 + 0.2, 0.3)
```
This is another taught approximate comparison; it does not require changing the exact integer check.

- (a) NameError for pytest (0.5); move import pytest to module top level (0.5); explain that the import is skipped when the test module is imported (1). Do not credit a claim that if creates a local scope.
- (b) Whole test fails on the second assertion (1); correct expected integer 5 while retaining both checks (1).
- (c) import math plus correct math.isclose assertion (1).

</details>

### Small test domains can be exhausted

5 practice marks

`both(left: bool, right: bool) -> bool` should return `True` exactly when both inputs are `True`. Its current body is wrong:
```python
return left == right
```
(a) [2 marks] For each of the four Boolean input pairs, give the required result and this body's actual result.

(b) [2 marks] Write two doctest examples documenting the intended behavior: one that passes with the current body and one that fails with it.

(c) [1 mark] Assume the function always returns the same Boolean for the same inputs and accepts only Boolean inputs. If tests with correct expectations pass for all four pairs, have they checked the entire stated input domain? Explain.

<details><summary>Hints</summary>

Compare the stated meaning with equality on each Boolean pair. Distinguish an entire finite domain from representative samples.

</details>

<details><summary>Worked solution and rubric</summary>

(a)

| `left` | `right` | Required result | Actual `left == right` |
|---|---|---|---|
| `False` | `False` | `False` | `True` |
| `False` | `True` | `False` | `False` |
| `True` | `False` | `False` | `False` |
| `True` | `True` | `True` | `True` |

(b) A passing example documenting the intended behavior:
```pycon
>>> both(True, False)
False
```
A failing example documenting the intended behavior:
```pycon
>>> both(False, False)
False
```
The second example fails with the current body because it actually returns `True`; the expected result should remain `False` to expose the bug.

(c) Yes. Two Boolean choices for each of two arguments give exactly four input pairs. Under the stated assumptions, testing all four with correct expectations checks the entire input domain. This exhaustive case differs from a few representative tests over all integer sets.

- (a) Each row with both required and actual results correct (0.5 each, 2 total).
- (b) Proper doctest for any passing pair with intended expected result (1); proper doctest for (False, False) with intended expected False (1).
- (c) Yes, with explanation that the four pairs exhaust the stated deterministic Boolean-input domain (1).

</details>
