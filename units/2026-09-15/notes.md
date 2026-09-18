# CSC110: Defining functions and methods

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Multiple-variable comprehensions: traversal and output

In a comprehension with several `for` clauses, Python chooses the **leftmost variable first**, then tries all choices to its right before changing that leftmost choice.

```python
[(x, y) for x in [1, 2] for y in [3, 4]]
# [(1, 3), (1, 4), (2, 3), (2, 4)]
```

Hold `x = 1` while `y` takes `3`, then `4`; only then set `x = 2` and repeat the choices for `y`. The same rule applies with three variables:

```python
[(y, x, z) for x in [2, 1] for y in [5, 10] for z in [11, 12]]
# [(5, 2, 11), (5, 2, 12), (10, 2, 11), (10, 2, 12),
#  (5, 1, 11), (5, 1, 12), (10, 1, 11), (10, 1, 12)]
```

Here `x` is chosen first, then `y`, then `z`; `z` changes fastest. The **output tuple is `(y, x, z)`**, even though the `for` clauses introduce `x`, then `y`, then `z`. Output-component order and traversal order are different decisions.

Changing only the expression before the first `for` changes what is produced, not which variable choices happen first:

```python
[x + y + z for x in [2, 1] for y in [5, 10] for z in [11, 12]]
# [18, 19, 23, 24, 17, 18, 22, 23]
```

The lecturer works through the first results and the ordering rule; the complete result lists above finish that trace for study.

## Function definitions, calls, and return

A function packages a computation so it can be reused without retyping its implementation. The mathematical rule \(f(x)=x^2\) becomes:

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

The definition has three main parts:

- **Header:** `def`, the descriptive function name, parentheses containing parameter names and annotations, `->` and the return annotation, then a colon. Separate multiple parameters with commas.
- **Docstring:** indented triple-quoted text containing the description and example calls/results. These console-style examples are **doctest examples**.
- **Body:** the indented statements that perform the computation when the function is called.

A **parameter** is a variable in the definition, such as `x`; an **argument** is a value supplied in a call, such as `3.0` in `square(3.0)`. Defining `square` makes it available; it does **not** execute its body. Call it separately to compute a result.

`return expression` is a **statement**, not an expression. It evaluates the expression, stops execution of the function body immediately, and returns that value to the caller. In `return x ** 2`, the exponentiation is the expression. Code placed after this return is unreachable; the lecturer demonstrates VS Code greying out a subsequent `y = 5`. A return statement is only valid inside a function.

Indentation is part of Python syntax. The docstring and body belong under the header; unindenting them can produce an `IndentationError` for a missing indented block and a `SyntaxError` for a return outside a function. Keep indentation consistent.

The worksheet applies this vocabulary to:

```python
def calculate(x: int, y: int) -> list:
    """Return a list containing the sum and product of x and y.

    >>> calculate(1, 2)
    [3, 2]
    """
    return [x + y, x * y]
```

There are two `int` parameters and a `list` return annotation. The original worksheet lacked the doctest; the lecturer adds the call and `[3, 2]` answer shown here.

`help(square)` displays the function’s documentation, including its docstring. To load a definition from the file for this lesson, select the code in VS Code and use **Run Python → Run Selection/Line in Python Terminal**, or **Shift+Enter**. Then call the function in that terminal.

## Type contracts and what Python actually checks

A function’s **type contract** specifies its parameter types and return type. It communicates how the function is intended to be used, but ordinary Python does **not** automatically enforce the annotations.

With the annotated `square` above:

```python
square(1)     # 1: the body can square an int
square('hi') # TypeError occurs when the body tries 'hi' ** 2
```

The second call is not rejected by a `float` annotation check. Python enters the function and encounters an unsupported operation in its body. 

Return annotations are also unchecked at runtime. Later, the lecturer temporarily replaces the body of `is_same_length(...)-> bool` with `return 1`. VS Code flags the mismatch, but Python runs it and returns the integer `1`. **Editor warnings and Python runtime exceptions are different things.** An execution that succeeds can still violate the documented contract.

