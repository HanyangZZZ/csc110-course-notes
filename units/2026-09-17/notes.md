# CSC110: Scope, debugging and testing

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Local variables and separate memory tables

A variable's **scope** is the part of a program where it can be accessed. Parameters are variables too: each call assigns its argument values to the function's parameters. Parameters and variables assigned inside a function are **local variables** of that function.

```python
def square(x: float) -> float:
    """Return x squared.

    >>> square(3.0)
    9.0
    >>> square(2.5)
    6.25
    """
    return x ** 2
```

`square(10)` returns `100`, but afterwards typing `x` in the console raises `NameError` if no console variable `x` exists. The function's local `x` has not become a console variable.

Our value-based memory model now has a separate table for `__main__` (the console/top level) and for each active function call. Highlight the table whose code is currently running. For:

```python
n = 10.0
result = square(n + 3.5)
```

| Moment | `__main__` | `square` |
| --- | --- | --- |
| Before `square` returns | `n = 10.0`; no `result` yet | **Active:** `x = 13.5` |
| After assignment finishes | **Active:** `n = 10.0`, `result = 182.25` | No longer accessible |

When a function returns, its table disappears from the active model. On paper, grey it out or leave it inactive. If the console variable were also named `x`, the two `x` variables would still be separate: after `x = 10` and `square(x + 3.5)`, the console's `x` remains `10`.

**Course simplification:** treat these tables as separate and pass information through arguments and return values. Python does allow functions to access module/global variables; the course avoids relying on that feature. A function does not gain access to another function's local variables merely because that function called it.

The first worksheet makes the distinction concrete. Assume no console variables `x` or `y` have been defined:

```python
def add(x: int, y: int) -> int:
    """Return the sum of x and y.

    >>> add(5, 6)
    11
    """
    return x + y
```

```pycon
>>> eleven = add(5, 6)
>>> twelve = x + 7
Traceback (most recent call last):
    ...
NameError: name 'x' is not defined
```

The **second** assignment fails. `eleven` was successfully assigned `11`; `x` and `y` are local to `add`, so this console expression cannot use that `x`.

Reference: [Course notes §2.3: Local Variables and Function Scope](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/02-functions/03-function-scope.html).

## Use the debugger to inspect a paused program

The debugger lets you pause execution, inspect variables, and follow the calls that brought the program there.

1. Click beside an executable line number to set a red **breakpoint**; click again to remove it.
2. Start **Debug Python File**, use the **Run and Debug** sidebar, or choose **Run → Start Debugging / F5**. If prompted, select **Python File** to debug the current file. The ordinary **Run Python File** button does not stop at breakpoints.
3. At a pause, the yellow highlighted line is the **next statement to execute**. It has **not run yet**. Inspect **Variables** for the current local values.

A function-body breakpoint is reached when that function is called, not merely when its definition is encountered. A breakpoint needs executable code: for the worksheet's final marker, the lecturer added `a = 1` after the calculation. Pausing **before** this extra assignment lets us inspect Marker D without creating `a` yet; the assignment would bind `a` if executed.

| Control | What it does |
| --- | --- |
| Continue | Run to the next breakpoint, or finish if there are none. |
| Step Over | Execute the next statement, including any function call, without ordinarily displaying the called function's individual steps. |
| Step Into | Enter a function call on the highlighted line, even without a breakpoint inside it. |
| Step Out | Finish the current function and return one level to its caller. The remaining statements still execute. |
| Restart / Stop | Start debugging again / end the debugging session. |

**Breakpoints still take priority while stepping.** In the demonstration, Step Over entered `square` because a breakpoint remained there. Removing that breakpoint made Step Over skip the internal inspection as intended.

The **Call Stack** lists the active sequence of calls: the current function is at the top; its callers appear below. The entry labelled **module** corresponds to the top level, or `__main__` in our diagrams. Click a caller to inspect its variables. This changes the frame you are viewing, not the program's progress. For example, while `square` has `x = 10`, the top-level frame may have no ordinary data variables yet.

## Running a file and experimenting in the Python terminal

The two execution methods used in class have different purposes:

| Whole file: Run or Debug | Selection: Shift+Enter |
| --- | --- |
| Executes the file from top to bottom. | Sends the selected lines to the Python terminal, effectively copying and pasting them. |
| The process finishes when the file finishes. | The interactive session remains available, with definitions and variables for further experiments. |
| Bare expression results are not automatically displayed. | Expression results are normally displayed by the console. |

