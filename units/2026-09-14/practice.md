# CSC110: Comprehensions and function calls — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Design a comprehension in three steps

5 practice marks

Use `values = [2, 4, 2]`. You want a new collection containing one more than each value, in the same order and with repeated results retained.

(a) [1 mark] Choose the output collection type.

(b) [1 mark] Write its identity comprehension.

(c) [2 marks] Modify that comprehension to solve the problem and give its value.

(d) [1 mark] Explain why the result contains a repeated element.

<details><summary>Hints</summary>

Choose the output type first, then identity, then change only the result expression.

</details>

<details><summary>Worked solution and rubric</summary>

(a) A `list`.

(b) `[x for x in values]`.

(c) `[x + 1 for x in values]` evaluates to `[3, 5, 3]`.

(d) A list retains one result for each input occurrence. Both occurrences of 2 produce 3, so both results remain. A set would discard that repetition.

- (a) List (1).
- (b) Correct identity list comprehension (1).
- (c) Correct transformed comprehension (1); [3, 5, 3] (1).
- (d) Explain that list results preserve repeated input occurrences (1).

</details>

### Transform dictionary keys and values separately

5 practice marks

Use `numbers = [2, 5, 7]`. Create a dictionary whose key is twice an input number and whose associated value is one more than that input number.

(a) [3 marks] Start with an identity dictionary comprehension, then write the completed comprehension.

(b) [2 marks] Give the resulting dictionary and explain what the colon separates.

<details><summary>Hints</summary>

The expression before the colon creates a key; the expression after it creates the associated value.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Identity: `{x: x for x in numbers}`.

Completed: `{2 * x: x + 1 for x in numbers}`.

(b) `{4: 3, 10: 6, 14: 8}`. The colon separates each key from its associated value. For input 5, for example, the key is 10 and its value is 6. Pair order is irrelevant to dictionary equality.

- (a) Identity dictionary comprehension (1); correct key expression (1); correct value expression and comprehension structure (1).
- (b) Correct dictionary (1); distinguish key from associated value (1).

</details>

### Built-in function calls (beyond Checkpoint 1)

5 practice marks

This question practices the full lecture; function calls are excluded from Checkpoint 1.

[5 marks] Give the value and Python type of each expression. Work through the calls before combining their returned values.
```python
abs(-8)
len([4, 4, 1])
sum({2, 5, 2})
sorted({6, 1, 4})
max([2, 9, -1]) - min([2, 9, -1])
```

<details><summary>Hints</summary>

A set removes repeated elements before sum uses it. sorted returns a list.

</details>

<details><summary>Worked solution and rubric</summary>

| Expression | Value | Type |
|---|---|---|
| `abs(-8)` | `8` | `int` |
| `len([4, 4, 1])` | `3` | `int` |
| `sum({2, 5, 2})` | `7` | `int` |
| `sorted({6, 1, 4})` | `[1, 4, 6]` | `list` |
| `max([2, 9, -1]) - min([2, 9, -1])` | `10` | `int` |

The final expression is `9 - (-1)`. The set passed to `sum` contains only 2 and 5, while the list passed to `len` retains both 4s.

- 5 marks: each row earns 0.5 for its value and 0.5 for its type.

</details>

### Range endpoints

5 practice marks

(a) [3 marks] Write one list comprehension using `range` that produces the reciprocals of the integers 2 through 5, inclusive, in increasing input order.

(b) [2 marks] Evaluate the following expression and explain why 7 is absent.
```python
[x for x in range(3, 7)]
```

<details><summary>Hints</summary>

The starting argument is included; the stopping argument is excluded.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `[1 / x for x in range(2, 6)]`, producing `[0.5, 0.3333333333333333, 0.25, 0.2]`.

(b) `[3, 4, 5, 6]`. `range(3, 7)` stops before 7. Similarly, part (a) needs the stop argument 6 to include 5. The list records results in input order; the reciprocals themselves decrease.

- (a) List comprehension (1); reciprocal expression 1 / x (1); correct start and exclusive stop, range(2, 6) (1).
- (b) Correct list (1); explain the exclusive stopping argument (1).

</details>

## Review quiz

