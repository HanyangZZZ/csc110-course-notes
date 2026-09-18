# CSC110: Comprehensions and function calls

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Building collections with comprehensions

The lecture builds on seven familiar types: numeric `int` and `float`, Boolean `bool`, text `str`, and collections `set`, `list`, and `dict`.

A **comprehension** builds a collection by evaluating an expression for each element of an input collection. Its mathematical model is set-builder notation:

\[
\{x^2 \mid x \in \mathbb{N}\}=\{0,1,4,9,\ldots\}.
\]

The professor uses **natural numbers starting at 0**. Read the expression as “the set of squared values, where x ranges over the natural numbers.” Python's `for` replaces the separator and `in` replaces mathematical membership:

```python
nums = {0, 1, 2, 3, 4, 5}
{x ** 2 for x in nums}  # {0, 1, 4, 9, 16, 25}
```

Python has three forms:

```python
{expression for variable in collection}             # set
[expression for variable in collection]             # list
{key_expression: value_expression for variable in collection}  # dict
```

The outer brackets determine the **output type**, independently of the input type. A list can supply a set comprehension; a dictionary can supply a list comprehension.

A list retains traversal order and repeated results; a set removes duplicates and has no meaningful element order. For the lecture's duplicate input:

```python
nums2 = [1, 2, 2, 2, 3]
[x for x in nums2]  # [1, 2, 2, 2, 3]
{x for x in nums2}  # {1, 2, 3}
```

A list comprehension over a **set** does not guarantee sorted output. The input determines traversal; changing the output brackets does not sort it.

The comprehension variable may be renamed consistently and is local to the comprehension. After `[num ** 2 for num in nums]`, evaluating `num` raises `NameError` if it had no separate definition outside the comprehension.

## Choosing the expression: values, keys, and arithmetic

Use the instructor's three-step design method:

1. Choose the **required output collection type**.
2. Write its **identity comprehension**, initially keeping each input value unchanged.
3. Change the expression before `for` to compute the desired result.

**Reciprocals.** For `numbers = {1, 2, 3, 4, 5}`, choose a set, start with `{x for x in numbers}`, then use:

```python
{1 / x for x in numbers}
# {1.0, 0.5, 0.3333333333333333, 0.25, 0.2}
```

These are floating-point results of `/`; the order shown for a set is immaterial.

**Cubes, not triples.** The worksheet instead starts with `numbers = [1, 2, 3]`. Indexing begins at zero: `numbers[0]`, `numbers[1]`, and `numbers[2]` give `1`, `2`, and `3`. Cubing those values gives `1`, `8`, and `27`: `2 * 2 * 2 = 8`.

```python
[x for x in numbers]       # identity: [1, 2, 3]
[x ** 3 for x in numbers]  # [1, 8, 27]
```

`** 3` raises to the third power; `* 3` multiplies by three.

**Dictionary keys and values.** Continue with the same input list:

```python
{x: x for x in numbers}      # identity: {1: 1, 2: 2, 3: 3}
{x: 3 * x for x in numbers}  # {1: 3, 2: 6, 3: 9}
{3 * x: x for x in numbers}  # {3: 1, 6: 2, 9: 3}
{x: 0 for x in numbers}      # {1: 0, 2: 0, 3: 0}
```

Design the key and value separately. Either expression can use the input variable or be constant. `3 * x` and `x * 3` are equivalent here, but moving the multiplication across the colon changes the associations.

A suggested alternative, `{x: x // 3 for x in numbers}`, gives `{1: 0, 2: 0, 3: 1}`: it never changes the keys. It would produce the desired inverse mapping **if the input were `[3, 6, 9]` instead**. `/` would produce floats here; `//` performs floor division.

**Translating a fraction.** For the same `[1, 2, 3]`, the mathematical set \(\{x/(x+1)\mid x\in\mathrm{numbers}\}\) becomes:

```python
{x / (x + 1) for x in numbers}  # {0.5, 0.6666666666666666, 0.75}
```

Parentheses matter: `x / x + 1` divides before adding, giving `2.0` for each of these nonzero inputs.

## Functions, arguments, and return values

A **function** packages reusable computation: name a calculation once, then use it with different inputs.

In mathematics, a function maps inputs from a set **A**, its **domain**, to outputs in a set **B**, its **codomain**. The notation is \(f:A\to B\). The lecture's example is:

\[
f:\mathbb{R}\to\mathbb{R},\qquad f(x)=x^2.
\]

Its domain and codomain are both the real numbers; `f` is the function's name and `x` represents its input. Applying it gives \(f(5)=25\), \(f(0)=0\), and \(f(-1.5)=2.25\).

Python uses similar parentheses to **call** a function:

```python
abs(-5)  # 5
```

`abs` names the function, `-5` is the **argument** passed to it, and `5` is the **return value**. Say “call `abs` with argument `-5`” or “pass `-5` to `abs`; it returns `5`.” Multiple arguments, when a function accepts them, are separated by commas.

The entire call is an expression that evaluates to its return value. Merely typing a function name, such as `len`, displays the function itself; `len([1, 2, 3])` calls it and returns `3`.

Distinguish an expression from a statement containing it:

```python
x = abs(-5)
```

`abs(-5)` evaluates to `5`; the whole line is an **assignment statement** that binds `x` to that result. Here we are calling existing functions.

## Built-in functions and what they consume

| Function | Use taught in this lecture |
|---|---|
| `abs(x)` | Absolute value of a number; `abs(1)` and `abs(-1)` both return `1` |
| `len(collection)` | Number of items |
| `sum(collection)` | Sum of a collection of numbers |
| `min(collection)`, `max(collection)` | Smallest or largest element of a non-empty numeric collection |
| `sorted(collection)` | A new list of the elements in ascending order |
| `type(value)` | The value's Python type: `type(10)` gives `<class 'int'>` |

**Check what the function consumes.** A set has already discarded duplicates before a function receives it: `len({1, 2, 3, 3})` is `3`. The demonstrated form of `sum` consumes a collection, not separate numbers to add:

```python
sum([1, 2, 3])  # 6
sum(1, 2)      # TypeError: 'int' object is not iterable
```

The error concerns the first argument: `1` cannot supply elements to sum. In this lecture, think of an **iterable** as something that supplies elements, such as a collection.

**Dictionary inputs supply keys.** Both a comprehension and these collection-consuming functions visit dictionary keys. For `d = {1: 5, 2: 6}`, `max(d)` returns `2` and `sorted(d)` returns `[1, 2]`.

The professor suggested working out how a comprehension could retrieve the **values** for sorting. A completed solution to that suggestion uses each key to look up its value:

```python
d = {1: 5, 2: 6}
[d[key] for key in d]  # [5, 6]
sorted([d[key] for key in d])  # [5, 6]
```

**String sorting is not sorting by length.** In `sorted(['asdf', 'a'])`, `'a'` comes first because the initial character matches and the shorter string then ends. This does not mean every shorter string precedes every longer string.

**Ask Python for help.** `help(min)` displays documentation about `min`; the argument is the function itself. Pressing Enter can reveal more documentation. `help()` opens the interactive help console; enter a function name there, then `q` to return to Python.

## Evaluating expressions accurately

Evaluate an assignment's right-hand side using current variable values before binding the result. The worksheet setup is:

```python
n = -5
numbers_list = [1, 10, n]
numbers_set = {100, n, 200}
```

| Variable | Value after these assignments |
|---|---|
| `n` | `-5` |
| `numbers_list` | `[1, 10, -5]` |
| `numbers_set` | `{100, -5, 200}` |

The **values column contains evaluated values, with no variable names**. `[1, 10, n]` is not an acceptable value-table answer. Preserve list order; set display order is irrelevant.

The worksheet combines calls with variables and operators. These worked answers include the short rows left for students to check:

| Expression | Value and reasoning |
|---|---|
| `abs(n)` | `5`: the absolute value of `-5` |
| `sorted(numbers_list)` | `[-5, 1, 10]`: ascending list order |
| `len(numbers_set)` | `3`: three distinct elements |
| `len(numbers_list) == n` | `False`: compare `3 == -5` after evaluating the call and variable |
| `sum(numbers_set) - n` | `300`: `295 - (-5)` adds five |

The longer row needs special care:

```python
sorted(numbers_set) + sorted(numbers_list)
# [-5, 100, 200] + [-5, 1, 10]
# [-5, 100, 200, -5, 1, 10]
```