For example, the console displays `15` after `5 + 10` and `100` after `square(10)`. A file containing only the latter call computes its result without displaying it. Use:

```python
print(square(10))
```

to display `100` during a whole-file run or debugging session. `return` supplies a function's result; `print` explicitly displays a value. Run the file to check the program's complete behavior; use the interactive terminal to explore definitions and try additional expressions. Imports and file paths also depend on how the code is run, as the testing example below shows.

## Worksheet: trace nested calls at Markers A–D

Reuse `square` above. **This September 17 worksheet rounds to two decimal places**:

```python
def calculate_distance(x1: float, y1: float,
                       x2: float, y2: float) -> float:
    """Return the distance between points (x1, y1) and (x2, y2),
    rounded to two decimal places.

    >>> calculate_distance(0.0, 0.0, 3.0, 4.0)
    5.0
    """
    # MARKER B
    dx_squared = square(x1 - x2)
    dy_squared = square(y1 - y2)
    return round((dx_squared + dy_squared) ** 0.5, 2)

p = [3.0, 4.0]
# MARKER A
distance = calculate_distance(0.0, 0.0, p[0], p[1])
# MARKER D
```

Marker C is immediately **before `return x ** 2` inside `square`**. Comments mark the moments to inspect; they do not themselves execute an operation.

| Marker | Active table | `__main__` table | `calculate_distance` table | `square` table |
| --- | --- | --- | --- | --- |
| A: before calling `calculate_distance` | `__main__` | `p = [3.0, 4.0]` | — | — |
| B: before the first call to `square` | `calculate_distance` | `p = [3.0, 4.0]` | `x1 = 0.0`, `y1 = 0.0`, `x2 = 3.0`, `y2 = 4.0` | — |
| C: first visit, before `square` returns | `square` | `p = [3.0, 4.0]` | Same four parameters | `x = -3.0` |
| D: after the outer assignment | `__main__` | `p = [3.0, 4.0]`, `distance = 5.0` | Returned | Returned |

An assignment evaluates its **entire right-hand side before assigning the result**. Consequently, `distance` is absent at A, B, and C. At the first C, `dx_squared` is also absent: Python is still evaluating `square(x1 - x2)`.

A call first evaluates its arguments, then binds those values to the parameters before running the body. Here, `x1 - x2` evaluates to `0.0 - 3.0`, or `-3.0`, which becomes `square`'s local `x`. After that call returns `9.0`, `dx_squared` receives `9.0`.

The second call reaches C again with `x = -4.0`; then `dx_squared` already exists, while `dy_squared` still awaits its result. The worksheet asks for **the first** visit to C. Finally, the two squares give `9.0 + 16.0`, whose square root is `5.0`; returning it completes the assignment to `distance`.

**Correction from class:** bind by the signature's order, `x1, y1, x2, y2`. The debugger displayed names in the different order `x1, x2, y1, y2`, causing a temporary transcription error that was corrected. The order of rows in your memory table does not matter; each name's value does.

## Run docstring examples automatically

Doctest examples are both **documentation** and simple executable tests. Instead of manually copying each example into the console, put this at the bottom of the Python file:

```python
if __name__ == '__main__':
    import doctest
    doctest.testmod()
```

Place runnable demonstration/testing commands in this bottom block; the guard's full explanation was deferred.

Run the file. **Passing doctests are silent by default.** For explicit results, use `doctest.testmod(verbose=True)`. When the lecturer temporarily changed `square` to `return x ** 3`, both examples failed: `3.0` produced `27.0` instead of `9.0`, and `2.5` produced `15.625` instead of `6.25`. Failure reports show expected and actual output.

Doctest compares console-style text, so format examples carefully: `>>>` introduces the expression, expected output goes directly below it, and relative indentation matters. Adding spaces before expected output can cause a failure.

For sets and dictionaries, test **equality** rather than a particular printed ordering. The lecture's dictionary example is:

```pycon
>>> result = divides_by({2, 3, 20}, 2)
>>> result == {2: True, 3: False, 20: True}
True
```

This checks which numbers are divisible by `2`. Dictionary equality ignores insertion order, while printed dictionaries preserve it; equal dictionaries can therefore display differently. Set equality also avoids depending on display order. [Python dictionary documentation](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict).