For this course, include the annotations even though Python accepts a definition without them. PythonTA is mentioned as a way to help enforce types; enforcement details are deferred. Combining accepted types and `Any` are also previews, not new syntax required in this lesson.

**Classroom correction:** ordinary Python does not retain multiple same-name function definitions and select one by argument count. A later definition replaces the earlier binding. The professor corrects his earlier overloading claim after students test it. Variable-argument functions and class-related alternatives are mentioned only as future possibilities.

**Brief preview:** a function may perform useful work without returning a useful value; `-> None` is introduced for that situation.

## The Function Design Recipe: clarify, document, implement, test

Follow these five steps:

1. **Write example uses:** choose a name and concrete calls with expected results.
2. **Write the header:** specify the parameters and return type.
3. **Write the description:** explain what the function returns and the roles of its parameters.
4. **Implement the body.**
5. **Test the function.**

The first three steps check your understanding before you invest in code. They also communicate the function’s contract to another programmer—or to your future self. The instructor repeatedly says not to skip them.

Choose descriptive names, usually with lowercase words separated by underscores, as in `after_tax_cost` and `tax_rate`. Describe **what the function does**, not its arithmetic line by line. Start with **“Return …”**, rather than “This function returns …”. Mention parameter names or otherwise make their roles clear, and include requirements such as rounding.

A doctest looks like the Python console: a line beginning with `>>>` contains the call, and the next line contains the expected result. One or two normal, informative examples are usually sufficient for a simple function; choose different cases when inputs lead to meaningfully different behavior. An example that exposes a stated rounding requirement is more useful than two examples that both happen to give integers.

**Documentation examples are not a complete test suite.** Keep the docstring short and readable. Ordinary negative inputs and noninteger distances can be normal examples. Unusual/error cases still need tests somewhere, and applicable error behavior should be documented, but detailed testing techniques are deferred to Thursday. Running a few examples gives some confidence; it does not amount to comprehensive testing.

For a function without a useful returned value, a doctest can show a terminal-observable effect, such as querying changed data. A visual action such as opening a video may not fit this format.

[Course notes §2.7](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/02-functions/07-the-function-design-recipe.html) gives the same recipe and explains why documenting before implementing improves understanding and communication.

## Worked recipe: distance rounded to one decimal place

**Requirement:** take four floats representing two points `(x1, y1)` and `(x2, y2)`, and return their distance rounded to one decimal place. Keep that four-input interface rather than changing the problem to accept two point objects.

The examples help decide what the function must handle:

| Points | Expected result | What it checks |
|---|---:|---|
| `(1, 3)` and `(1, 1)` | `2.0` | Straight vertical distance |
| `(0, 0)` and `(3, 4)` | `5.0` | \(3^2+4^2=25\), then square root |
| The same point twice | `0.0` | Zero distance |
| `(0, 0)` and `(1, 1)` | `1.4` | Rounding a noninteger distance |
| Negative or fractional coordinates | Depends on points | The inputs are floats, not only positive integers |

Once the name and coordinate order are chosen, write examples as actual calls. The header must make the order `x1, y1, x2, y2` clear. A suitable description states the returned distance and rounding, without reciting the formula.

```python
def distance(x1: float, y1: float, x2: float, y2: float) -> float:
    """Return the distance between (x1, y1) and (x2, y2), rounded to one decimal.

    >>> distance(0.0, 0.0, 3.0, 4.0)
    5.0
    >>> distance(0.0, 0.0, 1.0, 1.0)
    1.4
    """
    return round(((x2 - x1) ** 2 + (y2 - y1) ** 2) ** 0.5, 1)
```

This uses the distance formula:

\[\sqrt{(x_2-x_1)^2+(y_2-y_1)^2}\]
 `** 0.5` takes the square root here. `round(number)` rounds to an integer; a second argument specifies decimal places, so `round(d, 1)` is needed for this problem.

