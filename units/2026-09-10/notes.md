# CSC110: Data types, collections and variables

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## 1. Values, types, and expressions

A **type** categorizes data and determines which operations it supports. The basic types reviewed were `int` (integers), `float` (floating-point numbers), `bool` (`True` and `False`), and `str` (text). Use `type(...)` to inspect a value’s type; Python’s name for the string type is `str`, not `string`.

An **expression** is code evaluated to produce a value. A **literal** represents a value directly: in `4 + 5`, `4` and `5` are literals, while the whole expression produces `9`. A **statement** is an instruction. An expression can stand alone as an expression statement; not every statement is an expression.

Comparisons such as `==` and `>` produce Boolean results. For Boolean operands:
- `and` is true only when both operands are true.
- `or` is inclusive: it is true when at least one operand is true, including when both are true.
- `not` reverses the Boolean value.

For example, `not (5 * 2 > 3)` evaluates through `not (10 > 3)` to `not True`, giving `False`.

## 2. Arithmetic: track both value and type

A mathematically whole-number result is not necessarily an `int`. In the reviewed examples, `9 / 3` gives `3.0` (`float`), whereas `9 // 3` gives `3` (`int`). Floor division does not itself guarantee an integer type: `3.5 // 2` gives `1.0`.

**Explicit assessment example:** the instructor marked the statement that mixed `int`–`float` arithmetic produces a `float` as True. Use this rule for the ordinary arithmetic examples practised here.

For the sample expression
```python
12 - 4 * 5 // (3.0 ** 2) + 100
```
trace types rather than doing unnecessary arithmetic: `3.0 ** 2` produces a float; floor-dividing the integer product by that float produces a float; the remaining subtraction and addition retain a float result.

The instructor described these numeric-type questions as typical quiz/test practice. Worksheet evaluation also requires **both the resulting value and its type**.

## 3. Strings: characters, membership, and indexing

A string is a sequence of characters. String literals use matching single or double quotes; choosing the other kind of outer quote permits embedded quotes, as in `"'hello'"`. A single character is still a `str`: Python has no separate character type here.

String `==` compares contents. String `+` concatenates without inserting spaces: `'hello' + 'CSC110'` gives `'helloCSC110'`. Supply any desired space explicitly. Mixing a string and integer with `+`, as in `'b' + 1`, raises `TypeError`.

**Indexing** uses square brackets. For a nonempty string of length `n`, valid indices are `0` through `n - 1`, or `-n` through `-1` counting from the end. In `'hello'`, `[0]` and `[-5]` select `'h'`; `[-1]` selects `'o'`. Index `1` selects the second character. Indices `5` and `-6` are out of range and raise `IndexError`.

**Substring membership** requires contiguous characters: `'ban' in 'banana'` is `True`, but `'baa' in 'banana'` is `False`. The empty string `''` is contained in every string; it is not a space.

**Newline clarification:** `\n` represents one newline character. Evaluating `'hello\nworld'` in the console shows an escaped representation; `print('hello\nworld')` displays two lines without enclosing quotes. The newline belongs to the string, not just to `print`.

## 4. Sets: distinct elements without positions

A `set` contains distinct elements; order does not matter. Nonempty set literals use braces, such as `{1, 2, 3}`. Reordering or repeating elements does not change equality: `{1, 2, 3} == {3, 2, 1, 1}` is `True`. Display order is not guaranteed to be sorted.

**Cross-type equality trap, explicitly linked to the worksheet:** `1 == 1.0` and `True == 1` are `True`, while `True == 2` is `False`. Consequently, `{1, 1.0}` collapses to one element, as does `{0, False}`.

In a student-question demo, `False + 1` returned `1`. The professor cautioned against relying on this behaviour; he did not develop general Boolean-arithmetic rules.

Distinguish two operations:
- `in` checks an **element**: `1 in {1, 2, 3}` is `True`, while `'1' in {1, 2, 3}` is `False`.
- Between sets, `<=` checks **subset** and `<` checks **strict subset**; `>=` and `>` check the corresponding superset relationships. These are not the meanings of comparisons between strings or lists.

Sets may mix element types, but cannot contain lists or other sets: `{'a', [1, 2]}` and `{{1, 2}, 'a'}` raise `TypeError`.