### Checkpoint 1: value and collection type

5 practice marks

Chapter 1 practice: comprehensions, variables, and collections.

[5 marks] Give the value and Python type of each independent expression. Keep list order exact; a set may be written in any order.
```python
[x * 2 for x in [3, 1, 3]]
{x * x for x in [-2, 2, 3]}
{x: x + 2 for x in [1, 4]}
[y for y in []]
([1, 2] + [3])[1]
```

<details><summary>Worked solution and rubric</summary>

| Expression | Value | Type |
|---|---|---|
| `[x * 2 for x in [3, 1, 3]]` | `[6, 2, 6]` | `list` |
| `{x * x for x in [-2, 2, 3]}` | `{4, 9}` | `set` |
| `{x: x + 2 for x in [1, 4]}` | `{1: 3, 4: 6}` | `dict` |
| `[y for y in []]` | `[]` | `list` |
| `([1, 2] + [3])[1]` | `2` | `int` |

Squaring −2 and 2 produces the same set element. An empty input produces no comprehension results, but the outer brackets still determine the output collection type.

- 5 marks: each row earns 0.5 for its value and 0.5 for its type.

</details>

### Checkpoint 1: a value-based memory table

5 practice marks

Execute the statements in order.
```python
n = 3
values = [n, n + 2]
result = [x - n for x in values]
n = 10
```
(a) [3 marks] Give the final value of each variable in a two-column table: **Variable | Value**.

(b) [2 marks] A student puts `[n, n + 2]` in the final Value cell for `values`. Explain both why this is an invalid value-table entry and why the final assignment does not change that list.

Answer requirements:

- Write evaluated values only in the Value column.

<details><summary>Worked solution and rubric</summary>

(a)
| Variable | Value |
|---|---|
| n | `10` |
| values | `[3, 5]` |
| result | `[0, 2]` |

(b) A value cell contains the evaluated value, not variable names or an unevaluated expression. The list was created when `n` was 3. The last assignment changes `n`; it does not re-run the previous list expression. `result` was also computed before that reassignment.

- (a) One mark per correct value (3).
- (b) Require evaluated values in the table (1); explain why the earlier list is not recomputed after n changes (1).

</details>

### Checkpoint 1: grouping inside a comprehension

5 practice marks

Use `numbers = [2, 4, 6]`. You want a list containing each number divided by the quantity “that number plus 2,” preserving input order.

(a) [3 marks] Show the identity comprehension, then the completed comprehension.

(b) [2 marks] A student writes `[x / x + 2 for x in numbers]`. Give its first result and the first result of the correct comprehension.

<details><summary>Worked solution and rubric</summary>

(a) Identity: `[x for x in numbers]`.

Completed: `[x / (x + 2) for x in numbers]`.

(b) Incorrect first result: `3.0`, because `2 / 2 + 2` is evaluated as `(2 / 2) + 2`. Correct first result: `0.5`, because `2 / (2 + 2)` divides by 4. The denominator needs explicit grouping.

- (a) Identity list comprehension (1); correct quotient expression with grouped denominator (1); complete list comprehension preserving input order (1).
- (b) Incorrect first result 3.0 (1); correct first result 0.5 (1).

</details>

### Checkpoint 1: ordered combinations

5 practice marks

Consider:
```python
[(x, y) for x in [2, 4] for y in [1, 3]]
```
(a) [4 marks] Write the resulting list, keeping its order exact.

(b) [1 mark] Which variable changes more frequently as consecutive pairs are produced?

<details><summary>Worked solution and rubric</summary>

(a) `[(2, 1), (2, 3), (4, 1), (4, 3)]`.

(b) `y`. Python holds the first selected `x` while it uses every `y`, then moves to the next `x`. This is ordered combination generation, not pairing elements only at matching positions.

- (a) One mark for each correct pair in its correct position (4).
- (b) y (1).

</details>

## Challenge quiz

### Chapter 1 challenge: combinations can collapse

5 practice marks

Chapter 1 practice: comprehensions, variables, and collections.

Use:
```python
values = [-1, 1, 2]
{x * x + y for x in values for y in [0, 1]}
```
(a) [2 marks] Give the resulting set.

