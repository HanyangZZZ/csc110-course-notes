# CSC110: Data types, collections and variables — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Strings, positions, and substrings

5 practice marks

(a) [3 marks] Give the value and Python type of each expression.
```python
'planet'[0]
'planet'[-2]
'ane' in 'planet'
```
(b) [2 marks] What happens when you evaluate `'planet'[6]`? Explain using the valid indices.

<details><summary>Hints</summary>

Start counting at zero; negative indices count from the end.

</details>

<details><summary>Worked solution and rubric</summary>

(a)
| Expression | Value | Type |
|---|---|---|
| `'planet'[0]` | `'p'` | `str` |
| `'planet'[-2]` | `'e'` | `str` |
| `'ane' in 'planet'` | `True` | `bool` |

(b) It raises `IndexError`: the six characters have non-negative indices 0 through 5, so index 6 is outside the string.

- (a) 3 marks: each row earns 0.5 for the value and 0.5 for its type.
- (b) 2 marks: identify an out-of-range error (1); explain that the last non-negative index is 5 (1).

</details>

### Empty collections and homogeneous dictionaries

5 practice marks

(a) [3 marks] Name the Python type of each expression: `[]`, `{}`, `set()`.

(b) [2 marks] Is `{'red': 2, 'blue': 5}` homogeneous under the course definition? Explain using the key and value types.

<details><summary>Hints</summary>

For a dictionary, check the keys as one group and the values as another.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `list`, `dict`, and `set`, respectively.

(b) Yes. Every key is a `str`, and every value is an `int`. Homogeneous dictionary keys and values do not have to share one common type.

- (a) 3 marks: one for each correct type.
- (b) 2 marks: yes (1); keys are all strings and values all integers, even though those types differ (1).

</details>

### Choose the collection that fits

5 practice marks

(a) [3 marks] Choose `list`, `set`, or `dict` for each task, and give a short reason:
1. Record bus stops in visit order, including repeated stops.
2. Record the distinct club names a student has joined; order is irrelevant.
3. Look up a student's score using their unique student ID.

(b) [2 marks] Give the value of each comparison.
```python
{2, 8, 2} == {8, 2}
[2, 8, 2] == [8, 2]
```

<details><summary>Hints</summary>

Consider whether order, repeated values, or key-to-value associations matter.

</details>

<details><summary>Worked solution and rubric</summary>

(a) 1. `list`: retains visit order and repeated stops. 2. `set`: represents distinct club names. 3. `dict`: associates each student ID with a score.

(b) `True`, then `False`. Duplicate entries do not create additional set elements; the two lists have different elements/lengths.

- (a) 3 marks: each task earns 0.5 for the collection and 0.5 for a relevant reason.
- (b) 2 marks: one for each Boolean value.

</details>

### Assignment uses the current value

5 practice marks

Run these lines in order.
```python
x = 6
y = x + 3
x = 1
z = {y: x}
```
(a) [4 marks] Make a table with one row after each line and columns `x`, `y`, `z`. Use `—` before a variable is defined.

(b) [1 mark] Does the third line make `y` become 4? Explain briefly.

<details><summary>Hints</summary>

Evaluate a right-hand side when its assignment runs; do not keep it as a formula.

</details>

<details><summary>Worked solution and rubric</summary>

(a)
| After line | x | y | z |
|---|---|---|---|
| 1 | 6 | — | — |
| 2 | 6 | 9 | — |
| 3 | 1 | 9 | — |
| 4 | 1 | 9 | `{9: 1}` |

(b) No. `y` keeps the value 9 computed on line 2; assigning a new value to `x` does not re-evaluate `x + 3`.

- (a) 4 marks: one for each fully correct row.
- (b) 1 mark: no, because the earlier assignment stored the evaluated value 9.

</details>

## Review quiz

### Value and type

5 practice marks

[5 marks] Give the value and Python type of each independent expression.
```python
10 // 4.0
'sun' + 'rise'
'coat'[1]
'at' in ['cat', 'hat']
{'oak': 4, 'elm': 7}['elm']
```

<details><summary>Worked solution and rubric</summary>

| Expression | Value | Type |
|---|---|---|
| `10 // 4.0` | `2.0` | `float` |
| `'sun' + 'rise'` | `'sunrise'` | `str` |
| `'coat'[1]` | `'o'` | `str` |
| `'at' in ['cat', 'hat']` | `False` | `bool` |
| `{'oak': 4, 'elm': 7}['elm']` | `7` | `int` |

List membership looks for an entire element; neither element is `'at'`.

- 5 marks: each row earns 0.5 for its value and 0.5 for its type.

</details>

### Dictionary membership and key lookup

5 practice marks

Use this dictionary for every part:
```python
d = {'map': 'key', 'key': 'door'}
```
(a) [3 marks] Evaluate `'map' in d`, `'door' in d`, and `d['map'] in d`.

(b) [2 marks] What happens for `d[0]`, and why?

<details><summary>Worked solution and rubric</summary>

(a) `True`, `False`, `True`, respectively. Membership checks keys; `d['map']` is `'key'`, which is also a key.

(b) `KeyError`, because the dictionary has no key `0`. Here, square brackets specify a key, not a position.

- (a) 3 marks: one for each correct Boolean value.
- (b) 2 marks: identify a missing-key error (1); explain that 0 is not a key, rather than treating it as a position (1).

</details>

### Trace a small program

5 practice marks

Run these lines in order.
```python
a = 4
b = [a, a + 1]
a = b[1]
c = a == b[0]
```
(a) [4 marks] Fill a value-based memory table after every line, with columns `a`, `b`, `c`. Use `—` for an undefined variable.

(b) [1 mark] Is the complete third line an expression or an assignment statement?