`sorted` returns a **list even when its input is a set**. Both intermediate results are therefore lists, so `+` concatenates them. The expression sorts each collection separately; it does not globally sort the combined result.

`+` is not a universal collection-combining operation:

```python
{1, 2, 3} + [1, 2, 3]  # TypeError
{1, 2, 3} + {1, 2, 3}  # TypeError
```

Sets do not support `+`.

The worksheet says to work **by hand first**, then check in Python, and recommends Appendix A.1, the Python Built-In Function Reference.

## Using functions and range inside comprehensions

The expression before `for` can itself be a function call. The worksheet asks for a **set of absolute values** from a list of positive and negative integers. Start with `{x for x in numbers}`, then replace `x` with `abs(x)`:

```python
numbers = [-1, 2, 3]  # the worksheet's example input
{abs(x) for x in numbers}  # {1, 2, 3}
```

The set output is required because the prompt explicitly asks for a set.

For integer endpoints **m ≤ n**, `range(m, n)` supplies `m, m + 1, ..., n - 1`: the start is **inclusive**, the stop **exclusive**, and the default step is 1. There are **n − m** integers. If `m > n`, this increasing range is empty, with size 0; the lecture's count explanation applies to the increasing-endpoint case.

Its type is `range`, not `list`; it supplies inputs to a comprehension without manually listing them.

To compute reciprocals of integers 1 through 20 inclusive:

```python
[x for x in range(1, 21)]      # integers 1, 2, ..., 20
[1 / x for x in range(1, 21)]  # their reciprocals, in that order
```

The second list begins `[1.0, 0.5, 0.3333333333333333]` and ends with `0.05`, the reciprocal of 20. Checking the identity comprehension first makes the endpoints easy to inspect. `range(1, 20)` would stop at 19—the professor explicitly identified this as a common **off-by-one error**. Starting at 1 also avoids division by zero.

**Explicit assessment guidance:** this reciprocal prompt did not specify an output collection type, so the professor said a set comprehension would also be acceptable on a test. He chose a list to make the input order easy to see. That permission does not override an explicit type requirement.

## Multiple comprehension variables: combinations and order

The **Cartesian product** \(A\times B=\{(x,y)\mid x\in A,\ y\in B\}\) contains every possible pair with one element from each set. Python expresses these combinations by repeating `for` clauses:

```python
{(x, y) for x in {1, 2} for y in {10, 20}}
# {(1, 10), (1, 20), (2, 10), (2, 20)}
```

The result elements are pairs, written `(x, y)`; a third component makes a triple. A comprehension can use three input collections:

```python
{(x, y, z) for x in {1, 2} for y in {10, 20} for z in {50, 60}}
```

There are eight triples: for each of the four `(x, y)` pairs above, `z` can be `50` or `60`.

**All combinations need not mean all distinct results.** Combining two inputs in one component can produce repeated results:

```python
{(x, y + z) for x in {1, 2} for y in {10, 20} for z in {50, 60}}
# {(1, 60), (1, 70), (1, 80), (2, 60), (2, 70), (2, 80)}
```

Eight input combinations produce only **six set elements**: `10 + 60` and `20 + 50` both give `70`. Sets retain one copy of each equal result. The order used to display a set is immaterial.

**List output makes order visible.** With ordered inputs, clauses are processed left to right: the rightmost variable runs through its choices fastest.

```python
[(x, y) for x in [1, 2] for y in [10, 20]]
# [(1, 10), (1, 20), (2, 10), (2, 20)]

[(x, y) for y in [10, 20] for x in [1, 2]]
# [(1, 10), (2, 10), (1, 20), (2, 20)]
```

The first choice stays fixed while all choices to its right are tried. The output pair remains `(x, y)` in both expressions: **clause order and component order are separate choices**.

**Final ordering question.** The lecture asked which clause order produces this target while the output stays `(x, y, z)`:

```python
[(1, 1, 1), (2, 1, 1), (1, 1, 2), (2, 1, 2),
 (1, 2, 1), (2, 2, 1), (1, 2, 2), (2, 2, 2)]
```

`y` stays fixed for four entries, `z` for two, and `x` changes every entry. The answer is **y, z, x**:

```python
[(x, y, z) for y in [1, 2] for z in [1, 2] for x in [1, 2]]
```

This also shows that the same input collection can be reused for multiple variables. Verify the order in Python, as the professor encouraged.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
