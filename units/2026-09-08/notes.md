# CSC110: Python expressions and Boolean logic

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## The big picture: code, language, interpreter

**A program is a set of instructions a computer can execute.** Source code is those instructions written in a programming language. The Python **language** supplies the rules for writing code; the Python **interpreter** is the program that runs it. “Run” and “execute” mean the same thing here. The computer follows the instructions as written; it does not infer what we intended.

CSC110 combines programming with mathematical reasoning. Two different questions matter: **correctness** asks whether a program does what it should; **efficiency** asks how much time it takes. Proofs and detailed efficiency analysis come later. Testing checks how a program behaves in particular runs; the course will also use mathematical proof to justify correctness beyond observing successful tests.

**Modeling a new problem domain** means deciding how to represent a real problem and approach it as a program. The work also includes design principles, documenting and testing code, and explaining in English what software should do.

Python was chosen because its syntax is approachable, it has useful libraries, and it is widely used. Today's practical goal is to enter expressions, predict their results, and distinguish numeric and Boolean data.

## Run Python in a console or a file

| Use | Best for | How to start |
|---|---|---|
| **Console** | Quick calculations, experiments, and testing small pieces of code | VS Code → **Terminal → New Terminal**; enter `python` on Windows or `python3` on Mac/Linux. |
| **`.py` file** | Saving, organizing, and submitting a program | Open the file in VS Code and click the play button at the top right. |

The terminal runs commands; starting Python inside it gives you the Python console. The prompt `>>>` means Python is ready for an expression:

```pycon
>>> 2 ** 4
16
```

Here, `2 ** 4` is the expression you enter and `16` is its output. **Do not type the `>>>` prompt itself.** In a file, write the code without console prompts or copied output lines. Save work you need later in a file; do not rely on a console session as your saved assignment.

## Values, types, and comments

A value's **type** tells you what kind of data it is. The three types introduced today are:

| Type | Examples | Meaning |
|---|---|---|
| `int` | `-45`, `0`, `6` | Integers: whole numbers, including negatives. |
| `float` | `-15.67`, `0.1`, `2.0` | Floating-point numbers, used to represent real-number values approximately. |
| `bool` | `True`, `False` | Exactly two values, used for yes/no answers. Capitalization matters. |

**Value and type are different questions.** `2` and `2.0` represent equal numeric values, but their Python types differ. Use `type(expression)` to inspect the result's type:

```pycon
>>> type(2)
<class 'int'>
>>> type(2.0)
<class 'float'>
>>> type(2 ** 0.5)
<class 'float'>
>>> type(True)
<class 'bool'>
```

Here Python computes `2 ** 0.5` before reporting its result type. In the mixed `int`–`float` arithmetic demonstrated here, the result is a `float`: `1 - 1.0` gives `0.0`.

Python also supports very large integers; a long integer literal does not become a `float` just because it has many digits.

**Comments explain code to a human reader.** `#` starts a comment outside a string; the rest of that line is ignored. Code before it still runs:

```pycon
>>> # this is a note
>>> 1 + 1 # this is a note
2
```

The comment-only line produces no result. It does not produce `0` or `None`.

## Arithmetic: quotient, remainder, and grouping

| Operator | Meaning | Example → result |
|---|---|---|
| `+`, `-`, `*` | Addition, subtraction, multiplication | `1 + 1` → `2` |
| `**` | Exponentiation | `2 ** 4` → `16`; `2 ** 0.5` → approximately √2 |
| `/` | Ordinary division | `5 / 2` → `2.5` |
| `//` | Floor division: round the quotient down | `5 // 2` → `2` |
| `%` | Remainder (also called modulo/modulus) | `5 % 2` → `1` |

For these integer-operand division examples, `/` returns a `float`, while `//` and `%` return `int`s. Do not confuse **the quotient** with **the remainder**: five contains two complete groups of two, with one left over. With float operands, `//` can return a float; the rounding rule still applies.

**Evaluate the grouping, not just the symbols.** For the positive-number expressions used here, powers come before multiplication, which comes before addition. Parentheses can change that order. The course notes explain the lecture's sample calculation:

```text
1 + 2 ** 3 * 5
= 1 + 8 * 5
= 41

(1 + 2) ** (3 * 5)
= 3 ** 15
= 14348907
```