Sets have no positional indexing: `{1, 2}[0]` raises `TypeError`. They also do not support concatenation with `+`.

## 5. Lists and choosing a collection

A `list` is an ordered sequence that allows duplicates, written with square brackets. Both order and multiplicity affect equality: `[1, 2, 3]` differs from `[3, 2, 1]` and from `[1, 2, 3, 3]`.

Lists support element membership, concatenation, and indexing:
```python
[1, 2] + [2, 3]       # [1, 2, 2, 3]
[1, 2, 3, 4][-1]      # 4
```
The index bounds are the same as for strings; index `-5` is invalid for this four-element list. Membership checks whole elements: `'cat' in ['c', 'a', 't']` is `False`.

**Choose by the information you must preserve:** use a set when duplicates are unwanted **and** order does not matter; use a list when duplicates may occur **or** order matters. Employee names need a list rather than a set if repeated names must be retained.

Collections may mix types. A **homogeneous** set or list has one element type. A homogeneous dictionary has one key type and one value type, which need not match each other. Otherwise the collection is heterogeneous; this course mostly uses homogeneous collections.

Remember the empty forms: `[]` is an empty list, `{}` is an empty dictionary, and `set()` creates an empty set.

## 6. Dictionaries: lookup by key

A mapping associates **keys** with **values**. Python’s `dict` syntax uses colons within pairs and commas between pairs: `{'Students': 6, 'TAs': 2}`. Choose the direction to match the desired lookup—for prices, map item names to prices.

Keys are distinct, but values may repeat; keys need not be strings. In the demonstrated duplicate-key literal `{'asdf': 1, 'asdf': 2}`, the resulting dictionary was `{'asdf': 2}`.

Values can themselves be collections: `{'cool': ['David', 'Tom', 'You']}` is a `dict` whose value is a `list`. Identify the outer collection when asked for the literal’s type.

Direct dictionary membership checks **keys**, not values:
```python
'hi' in {'asdf': 1, 'hi': 1}     # True
1 in {'asdf': 1, 'hi': 1}        # False
{'asdf': 1, 'hi': 1}['hi']       # 1
```
A missing key raises `KeyError`. The instructor recommends checking membership before lookup when the key’s existence is uncertain.

Dictionary equality compares key–value associations, not pair order. Python tracks insertion order, but reordering the same associations does not change equality.

## 7. Assignment binds a name to a value

Assignment has the form:
```python
variable = expression
```
Python first evaluates the right-hand expression, then binds the name on the left to the resulting value. Assignment is a statement and does not produce a value; `==`, by contrast, compares values.

After `x = 10 + 30`, evaluating `x` produces `40`. A variable reference is an expression, not a literal or a new data type. No explicit type declaration is needed, and reassignment can change the type of value referred to: after `x = 'asdf'`, `x` refers to a string.

**Variable names:** use letters, digits and underscores; a name cannot start with a digit. For example, `score_2` is valid, but `2_score` is not.

## 8. Trace sequentially with a value table

The course’s simplified **value-based memory model** records each variable’s current value. Execute statements in order, updating the table as you go:
```python
a = 3
b = 7 * a
c = [1, a, b]
d = c[0] + a
```

| Variable | Final value |
|---|---|
| `a` | `3` |
| `b` | `21` |
| `c` | `[1, 3, 21]` |
| `d` | `4` |

The instructor recommends this table instead of repeatedly searching earlier code.

**Evaluate names inside collections too.** In the worksheet:
```python
x = 4
y = x + 2
z = {y: x}  # {6: 4}
```
Both the dictionary key and its value are evaluated: the final table entry for `z` is `{6: 4}`, not `{y: x}`.

**Assignment does not establish a persistent formula.** After
```python
x = 37
y = x + 2
x = 20
```
`x` is `20`, but `y` remains `39`: its right-hand side was evaluated once, before `x` changed.

Evaluating an undefined name raises `NameError`. `5 = x` raises `SyntaxError` because a literal cannot be the assignment target. To assign five to `x`, write `x = 5`, not a comparison. For the worksheet’s error task, provide both a brief explanation and a correction.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
