# CSC110: Defining functions and methods — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Parameters, arguments, and documentation

5 practice marks

Consider this definition:
```python
def gap(a: float, b: float) -> float:
    """Return a minus b."""
    return a - b
```
(a) [2 marks] Give the parameter names with their annotated types, and the annotated return type.

(b) [1 mark] Write the two doctest lines for `gap(7.5, 2.0)` to add inside its docstring.

(c) [2 marks] In that call, what are the arguments, and does the body run when the definition is first executed or when the function is called?

<details><summary>Hints</summary>

Parameters are names in the definition; arguments are values supplied by a call.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Parameters: `a: float`, `b: float`. Return type: `float`.

(b)
```pycon
>>> gap(7.5, 2.0)
5.5
```

(c) The arguments are `7.5` and `2.0`, supplied to `a` and `b`. Defining the function makes it available; its body runs when it is called.

- (a) Both named parameter annotations (1); return type float (1).
- (b) Correct console-style call and expected result (1).
- (c) Both argument values (1); body executes on the call, not the definition (1).

</details>

### Follow the Function Design Recipe

5 practice marks

(a) [1 mark] List the five steps of the Function Design Recipe in order.

(b) [4 marks] Write a complete function `mean_pair(first: float, second: float) -> float` that returns the average of `first` and `second`, rounded to one decimal place. Include a plain-English docstring and the doctest `mean_pair(1.0, 2.48)` with its result.

<details><summary>Hints</summary>

The average is the sum divided by two; the second argument to round controls decimal places.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Write example uses → write the header → write the description → implement the body → test the function.

(b)
```python
def mean_pair(first: float, second: float) -> float:
    """Return the average of first and second, rounded to one decimal place.

    >>> mean_pair(1.0, 2.48)
    1.7
    """
    return round((first + second) / 2, 1)
```

The unrounded average is `1.74`. Run the example after defining the function; writing it in a docstring alone is documentation, not a manual test run.

- (a) All five steps in the correct order (1).
- (b) Correct typed header, colon, and indentation (1); description states average and rounding (1); correct doctest (1); correct return expression (1).

</details>

### What annotations and return actually do

5 practice marks

```python
def cube(x: float) -> float:
    return x ** 3
    x = 0
```
(a) [3 marks] For each independent call, give its returned value and type, or name the error and its cause:
```python
cube(2)
cube(1.5)
cube('go')
```
(b) [1 mark] Why does the annotation not force the first call to return a float?

(c) [1 mark] Does `x = 0` execute during any of these calls? Explain.

<details><summary>Hints</summary>

Trace the body itself. An annotation does not convert a value; return ends the call.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `cube(2)` returns `8` (`int`); `cube(1.5)` returns `3.375` (`float`). `cube('go')` raises `TypeError` when the body attempts string exponentiation.

(b) Ordinary Python does not enforce or perform conversion based on these annotations. They document the intended contract.

(c) No. Successful calls leave at `return`; the string call fails while evaluating that return expression, before reaching the assignment.

- (a) Correct value and type for each numeric call (1 each); TypeError attributed to string exponentiation (1).
- (b) Annotations do not enforce/convert runtime types (1).
- (c) Assignment is unreachable after return, and also not reached when its expression fails (1).

</details>

### Read and rewrite method calls

5 practice marks

(a) [2 marks] Rewrite each call using the preferred object-dot notation. Assume `items` is a list; do not evaluate the second call.
```python
str.lower('FiLE')
list.append(items, 9)
```
(b) [2 marks] Give the returned values:
```python
str.split('red blue red')
list.count([4, 1, 4, 4], 4)
```
(c) [1 mark] What required information is missing from `list.count([4, 1, 4, 4])`?

Answer requirements:

- For append, only convert notation; mutation and return-value behavior are not assessed.

<details><summary>Hints</summary>

Move the object from the first argument to the position before the dot.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `'FiLE'.lower()` and `items.append(9)`.

(b) `['red', 'blue', 'red']` and `3`.

(c) The element whose occurrences should be counted. The list is the object being searched, so it does not supply the target element too.

- (a) One mark per equivalent object-dot call (2).
- (b) One mark per correct returned value (2).
- (c) Identify the missing target element (1).

</details>

## Review quiz

### Values and types across methods and calls

5 practice marks

Full-lecture mock practice.

[5 marks] Give the value and Python type of each independent expression. A set may be written in any order.
```python
'Blue Sky'.split()
'ABC'[1].lower()
[2, 5, 2].count(2)
{1, 3}.union({3, 5})
round(8 / 3, 1)
```