Doctests should remain clear examples of typical use. A large collection of edge cases belongs in a separate test suite.

Reference: [Course notes §2.8: doctest and pytest](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/02-functions/08-testing-functions-1.html).

## Import modules and explore their functions

A Python code file, such as `something.py`, is a **module**. You can run it directly or import it to use its definitions elsewhere. `import math` finds and loads the module and introduces the name `math`; a Python file's top-level code runs when it is first imported.

Before importing, `math.sin(1)` fails because `math` is not defined. Afterwards, use **module-qualified names** such as `math.sin`, identifying which module supplies the function. In the console, evaluating `math.sin` alone displays the function; adding parentheses and an argument calls it:

```python
import math

math.sin(1)                         # approximately 0.84147
math.dist([0, 0], [3.0, 4.0])       # 5.0
math.sqrt(1.4)                      # approximately 1.18322
math.sqrt(2)                        # approximately 1.41421
math.asin(1)                        # approximately 1.57080
math.asin(0)                        # 0.0
```

`math.dist` takes **two point sequences**, unlike our four-parameter distance function. `math.sqrt` is another way to calculate a square root besides raising a number to `0.5`. `math.asin` is inverse sine; `math.asin(2)` raises `ValueError: math domain error`, illustrating that library functions still have valid input domains.

Use `dir(math)` to list available names, including `sin`, `cos`, and `tan`, and `help(math.dist)` or `help(math.asin)` to inspect a function's documentation and expected arguments. Use them with Python, course, and your own modules.

The alternative `from even_function import is_even` was briefly demonstrated: it lets you write `is_even(...)` directly. The lecture otherwise uses `import even_function` followed by `even_function.is_even(...)`.

Reference: [Course notes §2.5: Importing Python Modules](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/02-functions/05-importing-modules.html).

## Write unit tests with assert and pytest

A **unit test** checks a function's behavior on a particular input. A **test suite** collects tests for one function or a small related group. The lecture starts with this module:

```python
# even_function.py
def is_even(n: int) -> bool:
    """Return whether n is even."""
    return n % 2 == 0
```

Put tests in a separate file whose name starts with `test_`. Each test is also a function named `test_...`, with a descriptive docstring, no parameters here, and return annotation `-> None`. It needs no doctest examples or explicit return statement.

```python
# test_even_function.py
import even_function


def test_is_even_zero() -> None:
    """Test is_even on zero."""
    arg = 0
    expected = True
    assert even_function.is_even(arg) == expected


def test_is_even_even() -> None:
    """Test is_even on a positive even input."""
    assert even_function.is_even(6) == True


def test_is_even_odd() -> None:
    """Test is_even on a positive odd input."""
    assert even_function.is_even(9) == False
```

The last two tests condense the lecture's repeated argument/expected-variable pattern. `assert expression` evaluates the expression: if true, execution continues silently; if false, Python raises **`AssertionError`**. Merely defining a test does not run it. Calling it manually works, but `pytest` automates calling all the test functions and reporting results.

Temporarily changing the implementation to test oddness makes the zero test fail. Restoring `n % 2 == 0` makes it pass. The later tests for `6` and `9` produced three passing tests overall. Let assertion failures fail the test; suppressing the error defeats the check.

At the bottom of the test file, the lecture uses:

```python
if __name__ == '__main__':
    import pytest
    pytest.main(['lectures/week02/test_even_function.py'])
```

`pytest` finds test functions, runs them, and reports how many were collected, passed, or failed, with details such as the failed `False == True` comparison. Its list argument can contain multiple test-file paths. This library was included in the course's software setup.

**Paths in the demonstrated VS Code setup:** the working directory was the `csc110` project root. The filename alone produced “file or directory not found”; `lectures/week02/test_even_function.py` correctly names the file relative to that root. Other setups may use a different working directory.

| How the code is executed | Import and call used in the demonstration |
| --- | --- |
| Run the test file beside `even_function.py` | `import even_function`; `even_function.is_even(0)` |
| Paste selection into the terminal at the project root | `import lectures.week02.even_function`; `lectures.week02.even_function.is_even(0)` |