A temporary variable is equally acceptable:

```python
# Alternative body for the same function
    d = ((x2 - x1) ** 2 + (y2 - y1) ** 2) ** 0.5
    return round(d, 1)
```

The two statements belong at the same indentation level inside the function. You can also reuse `square(x2 - x1)` and `square(y2 - y1)` instead of writing the two `** 2` expressions.

Finally, run the examples: the lecturer checks the two docstring examples, the vertical case, identical points, and a negative-coordinate case. These checks illustrate Step 5; merely writing expected results in a docstring does not show that you have executed them.

## Worked recipe: compare list lengths

The worksheet already supplies examples and a header, so the next step is the **description**, followed by the body and tests. State whether the two named lists have the same length:

```python
def is_same_length(list1: list, list2: list) -> bool:
    """Return whether list1 and list2 have the same length.

    >>> is_same_length([1, 2, 3], [])
    False
    >>> is_same_length([1, 2, 3], [2, 3, 4])
    True
    """
    return len(list1) == len(list2)
```

`len` gives each list’s length and `==` already produces the Boolean answer, so return that comparison directly. A separate conditional is unnecessary.

The lecturer also checks `[1, 2, 3]` against `[2, 3]`, which gives `False`. Changing an element to a different value or type does not change the list’s length: equal length does not mean equal contents.

The displayed implementation leaves a placeholder docstring to save demonstration time. The completed documentation above combines the spoken description with the demonstrated tests; students are still expected to write the docstring. The later temporary `return 1` is an intentional type-contract violation demonstration, not a correct solution.

## Worked recipe: one price, then a list of prices

For one product, accept a float price and a float tax rate; return the after-tax cost rounded to two decimal places. The lecture’s example is `after_tax_cost(5.0, 0.01) == 5.05`.

```python
def after_tax_cost(price: float, tax_rate: float) -> float:
    """Return the price after applying tax_rate, rounded to two decimal places.

    >>> after_tax_cost(5.0, 0.01)
    5.05
    """
    return round(price * (1 + tax_rate), 2)
```

The description gives the behavior, rather than saying “multiply price by …”. Splitting the calculation into intermediate variables is also acceptable. The instructor skips live testing here only because of time, and explicitly says **you should test your code**.

The next problem accepts a **list of prices, assumed to be floats**, plus a tax rate, and asks for the total after-tax cost rounded to two decimal places. Apply the full recipe. Choose examples containing multiple prices and a simple tax rate such as `0.5` or `1.0`.

The slide presents several **alternative bodies**, not a sequence of return statements:

```python
# Sum first, then apply tax:
return sum(prices) * (1 + tax_rate)
return sum(prices) + sum(prices) * tax_rate
return after_tax_cost(sum(prices), tax_rate)

# Transform each price, then sum:
return sum([price * (1 + tax_rate) for price in prices])
return sum([after_tax_cost(price, tax_rate) for price in prices])
```

Choose one approach. `sum` aggregates the prices; a comprehension transforms each item; calling the earlier helper is optional. The example also illustrates that one function can call another.

**Rounding condition:** the worksheet explicitly permits ignoring rounding errors, and the lecturer interprets this as allowing each item to be rounded before summing. That permission is specific to this exercise. Per-item rounding and rounding only the final total can differ in general. Some slide alternatives omit an explicit final `round`; a complete implementation can keep the requested two-decimal result by reusing the rounded helper:

```python
# Supplied study completion of the total-price function
def after_tax_cost_total(prices: list, tax_rate: float) -> float:
    """Return the total cost of prices after tax_rate, rounded to two decimals.

    >>> after_tax_cost_total([5.0, 10.0], 0.5)
    22.5
    >>> after_tax_cost_total([2.0, 3.0], 1.0)
    10.0
    """
    return after_tax_cost(sum(prices), tax_rate)
```

This full documented version completes one of the alternatives taught in class; its name, example calls and complete docstring are supplied for study.