So the sample comparison `(1 + 2 ** 3 * 5) == (1 + ((2 ** 3) * 5))` is `True`: both sides are `41`. Replacing the right side with `(1 + 2) ** (3 * 5)` makes it `False`.

## Comparisons: False is an answer, not an error

A comparison evaluates the relationship between values and returns a `bool`.

| Operator | Question | Example → result |
|---|---|---|
| `<` | Less than? | `5.0 < 5.0` → `False` |
| `<=` | Less than or equal to? | `5.0 <= 5.0` → `True` |
| `>` | Greater than? | `5.0 > 5.0` → `False` |
| `>=` | Greater than or equal to? | `5.0 >= 5.0` → `True` |
| `==` | Equal? | `5.0 == 5` → `True` |
| `!=` | Not equal? | `1 != 1` → `False` |

The equality boundary matters: `<` excludes equality; `<=` includes it. Numeric comparisons can mix `int` and `float`, so different types do not automatically mean unequal values.

**Use `==` for equality.** `5 == 4` is valid and returns `False`. `5 = 4` is invalid syntax and raises `SyntaxError`; single `=` has a different role, covered later. The error points to `5` on the left: that number is not permitted in that position. This is a problem with how the instruction is written, whereas `False` is the valid result of an unequal comparison.

Likewise, `True` is a Boolean literal, but lowercase `true` is a name. If that name has not been defined, evaluating it raises `NameError`.

For output questions, distinguish `True` from `<class 'bool'>`: the first is a value; the second is what `type(True)` displays.

Numeric examples do not mean every pair of Python types can be compared meaningfully. A set/integer example was deferred.

## Boolean logic: not, and, inclusive or

`not` is a **unary operator**: it takes one Boolean value and reverses it. `and` and `or` are **binary operators**: each takes two Boolean values. `and` requires both to be true; `or` requires at least one to be true.

| `p` | `q` | `not p` | `p and q` | `p or q` |
|---|---|---|---|---|
| `True` | `True` | `False` | `True` | `True` |
| `True` | `False` | `False` | `False` | `True` |
| `False` | `True` | `True` | `False` | `True` |
| `False` | `False` | `True` | `False` | `False` |

**Work out the comparisons first, then combine their Boolean results.** The same logic applies whether those comparisons involve integers, floats, or both: the logical operators receive the resulting Boolean values.

```pycon
>>> (1 == 1) and (2 == 3)
False
>>> (1 == 1) or (2 == 2)
True
>>> not (5 == 4)
True
>>> not (not (5 == 4))
False
```

Python's `or` is **inclusive**: both inputs being true still gives `True`. Two negations restore the original Boolean value. Repeated `or` combines more conditions the same way: `1 == 1 or 2 == 2 or 3 == 4` is `True`.

Parentheses help readers see the intended grouping. `not 5 == 4` is also valid and returns `True`; parentheses are not required for this particular expression.

## Floats: mathematical equality can fail in Python

A computer has finite storage, so a `float` cannot represent every real number exactly. A calculation can therefore differ slightly from its exact mathematical answer.

The lecture's final sample is the important example:

```pycon
>>> (2 ** 0.5) ** 2
2.0000000000000004
>>> (2 ** 0.5) ** 2 == 2
False
```

Mathematically, squaring √2 gives 2. In this computation, Python works with a finite approximation; the final value is not exactly `2`, so `==` returns `False`. The lecture also demonstrated that `0.1 + 0.2` is not exactly `0.3`.

The analogy was writing `1/3` as `0.333…`: finite space cannot hold infinitely many digits. **Course-note clarification:** floats use a finite binary representation, and some values, such as `2.5`, are exact. The lesson is “not every value is exact,” not “every float is wrong.”

When predicting an output, follow Python's computation rather than replacing it with an ideal real-number identity. Methods for handling numerical error are a later topic.

## Optional aside: negative division

“Round down” means toward smaller numbers, **not toward zero**:

```pycon
>>> 5 / -2
-2.5
>>> 5 // -2
-3
>>> 5 % -2
-1
```

The quotient `-2.5` rounds down to `-3`. The negative remainder was also demonstrated; a fuller explanation of `%` was deferred. Learn the positive division examples first. The recording does not clearly establish whether these negative cases are assessed.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