<details><summary>Worked solution and rubric</summary>

| Expression | Value | Type |
|---|---|---|
| `'Blue Sky'.split()` | `['Blue', 'Sky']` | `list` |
| `'ABC'[1].lower()` | `'b'` | `str` |
| `[2, 5, 2].count(2)` | `2` | `int` |
| `{1, 3}.union({3, 5})` | `{1, 3, 5}` | `set` |
| `round(8 / 3, 1)` | `2.7` | `float` |

The index selects `'B'` before `lower` runs. Union retains one copy of the repeated element 3.

- Each of the five rows: correct value (0.5) and correct type (0.5), for 5 marks.

</details>

### A later definition replaces the earlier one

5 practice marks

Execute this code in order:
```python
def adjust(x: int) -> int:
    return x + 2

before = adjust(4)

def adjust(x: int, y: int) -> int:
    return x - y

after = adjust(9, 2)
```
(a) [2 marks] Give `before` and `after`.

(b) [2 marks] After all these lines, does `adjust(4)` return a value? Name any error and explain why Python does not select the first definition.

(c) [1 mark] Does the second definition change the value already stored in `before`? Explain briefly.

<details><summary>Worked solution and rubric</summary>

(a) `before` is `6`; `after` is `7`.

(b) `adjust(4)` raises `TypeError` because the currently bound function requires the second argument `y`. The second definition replaced the binding for `adjust`; Python does not retain both ordinary definitions and select by argument count.

(c) No. `before` already holds the integer `6` computed by the earlier call; redefining the name does not recompute that assignment.

- (a) before=6 (1); after=7 (1).
- (b) TypeError due to missing y (1); later definition replaces the name rather than creating an overload (1).
- (c) Earlier computed value remains 6, with explanation (1).

</details>

### Repair a distance calculation and test it

5 practice marks

This function is meant to return Euclidean distance rounded to one decimal place:
```python
def point_distance(x1: float, y1: float, x2: float, y2: float) -> float:
    """Return the distance between (x1, y1) and (x2, y2), rounded to one decimal place."""
    return round((x2 - x1) ** 2 + (y2 - y1) ** 2, 1)
```
(a) [2 marks] What does `point_distance(0.0, 0.0, 2.0, 3.0)` currently return, and what should it return?

(b) [2 marks] Replace its return statement with a correct one.

(c) [1 mark] Give one additional console-style test using identical points and its expected result.

<details><summary>Worked solution and rubric</summary>

(a) It returns `13.0`, the sum of squared differences. It should return `3.6`, the square root of 13 rounded to one decimal place.

(b)
```python
return round(((x2 - x1) ** 2 + (y2 - y1) ** 2) ** 0.5, 1)
```

(c) For example:
```pycon
>>> point_distance(-2.0, -3.0, -2.0, -3.0)
0.0
```

- (a) Actual 13.0 (1); intended 3.6 (1).
- (b) Square root of the whole sum (1); correct return and one-decimal rounding (1).
- (c) Valid identical-points call with expected 0.0 (1).

</details>

### Design a function from a text specification

5 practice marks

[5 marks] Write a complete function `word_lengths(text: str) -> list`. It returns the lengths of the whitespace-separated words in `text`, in their original order.

Include a brief docstring describing the result and a doctest for `word_lengths('read the code')`. Use `split`, `len`, and a list comprehension; do not use a loop statement.

Answer requirements:

- Use a comprehension, not a loop statement.

<details><summary>Worked solution and rubric</summary>

```python
def word_lengths(text: str) -> list:
    """Return the lengths of the whitespace-separated words in text, in order.

    >>> word_lengths('read the code')
    [4, 3, 4]
    """
    return [len(word) for word in text.split()]
```

`split` supplies the word list; the comprehension replaces each word with its length while preserving order.

- Correct typed header/indentation (1); description covers words and order (1); correct doctest (1); split into words (1); return their lengths in an ordered list comprehension (1).

</details>

## Challenge quiz

### A Boolean contract does not repair a body

5 practice marks

Full-lecture challenge practice.

```python
def same_word_count(left: str, right: str) -> bool:
    """Return whether left and right contain the same number of words."""
    len(left.split()) == len(right.split())
    return 1
```
(a) [2 marks] Give the value and type returned by `same_word_count('one two', 'three')`. Explain why `-> bool` does not fix the result.

(b) [2 marks] Replace the body with one correct return statement.

(c) [1 mark] Write the corrected function's doctest lines for the same call.

<details><summary>Hints</summary>