(b) [2 marks] How many input combinations are considered, and why are there fewer elements in the result?

(c) [1 mark] Must an answer list the set elements in increasing order? Explain briefly.

<details><summary>Hints</summary>

Record both results for each x, then keep only distinct values.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `{1, 2, 4, 5}`.

(b) Six combinations: three choices of `x`, each paired with two choices of `y`. Both −1 and 1 square to 1, so they each produce 1 and 2. The repeated results occupy no additional set elements.

(c) No. A set's written element order does not affect its value; any ordering of those four elements is correct.

- (a) Include all four correct elements (1), with no extra elements (1).
- (b) Six combinations (1); explain duplicate results from x = -1 and x = 1 (1).
- (c) No, because set element order is irrelevant (1).

</details>

### Chapter 1 challenge: resolve values through a mapping

5 practice marks

Execute the statements in order.
```python
n = 2
numbers = [n, n + 1]
mapping = {x + 10: x * x for x in numbers}
out = [mapping[k] - 1 for k in [13, 12]]
n = 7
```
(a) [4 marks] Give the final value of each variable in a **Variable | Value** table.

(b) [1 mark] Why is `[mapping[13] - 1, mapping[12] - 1]` not an acceptable Value cell for `out`?

Answer requirements:

- Write evaluated values only in the Value column.

<details><summary>Hints</summary>

Build the dictionary first, then use the explicit key list [13, 12] in that order.

</details>

<details><summary>Worked solution and rubric</summary>

(a)
| Variable | Value |
|---|---|
| n | `7` |
| numbers | `[2, 3]` |
| mapping | `{12: 4, 13: 9}` |
| out | `[8, 3]` |

(b) That entry still contains names and computations. The table must show the evaluated list `[8, 3]`. Its order follows the supplied key list, not the order in which dictionary pairs were written.

- (a) One mark per correct final value (4).
- (b) Explain that values must be fully evaluated, so the entry is [8, 3] (1).

</details>

### Chapter 1 challenge: infer the clause order

5 practice marks

Use `A = [0, 1]`, `B = [4, 5]`, and `C = [8, 9]`.

(a) [3 marks] Write a list comprehension producing the target below. Its result expression must be `(a, b, c)`; use each clause `for a in A`, `for b in B`, and `for c in C` exactly once, in the correct order.
```text
[(0, 4, 8), (0, 5, 8), (1, 4, 8), (1, 5, 8),
 (0, 4, 9), (0, 5, 9), (1, 4, 9), (1, 5, 9)]
```
(b) [2 marks] Identify the fastest-changing and slowest-changing variables in this list.

<details><summary>Hints</summary>

Look first for the coordinate that stays constant for the largest block of results.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `[(a, b, c) for c in C for a in A for b in B]`.

(b) Fastest: `b`. Slowest: `c`. For each fixed `c`, the comprehension takes each `a`, and for each such choice it takes both `b` values. The coordinate order in `(a, b, c)` stays fixed even though the clauses use a different order.

- (a) Correct first clause for c in C (1), second clause for a in A (1), and third clause for b in B (1), preserving the specified result expression.
- (b) b fastest (1); c slowest (1).

</details>

### Chapter 1 challenge: repair two independent mistakes

5 practice marks

Use `numbers = [1, 1, 3]`. The required result is a **list**, preserving input order and repetitions, containing the reciprocal of “each number plus 1.” A student proposes:
```python
{x + 1 / x for x in numbers}
```
(a) [3 marks] Write a corrected comprehension.

(b) [2 marks] Give its value and explain why the result has three elements rather than two.

<details><summary>Hints</summary>

Check the outer brackets separately from the arithmetic expression.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `[1 / (x + 1) for x in numbers]`.

(b) `[0.5, 0.5, 0.25]`. The expression divides 1 by the whole quantity `x + 1`. The list retains a result for each input occurrence, so the two input 1s produce two copies of 0.5. Merely changing the original braces would leave the arithmetic incorrect.

- (a) List brackets (1); numerator 1 (1); correctly grouped denominator x + 1 and valid comprehension (1).
- (b) Correct ordered list (1); explain that list results retain repeated occurrences (1).

</details>