<details><summary>Worked solution and rubric</summary>

(a)
| After line | a | b | c |
|---|---|---|---|
| 1 | 4 | — | — |
| 2 | 4 | `[4, 5]` | — |
| 3 | 5 | `[4, 5]` | — |
| 4 | 5 | `[4, 5]` | `False` |

(b) An assignment statement. It evaluates `b[1]` and assigns that value to `a`.

- (a) 4 marks: one for each fully correct row.
- (b) 1 mark: assignment statement.

</details>

### Program terminology and a type claim

5 practice marks

(a) [3 marks] Give the most specific category for each complete piece of code: **literal**, **expression that is not a literal**, or **assignment statement**.
```python
12
6 + 6
score = 12
```
(b) [2 marks] A student says, “In a homogeneous dictionary, keys must have the same type as values.” Is this correct? Justify your answer with a small dictionary.

<details><summary>Worked solution and rubric</summary>

(a) `12`: literal. `6 + 6`: expression that is not a literal. `score = 12`: assignment statement.

(b) No. For example, `{'desk': 3, 'lamp': 2}` is homogeneous: all keys are strings and all values are integers. The key type differs from the value type.

- (a) 3 marks: one for each correct classification.
- (b) 2 marks: reject the claim (1); provide and explain a homogeneous dictionary with different key/value types (1).

</details>

## Challenge quiz

### Combine indexing, membership, and concatenation

5 practice marks

Use:
```python
s = 'cocoa'
parts = ['co', 'coa']
```
(a) [3 marks] Give the value and type of each expression.
```python
s[-3] + s[-1]
(s[-2] + s[-1]) in parts
parts[0] + parts[1] == s
```
(b) [2 marks] Give all valid non-negative indices and all valid negative indices for `s`.

<details><summary>Hints</summary>

Work out the string produced inside each expression before checking membership or equality.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `'ca'` (`str`), `False` (`bool`), and `True` (`bool`). The second expression asks whether the whole string `'oa'` is an element of `parts`; it is not. Concatenating `'co'` and `'coa'` gives `'cocoa'`.

(b) Non-negative: 0, 1, 2, 3, 4. Negative: −5, −4, −3, −2, −1.

- (a) 3 marks: each expression earns 0.5 for its value and 0.5 for its type.
- (b) 2 marks: one for each complete, correct range.

</details>

### Follow a key through two collections

5 practice marks

Use:
```python
labels = {1: 'north', 2: 'south'}
route = [2, 1, 2]
```
(a) [3 marks] Give the value and type of each expression.
```python
labels[route[0]][-1]
labels[route[-1]] == labels[2]
'south' in labels
```
(b) [2 marks] A student uses `labels[0]` to find the label for the first item in `route`. Explain the error and replace it with one correct expression.

<details><summary>Hints</summary>

First find a route element; use that value as a dictionary key.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `'h'` (`str`), `True` (`bool`), and `False` (`bool`). The first route value is 2, whose label is `'south'`; its last character is `'h'`. `'south'` is a value, not a key.

(b) `labels[0]` raises `KeyError` because 0 is not a key. Use `labels[route[0]]`, which evaluates to `'south'`.

- (a) 3 marks: each expression earns 0.5 for its value and 0.5 for its type.
- (b) 2 marks: explain the missing key 0 (1); give labels[route[0]] or an equivalent expression using the first route item (1).

</details>

### Reassignment across a list and a dictionary

5 practice marks

Run these lines in order.
```python
n = 9
items = [n, n // 2]
lookup = {'first': items[0], 'last': items[-1]}
n = lookup['last'] + 0.5
```
(a) [4 marks] Fill a memory table after each line, with columns `n`, `items`, `lookup`. Use `—` before a variable is defined.

(b) [1 mark] After the final line, is the first element of `items` 9 or 4.5? Explain.

<details><summary>Hints</summary>

Each assignment uses the values available when that line executes.

</details>

<details><summary>Worked solution and rubric</summary>

(a)
| After line | n | items | lookup |
|---|---|---|---|
| 1 | 9 | — | — |
| 2 | 9 | `[9, 4]` | — |
| 3 | 9 | `[9, 4]` | `{'first': 9, 'last': 4}` |
| 4 | 4.5 | `[9, 4]` | `{'first': 9, 'last': 4}` |

(b) It remains 9. Reassigning `n` does not re-evaluate the earlier list expression or replace its first element.

- (a) 4 marks: one for each fully correct row, including the float value 4.5 on line 4.
- (b) 1 mark: 9, with the explanation that reassignment does not re-evaluate the earlier list expression.

</details>

### Repair the code and preserve the data

5 practice marks

A program must give `count` the value 5 and record three visits in order, including the repeated lab visit. Its author writes:
```python
5 = count
visits = {'lab', 'cafe', 'lab'}
```
(a) [2 marks] Rewrite both lines to meet those requirements.

(b) [1 mark] With your corrected collection, what is `visits[-1]`?

(c) [2 marks] Evaluate the following comparison and explain which dictionary property determines the result.
```python
{'start': 'lab', 'end': 'cafe'} == {'end': 'cafe', 'start': 'lab'}
```

<details><summary>Hints</summary>

An assignment needs the name on the left. Choose a collection that retains both order and repeated entries.

</details>

<details><summary>Worked solution and rubric</summary>

(a)
```python
count = 5
visits = ['lab', 'cafe', 'lab']
```
(b) `'lab'`.

(c) `True`. Both dictionaries have exactly the same key–value associations; their written pair order does not change dictionary equality.

- (a) 2 marks: count = 5 (1); an ordered list retaining all three visits (1).
- (b) 1 mark: the string lab.
- (c) 2 marks: True (1); explain equality through matching key–value associations, regardless of their written order (1).

</details>