Computing a comparison is different from returning its result.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `1` of type `int`. The comparison's result is discarded; the body explicitly returns `1`. Annotations document the contract but do not enforce it or convert the result to `bool`.

(b)
```python
return len(left.split()) == len(right.split())
```

(c)
```pycon
>>> same_word_count('one two', 'three')
False
```

The two strings contain 2 and 1 words respectively.

- (a) Value 1 and type int (1); annotations do not enforce/convert the return value (1).
- (b) Return the comparison (1); correct word counts for both strings (1).
- (c) Correct doctest with False (1).

</details>

### Preserve repeated purchases when using a helper

5 practice marks

Use this tested helper:
```python
def taxed_unit(price: float, rate: float) -> float:
    """Return price after tax at rate, rounded to two decimal places."""
    return round(price * (1 + rate), 2)
```
A basket contains `prices = [4.0, 4.0, 8.0]` with `rate = 0.25`. Each occurrence is a separate purchase.

(a) [2 marks] Evaluate the following attempted total and explain its mistake:
```python
sum({taxed_unit(p, rate) for p in prices})
```
(b) [2 marks] Write the body of `budget_left(budget: float, prices: list, rate: float) -> float`. It should subtract every individually taxed purchase from `budget`. Use the helper, a comprehension, and `sum`.

(c) [1 mark] What should `budget_left(30.0, prices, rate)` return?

<details><summary>Hints</summary>

The collection used inside sum must retain one cost for every purchase.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `15.0`. The set contains only `5.0` and `10.0`, losing one of the two purchases costing 5.0 after tax.

(b)
```python
return budget - sum([taxed_unit(p, rate) for p in prices])
```

(c) `10.0`: the correct costs are `[5.0, 5.0, 10.0]`, totalling 20.0, leaving 10.0 from 30.0.

- (a) Incorrect total 15.0 (1); identify removal of the repeated purchase by the set (1).
- (b) Helper applied in a list comprehension retaining occurrences (1); sum subtracted from budget (1).
- (c) Correct remaining budget 10.0 (1).

</details>

### Method calls do not change comprehension traversal

5 practice marks

```python
def tagged(words: list, tags: list) -> list:
    return [(word.upper(), tag.lower()) for tag in tags for word in words]
```
Use `words = ['go', 'up']` and `tags = ['A', 'B']`.

(a) [2 marks] Give the returned list.

(b) [2 marks] Swap only the two `for` clauses so that each word appears with all tags before moving to the next word. Give the revised return statement and its result.

(c) [1 mark] In the revised expression, which variable changes fastest?

<details><summary>Hints</summary>

Read clause order independently of the positions inside the output pair.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `[('GO', 'a'), ('UP', 'a'), ('GO', 'b'), ('UP', 'b')]`.

(b)
```python
return [(word.upper(), tag.lower()) for word in words for tag in tags]
```

Result: `[('GO', 'a'), ('GO', 'b'), ('UP', 'a'), ('UP', 'b')]`.

(c) `tag`, because its clause is now rightmost. The output pair still places the transformed word before the transformed tag.

- (a) Four correct pairs in their correct positions (0.5 each, 2 total).
- (b) Correct revised clauses/return (1); correct resulting list (1).
- (c) tag changes fastest (1).

</details>

### Design and document a case-insensitive frequency function

5 practice marks

[5 marks] Write a complete function `word_frequency(words: list, query: str) -> int` that counts occurrences of `query` in `words`, ignoring uppercase/lowercase differences. Assume all list elements are strings.

Use a list comprehension, `lower`, and `count`. Include a docstring naming the parameters and two doctests: one where matching requires ignoring case, and one returning zero. Do not use loop statements or conditionals.

Answer requirements:

- Use only the taught comprehension and methods; no loop statements or conditionals.

<details><summary>Hints</summary>

Normalize both the list elements and the query before counting.

</details>

<details><summary>Worked solution and rubric</summary>

```python
def word_frequency(words: list, query: str) -> int:
    """Return the number of occurrences of query in words, ignoring case.

    >>> word_frequency(['Red', 'red', 'BLUE'], 'RED')
    2
    >>> word_frequency(['Red', 'BLUE'], 'green')
    0
    """
    normalized = [word.lower() for word in words]
    return normalized.count(query.lower())
```

Normalizing only the list would miss an uppercase query; normalizing only the query would miss uppercase list entries. Repeated matches must remain in the list.

- Correct typed header/indentation (1); clear description naming words/query and ignoring case (1); two valid console-style examples covering case-insensitive matches and zero (1); lower every list element in a list comprehension (1); return count of the lowered query (1).

</details>