Use **dots in import names** and **slashes in file-path strings**. Pasted code loses its original file location, which explains the different import prefix. Follow the method assumed by tutorial instructions, or adjust the import when switching methods.

One demonstration showed a console import error followed by a passing pytest report: pytest loaded the test file separately and its import worked there. That passing report did not undo the earlier console error.

## Choose useful cases; apply them to the homework

Choose representative inputs from categories relevant to the function:

| Guideline | Examples |
| --- | --- |
| **Size** | Empty collections, one item, several items, or much larger collections. |
| **Order** | For an ordered collection such as a list, vary the order of its elements. |
| **Boundary** | Test on and around a change in behavior: for absolute value, `-1`, `0`, and `1`. |
| **Dichotomy** | Cover contrasting categories: for `is_even`, both even and odd inputs. |

These are rough guides, not a universal checklist. Categories can overlap. Passing more meaningful tests increases confidence, but generally does **not prove correctness**: most functions have too many possible inputs to test exhaustively. A tiny domain is an exception—for Boolean AND, all four pairs can be checked.

The final worksheet exercise was left for homework: add a doctest and unit tests for `rank_absolute_values`. The following is an **added study solution**, not a solution presented by the professor. The implementation is from the worksheet; the doctest and tests are supplied here.

```python
# rank_functions.py
def rank_absolute_values(numbers: set) -> list:
    """Return the absolute values of numbers in non-decreasing order.

    >>> rank_absolute_values({-3, 0, 2, 3})
    [0, 2, 3, 3]
    """
    absolute_values = [abs(number) for number in numbers]
    return sorted(absolute_values)
```

```python
# test_rank_functions.py
import rank_functions


def test_empty() -> None:
    """Test the empty input."""
    assert rank_functions.rank_absolute_values(set()) == []


def test_single_negative() -> None:
    """Test one negative number."""
    assert rank_functions.rank_absolute_values({-4}) == [4]


def test_mixed_signs_zero_and_equal_magnitudes() -> None:
    """Test signs, zero, sorting, and repeated absolute values."""
    assert rank_functions.rank_absolute_values({-3, 0, 2, 3}) == [0, 2, 3, 3]
```

These cases cover sizes and sign categories. Although the input set contains distinct numbers, `-3` and `3` produce equal absolute values, so the output list contains two `3`s. “Non-decreasing” allows such ties. List equality checks the required sorted output order; there is no input-list ordering to vary because the input is a set.

## Compare computed floats with a tolerance

Computers store floats with finite binary precision. Some values, including decimal `0.3`, cannot be represented exactly—just as `1/3` requires infinitely many decimal digits. Consequently:

```pycon
>>> 0.1 + 0.2
0.30000000000000004
>>> 0.1 + 0.2 == 0.3
False
```

A test that adds costs `[0.2, 0.1, 0.3]` and expects exact equality with `0.6` can therefore fail despite the intended calculation being correct. Where appropriate, prefer integers: represent money as integer cents, then **divide by `100`** to express dollars. This avoids float error during the integer calculations.

For computed float results in tests, use `pytest.approx`:

```python
import math
import pytest


def test_floats() -> None:
    """Test approximate comparisons after floating-point arithmetic."""
    actual = 0.1 + 0.2
    expected = 0.3
    assert actual == pytest.approx(expected)
    assert pytest.approx(expected) == actual
    assert math.isclose(actual, expected)
```

Both sides work for `approx`; placing the expected value on the right matches the usual actual-versus-expected pattern. The cost example similarly becomes `assert actual == pytest.approx(0.6)`.

**Import correction:** when test functions themselves use `pytest.approx`, put `import pytest` at the top of the file. The lecture moved it there after a `NameError`. A main-guarded import is skipped when pytest imports the test module; an `if` block does **not** create a separate local scope. [Python main-guard documentation](https://docs.python.org/3/library/__main__.html).

`math.isclose(a, b)` is another way to ask whether two floats are sufficiently close, including outside pytest; `math.isclose(1.233, 1.233000001)` returns `True`. These tools use tolerances, whose defaults can be adjusted. Their comparison rules differ; neither rounds both values to fixed decimal places.

Use approximate comparison when arithmetic may introduce representation error. Exact comparisons such as `0.5 == 0.5` are fine. A single unit test can contain multiple assertions, as above: it passes only if all pass; any assertion failure fails the test and stops that test's remaining statements.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