## Methods, their arguments, and receiver notation

A **method** is a function associated with a particular data type. All methods are functions, but not all functions are methods. The standalone functions defined so far in this course are **top-level functions**. Many builtins work with several types—for example, `abs` with integers and floats, and `len` with several collection types.

The lecture first shows method calls using the type name:

```python
str.lower('Hello')                # 'hello'
str.split('hello world')          # ['hello', 'world']
set.union({1, 2}, {3})            # {1, 2, 3}
list.count([1, 2, 3], 1)          # 1
list.count([1, 2, 3, 1, 1], 1)    # 3
```

- `lower` returns a lowercase string.
- With no separator supplied, `split` separates on **whitespace**, not just one literal space. The displayed help also says its default mode discards empty strings.
- `union` combines set elements; sets do not use `+` for this operation. Their display order is not a meaningful sorted order. The lecturer cautions against calling that order “random”: it depends on Python’s internals.
- `count` needs the value to search for. The initial `list.count([1, 2, 3])` fails with `TypeError` because that argument is missing. `count` reports occurrences of a value; `len` reports total length.

For the rest of the course, prefer **receiver notation**: put the first object argument before the dot, then pass the remaining arguments inside parentheses.

| Type-name form | Preferred receiver form |
|---|---|
| `str.lower(s)` | `s.lower()` |
| `str.split(s)` | `s.split()` |
| `list.count(items, 1)` | `items.count(1)` |
| `set.union(set1, set2)` | `set1.union(set2)` |

Python uses the receiver’s type to find the method. Do not repeat the receiver inside the parentheses. Keep the call parentheses even when no additional arguments are needed: `s.lower()` is the call. The slide also illustrates `list.append(l, 10)` becoming `l.append(10)`; details of mutation are for later.

[Course notes §2.4](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/02-functions/04-methods.html) explains the same notation. As its reference example shows, ask for method documentation with a qualified name such as `help(str.lower)`.

## Worksheet: combine indexing, comprehensions, and methods

With `wish = 'Happy Birthday'`, the lecturer works through:

```python
wish[0].lower() + wish[6].lower()  # 'hb'
```

Index `0` gives `'H'`; index `6` is the seventh character, `'B'`. Each is converted to lowercase, then the two strings are concatenated.

The worksheet also displays the following questions. **The answers in this table are supplied study answers** for rows that were not taken up aloud. Use `set1 = {1, 2, 3}` and `set2 = {2, 4, 5, 10}`.

| Expression | Result |
|---|---|
| `wish.lower()` | `'happy birthday'` |
| `set1.union(set2)` | `{1, 2, 3, 4, 5, 10}` |
| `set1.intersection(set2)` | `{2}` |

Union includes elements in either set; intersection keeps elements in both, as explained in [course notes §2.4](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/02-functions/04-methods.html). A different displayed set order is acceptable. The worksheet says to work by hand first, then check in the console, using Appendix A.2 as a reference if needed.

The next question is solved in class:

```python
strings = ['David', 'Tom', 'cool']
[x.upper() for x in strings]  # ['DAVID', 'TOM', 'COOL']
```

The comprehension visits each string and calls its `upper` method. `[str.upper(x) for x in strings]` is equivalent, but the receiver form is preferred.

The last displayed task asks for a complete function using the Function Design Recipe. **The following wrapper is a supplied study solution; the lecturer finishes the comprehension but does not complete this full function in class.**

```python
def uppercase_strings(strings: list) -> list:
    """Return a new list containing each string in strings in uppercase.

    strings is a list of strings.

    >>> uppercase_strings(['David', 'Tom', 'cool'])
    ['DAVID', 'TOM', 'COOL']
    >>> uppercase_strings([])
    []
    """
    return [x.upper() for x in strings]
```

The examples and header establish the required list input/output, the description states the result, and the body reuses the taught comprehension. Execute both example calls to finish the testing step.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
