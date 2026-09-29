# CSC110: Conditionals, Boolean simplification, and PythonTA

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Quantifier order: individual choices versus one shared choice

A **predicate** is a statement whose truth depends on its inputs. Here, `Loves(a, b)` says whether person `a` loves person `b`. Its domain is `A × B`: pairs with the first person from `A = {Breanna, Malena, Patrick, Ella}` and the second from `B = {Sophia, Thelonious, Stanley, Laura}`. The **universal quantifier** `∀` means “for every”; the **existential quantifier** `∃` means “there is at least one.” A choice showing that an existential statement is true is called a witness.

Compare:
- `∀a ∈ A, ∃b ∈ B, Loves(a, b)`: everyone in `A` loves someone in `B`. For each fixed `a`, choose a suitable `b`; different people may have different witnesses.
- `∃b ∈ B, ∀a ∈ A, Loves(a, b)`: there is someone in `B` whom everyone in `A` loves. Choose one `b` first, then verify that it works for every `a`. You cannot switch `b` partway through.

**Supplementary clarification — “Exercise 3: Multiple Quantifiers”** supplies the exact table used in this review:

| Person in A | Sophia | Thelonious | Stanley | Laura |
|---|---|---|---|---|
| Breanna | False | True | True | False |
| Malena | False | True | True | True |
| Patrick | False | False | True | False |
| Ella | False | False | True | True |

Initially both statements are true. For the first, possible witnesses are Thelonious for Breanna and Malena, Stanley for Patrick, and Stanley or Laura for Ella. For the second, Stanley is a single witness that works for all four people. The table also makes `∀a ∈ A, Loves(a, Stanley)` true, whereas `∀b ∈ B, Loves(Ella, b)` is false: Ella does not love every person in `B`.

Now change **only** `Loves(Ella, Stanley)` to `False`. The first statement stays true: use Thelonious for Breanna and Malena, Stanley for Patrick, and Laura for Ella. The second becomes false. Sophia fails because not everyone loves Sophia; Thelonious fails, for example, for Patrick; Stanley now fails for Ella; Laura fails, for example, for Breanna. These four checks exhaust `B`, so there is no remaining candidate for a shared witness.

This changed table is a **counterexample** to treating the two orders as equivalent: one statement is true while the other is false. It does not show that they must always differ—the original table made both true. The key distinction is the permitted dependence of `b` on `a`, not merely the presence of the same two quantifier symbols.

## Learning: memory, chunking, and cognitive load

The lecture distinguished two memory systems. **Long-term memory** stores more lasting knowledge and was described as functionally limitless. **Working memory** holds the information currently being used to solve a particular problem. Its capacity is limited and relatively fixed; the lecture described roughly five chunks, with variation between people, while the slide described fewer than ten. The analogy to computer storage and RAM is useful but limited: human memory does not behave exactly like a computer.

A **chunk** is a group of information treated as one familiar unit. For someone familiar with email addresses and URLs, `gmail.com` may occupy one chunk. An unfamiliar string such as `xvjki.wmt` may require several because the person cannot recognize a useful pattern. Experience does not have to increase the number of available working-memory slots to improve performance: it can increase how much meaningful information fits in each chunk. This is why expertise depends strongly on experience and knowledge in long-term memory, rather than just working-memory capacity.

**Cognitive load** is the demand a task places on working memory:
- **Intrinsic load** is the information or difficulty the task inherently requires.
- **Extraneous load** comes from unnecessary difficulty in how the task is presented or handled—for example, confusing wording or needing to translate the description.

A clear diagram and a long written description might contain the same information, yet the diagram can reduce the effort needed to organize it. Beginners often cannot yet tell which details matter and which can be ignored, so they may spend more working memory on extraneous information. Practice helps with both recognizing useful chunks and identifying relevant information. Organizing a problem, **abstracting** it—focusing on the important features—and **decomposing** it—breaking it into smaller parts—can make complex problems more manageable.

## From straight-line code to conditional execution

**Straight-line code** follows a fixed sequence of statements. A function call may temporarily transfer execution to another function, but the sequence still determines where execution goes next. The distance example computes the squared horizontal and vertical differences, adds them, takes a square root, then rounds:
```python
def calculate_distance(x1: float, y1: float,
                       x2: float, y2: float) -> float:
    dx_squared = (x2 - x1) ** 2
    dy_squared = (y2 - y1) ** 2
    exact_distance = (dx_squared + dy_squared) ** 0.5
    return round(exact_distance, 1)
```
Every call follows those same calculation steps.

A **Boolean expression** evaluates to `True` or `False`. Earlier filtering comprehensions used one to select collection elements:
```python
numbers = [1, 2, 3, 4]
[x * 100 for x in numbers if x % 2 == 0]
# [200, 400]
```
The filter keeps `2` and `4` because their remainder on division by two is zero, then multiplies those retained values by 100. Today’s `if` statements use Boolean expressions for a different purpose: choosing which statements execute.

```python
if condition:
    # Statements in the if branch
    ...
else:
    # Statements in the else branch
    ...
```
A **branch** is one of the alternative blocks of statements. Python evaluates the condition first. If true, it executes the `if` branch; otherwise it executes the `else` branch. Colons end the branch headers, and indentation identifies the statements belonging to each branch. Each branch can contain multiple statements.

An `if` statement is a **compound statement**: a statement containing other statements, possibly including another `if`. It controls execution rather than itself producing a value like an expression. A `return` inside a branch can, of course, return a value from the containing function.

The `else` branch is optional. Without it, a false condition simply causes Python to skip the `if` body and continue after the whole statement. With either form, execution normally continues after the conditional when its chosen branch finishes—unless a statement such as `return` has already exited the function.

## Flight status: choosing the right condition and tracing returns

The flight-status function receives `scheduled` and `estimated` integer hours from **0 through 23 inclusive**, interpreted within the simplified same-day model. It returns a string, not a Boolean. In the original two-status version, early and exactly-on-time departures both count as `'On time'`; later departures count as `'Delayed'`. This model does not handle crossing midnight.

```python
def get_status(scheduled: int, estimated: int) -> str:
    if scheduled >= estimated:
        return 'On time'
    else:
        return 'Delayed'
```
The condition says that the scheduled hour is at least the estimated hour, equivalently `estimated <= scheduled`. Thus the estimate is earlier than or equal to the schedule.

The initial suggestion, `scheduled <= estimated`, was wrong **with these return values in these branches**. For `(10, 10)` it returned `'On time'`, which looked correct, but for `(10, 12)` it also returned `'On time'`, even though the flight was late. One passing example did not establish that the condition was correct. Reversing the comparison to `>=` fixed both examples and included the early-departure case.

Execution traces:
- `get_status(10, 10)`: evaluate `10 >= 10` → `True`; execute `return 'On time'`; exit the function.
- `get_status(10, 12)`: evaluate `10 >= 12` → `False`; jump to `return 'Delayed'`; exit the function.

The `else:` header is a structural label telling Python where the alternative branch starts; it is not a separate computation in these body-line traces. The condition line does execute because Python must evaluate its comparison.

An equivalent early-return version is:
```python
def get_status(scheduled: int, estimated: int) -> str:
    if scheduled >= estimated:
        return 'On time'
    return 'Delayed'
```
The final return is outside the `if`, at the same indentation as its header. It is reached only when the condition is false: the true case already returned from the entire function. This reasoning depends on the early `return`; simply deleting `else` is not a generally valid transformation for arbitrary branch bodies.

Another valid arrangement checks `estimated > scheduled` and returns `'Delayed'` first, with `'On time'` in the other case. Changing the order of return values requires changing the condition accordingly.

The instructor demonstrated ordinary example calls by using Shift+Enter to send code to a Python terminal, and demonstrated tracing by adding a function call, setting a breakpoint, starting the debugger, and using Step Over. The debugger showed the current parameter values and the jump directly from the failed condition to the first statement of the else branch.

## Worksheet 1: traces, useful doctests, names, and larger sums

### Tracing `can_vote`
The exercise uses age alone to classify whether someone meets its voting-age threshold; it is not a complete model of voting eligibility. Its numbered function body is:
```text
6  if age < 18:
7      return 'Too young to vote'
8  else:
9      return 'Allowed to vote'
```
- Age `17`: `17 < 18` is true, so execute lines **6, 7**.
- Age `18`: `18 < 18` is false, so execute lines **6, 9**.
- Age `19`: the same condition is false, so execute lines **6, 9**.

Line 8 is not counted as a separate executed computation. When asked about line 1, the function header, the instructor allowed including it as a call-entry step where the argument is assigned to the parameter. The worksheet specifically asks for lines in the **function body**, for which the sequences above are the intended traces. The debugger can help check a trace by pausing at a breakpoint and stepping through the call.

### Choose examples that explain distinct behavior
A **doctest example** is a Python call and its expected result included in a function’s documentation. The proposed examples `can_vote(1)` and `can_vote(2)` both produce `'Too young to vote'`. They are not incorrect, but both demonstrate the same branch. A more informative pair includes a young age and an allowed age, such as `1` and `20`, to show the two possible outcomes.

**Supplementary explanation — “Testing all the branches”**: designing tests using knowledge of the code’s execution paths is **white box testing**; designing tests from the specification without inspecting the code is **black box testing**. Branching means one input exercises only one path, so a test suite should exercise each possible path. However, executing every line or branch at least once does not prove correctness: additional cases, particularly boundaries, can still expose bugs. This expands the lecture’s point about choosing informative examples; it is not a claim that these testing terms were introduced aloud today.

### Format a name with an empty-family-name case
```python
def format_name(given_name: str, family_name: str) -> str:
    if family_name == '':
        return given_name
    else:
        return family_name + ', ' + given_name
```
An **empty string**, `''`, contains no characters. If the family name is empty, adding the usual separator would create a misleading leading comma, so return only the given name. Otherwise concatenate the family name, a comma followed by one space, and the given name.
- `format_name('Cherilyn', 'Sarkisian')` → `'Sarkisian, Cherilyn'`.
- `format_name('Cher', '')` → `'Cher'`.

### Return the list with the larger sum
```python
def larger_sum(nums1: list, nums2: list) -> list:
    if sum(nums1) >= sum(nums2):
        return nums1
    else:
        return nums2
```
Assume both inputs are lists of floats. `sum` adds their elements. The function must return the **original input list**, not its sum, and must choose `nums1` on a tie. The `>=` comparison gives equality to the first branch; using only `>` here would incorrectly give a tie to `nums2`.
- `[1.26, 2.01, 3.3]` has sum `6.57`, less than the sum `9.0` of `[3.0, 3.0, 3.0]`, so the example returns `[3.0, 3.0, 3.0]`.
- `[2.0, 1.0]` and `[1.0, 2.0]` both sum to `3.0`, so the example returns the first list, `[2.0, 1.0]`.

The else can again be removed if `return nums2` is unindented to follow the `if`: a true condition already exits via `return nums1`.

The attempted replacement `return max(sum(nums1), sum(nums2))` is wrong because it returns the larger **number**, not the corresponding list. The editor’s return-type warning helped reveal this mismatch. The issue is not that `max` requires a collection: it accepts either a collection or multiple arguments. Both `max([sum(nums1), sum(nums2)])` and the two-argument form still select a sum, so neither satisfies this list-returning contract. The name-formatting and larger-sum implementations were explained in class but students were asked to run and check them themselves.

## Multiple branches: elif, branch order, and flight cancellation

An `elif`—“else if”—adds a condition to try only if all preceding conditions in the same chain were false:
```python
if condition1:
    ...
elif condition2:
    ...
elif condition3:
    ...
else:
    ...
```
Python evaluates conditions from top to bottom and stops at the **first true condition**, executing only that branch. Later conditions are not checked. If all conditions are false, the final else runs, if present. Without an else, no branch need run. There can be any number of `elif` branches.

For the revised flight problem:
- `'On time'`: estimated departure is earlier than or equal to scheduled departure.
- `'Delayed'`: late by **less than four hours**.
- `'Cancelled'`: late by **four or more hours**.

The final lecture implementation is:
```python
def get_status(scheduled: int, estimated: int) -> str:
    if scheduled >= estimated:
        return 'On time'
    elif estimated - scheduled < 4:
        return 'Delayed'
    else:
        return 'Cancelled'
```
Keep the same-day integer-hour assumptions from the original problem. `estimated - scheduled` measures lateness. Although the second condition does not explicitly test that lateness is positive, reaching it tells us that `scheduled >= estimated` was false. Therefore `estimated > scheduled`, so lateness is already known to be positive. The second branch consequently covers `0 < estimated - scheduled < 4`, and the else covers `estimated - scheduled >= 4`.

Exactly four hours late is **cancelled**, not merely delayed. Also, the final else includes equality to four, not just values greater than four. **Reading discrepancy:** the supplied “Code with more than two cases” reading uses `<= 4` for delayed; that boundary differs from this lecture’s specification and live `< 4` code. Follow the lecture boundary for these notes and exercises.

You can instead test cancellation first with `estimated - scheduled >= 4`, then separate delayed from on-time cases. But moving conditions around without adjusting them can change the answer: `< 4` by itself includes early and exactly-on-time flights.

Because an `if` is itself a statement, another `if` can be placed inside a branch. This is **nesting**. Nested two-way choices can express the same three cases, but an `if`/`elif`/`else` chain makes the alternatives easier to see together at one indentation level.

## Worksheet 2: exhaustive cases for porridge and rock, paper, scissors

### Porridge: preserve strict and inclusive boundaries
```python
def porridge_satisfaction(temperature: float) -> str:
    if temperature > 50.0:
        return 'This porridge is too hot! Ack!!'
    elif temperature < 49.0:
        return 'This porridge is too cold! Brrr..'
    else:
        return 'This porridge is just right! Yum!!'
```
Temperatures strictly above `50.0` are too hot; temperatures strictly below `49.0` are too cold. Reaching the else means both comparisons failed: the temperature is at most `50.0` and at least `49.0`. Thus **both endpoints**, `49.0` and `50.0`, are just right.

The worksheet examples are `65.5` → `'This porridge is too hot! Ack!!'`, `30.0` → `'This porridge is too cold! Brrr..'`, and `49.5` → `'This porridge is just right! Yum!!'`.

An equivalent nested structure puts `if temperature < 49.0` and its else inside the outer else. It has the same three outcomes, but the reader must follow extra indentation to see them. The instructor recommended the flatter `elif` version for readability; the exercise specifically requested `elif` branches.

### Rock–paper–scissors: group cases with the same outcome
Assume each input string belongs to `{'rock', 'paper', 'scissors'}`. Rock beats scissors, scissors beats paper, and paper beats rock; equal moves tie.

There are `3 × 3 = 9` ordered input pairs because each of three first-player choices can be paired with each of three second-player choices. A nine-case chain works. Checking equality first groups the three ties into one branch, leaving six non-ties. Three are Player 1 wins; the other three are Player 2 wins.

```python
def rock_paper_scissors(player1: str, player2: str) -> str:
    if player1 == player2:
        return 'Tie!'
    elif (player1 == 'rock' and player2 == 'scissors') or \
         (player1 == 'scissors' and player2 == 'paper') or \
         (player1 == 'paper' and player2 == 'rock'):
        return 'Player1 wins'
    else:
        return 'Player2 wins'
```
Each `and` requires both moves for one winning pair. The `or`s combine those alternative ways Player 1 can win. A backslash at the end of a physical line continues the long statement onto the next line, as demonstrated in class.

The final else is justified by **exhaustiveness**: all allowed input pairs have been accounted for as ties, Player 1 wins, or Player 2 wins. If neither of the first two categories applies, the input must be in the third. This reasoning depends on the input-domain assumption; it is not validation for arbitrary strings.

The live code first listed Player 2’s cases separately: `(player2, player1)` equal to `(rock, scissors)`, `(scissors, paper)`, or `(paper, rock)`. The final individual condition could be replaced by else once the preceding possibilities were eliminated. Then all three Player 2 cases were replaced by the single else above.

Examples: `('rock', 'scissors')` → `'Player1 wins'`; `('rock', 'paper')` → `'Player2 wins'`; `('rock', 'rock')` → `'Tie!'`.

Other arrangements can be correct: leave the six non-tie cases separate, or use an outer branch for Player 1’s move and inner branches for Player 2’s move. A dictionary of move/outcome relationships was also mentioned as a possible alternative, but no dictionary implementation was developed.

## Modules, __name__, and the main block

A **module** is a Python file in the setting used here. Python sets a special variable, `__name__`, for each module:
- When an ordinary module is imported, its `__name__` is its module name. For example, after `import doctest`, `doctest.__name__` is `'doctest'`.
- When a file is run directly as the program, its `__name__` is `'__main__'`.

That distinction explains the **main block**:
```python
if __name__ == '__main__':
    import doctest
    doctest.testmod(verbose=True)
```
The condition is true when this file is run directly, so the indented checks run. When another file imports it, the condition is false, so those checks are skipped. The importer may want the module’s function definitions without automatically running its doctests, pytest checks, or PythonTA checks. The main block separates reusable definitions from work intended to run only on demand. It does not prevent every top-level statement from running on import—only the statements guarded by this condition are skipped.

The demonstration used `test.py` containing:
```python
print(__name__)
```
Running `test.py` directly printed `__main__`. Running another file containing `import test` printed `test`, because the imported module’s unguarded print executed under its own name. After changing `test.py` to:
```python
if __name__ == '__main__':
    print(__name__)
```
running it directly still printed `__main__`, but importing it in the demonstration no longer printed anything.

Shift+Enter sends selected code into an interactive Python terminal. There, `__name__` is also `'__main__'`, but sending text this way is **not** the same as running the source file as a module: the terminal does not acquire that file’s module identity. This distinction matters especially for file-based tools such as PythonTA.

A student asked about naming a file `__main__.py`. The exploratory demo did not establish that such a filename is forbidden; the instructor’s initial guess that Python would reject the name was not supported by what followed. The instructor tentatively attributed the unexpected import behavior to `__main__` referring to the currently running program and advised avoiding this special-name experiment. Treat that discussion as a caveat, not a rule that ordinary files cannot be named `__main__.py`.

## Simplifying conditionals without changing the contract

Long chains and deep nesting can make code difficult to read. Simplification is useful when it preserves the function’s meaning, input assumptions, and return type—not merely when it removes lines.

### Return a Boolean expression directly
```python
def is_even(n: int) -> bool:
    return n % 2 == 0
```
`%` gives the remainder. An integer is even exactly when division by two leaves remainder zero. The longer code `if n % 2 == 0: return True` with `else: return False` simply returns the same truth value that the comparison already computed, so the conditional adds no decision beyond that expression.

For oddness, the original branch returns **False** on remainder zero and **True** otherwise. Therefore return the negation rather than the original condition:
```python
def is_odd(n: int) -> bool:
    return n % 2 != 0
```
`return not (n % 2 == 0)` is equivalent, though the instructor preferred `!=` for clarity. `return n % 2 == 1` also works for integer `n`, since Python’s remainder with divisor `2` is either `0` or `1`.

### Convert nested exclusions into an inclusive range
The worksheet’s `is_teenager` returns false below 13, false above 18, and true otherwise. To reach true, neither exclusion may apply: `age >= 13` **and** `age <= 18`.
```python
def is_teenager(age: int) -> bool:
    return 13 <= age <= 18
```
A **chained comparison** combines the two comparisons; here it is equivalent to `13 <= age and age <= 18`. This function’s specified range is **13 through 18 inclusive**, regardless of other everyday uses of the word “teenager.”

### Replace selection of a smaller or larger value with `min` or `max`
```python
def cap_at_100(grade: float) -> float:
    return min(grade, 100.0)
```
If the grade is below `100.0`, it is the smaller value and is returned unchanged. If above, `100.0` is smaller and becomes the result; equality also returns `100.0`. This is an upper cap only: it does not impose a lower bound.

```python
def larger_first_value(numbers1: list, numbers2: list) -> int:
    return max(numbers1[0], numbers2[0])
```
Assume both inputs are **non-empty lists of integers**, so index `0` exists and gives the first element. The function returns the larger of these two integer values. Unlike `larger_sum`, its required result is the compared value itself, not the list containing it; that is why `max` is an appropriate direct replacement here.

### Combine required conditions with `and`
```python
def is_common_prefix(prefix: str, s1: str, s2: str) -> bool:
    return s1.startswith(prefix) and s2.startswith(prefix)
```
A **prefix** is a string at the beginning of another string; `s.startswith(prefix)` checks that relationship. The nested version returned true only when both strings started with the prefix. `and` expresses exactly that requirement: one match alone is not enough.

The general method is to work backward from the result you want to preserve: determine precisely what must have been true—and which earlier conditions must have been false—to reach each return. Then express that condition directly, or use a built-in selector only when it returns the right kind of object.

## Short-circuit evaluation: safe dictionary comparisons

A dictionary maps **keys** to corresponding **values**. `key in mapping` checks whether a key exists; `mapping[key]` retrieves its value. Retrieving a missing key raises a `KeyError` rather than returning `False`. In the demonstration:
```python
d = {1: 'a', 2: 'b'}
d[3]
# KeyError: 3
```
The keys `1` and `2` exist, but `3` does not.

The function `same_corresponding_values(mapping, key1, key2)` must return true when both keys exist and their values are equal, and false if either key is missing. The longer code checks for a missing first key, then a missing second key, then compares their values:
```python
if key1 not in mapping:
    return False
elif key2 not in mapping:
    return False
elif mapping[key1] == mapping[key2]:
    return True
else:
    return False
```
Reaching `return True` requires three facts: the first missing-key condition was false, the second missing-key condition was false, and the equality was true. Negating the missing-key tests gives membership tests, so the simplified implementation is:
```python
def same_corresponding_values(mapping: dict, key1: str,
                              key2: str) -> bool:
    return (key1 in mapping and key2 in mapping
            and mapping[key1] == mapping[key2])
```
Why this is safe depends on **short-circuit evaluation**: Python can stop evaluating a Boolean chain once its result is determined.
- In a chain of Boolean expressions joined by `and`, Python evaluates left to right and stops at the first false expression. One false requirement is enough to make the conjunction false.
- In a chain joined by `or`, Python stops at the first true expression. One true alternative is enough to make the disjunction true.

If `key1` is missing, the first membership test is false and nothing later is evaluated. If only `key2` is missing, the second test is false and the equality is skipped. Only when **both** membership tests pass are the dictionary lookups evaluated. Thus the order does more than state the right logic: it prevents an error.

Returning only `mapping[key1] == mapping[key2]` fails the required behavior for missing keys. Moving the equality before the membership checks is unsafe for the same reason: a later check cannot protect a lookup that has already been attempted.

## PythonTA: running checks and interpreting the report

**PythonTA** is a program that checks Python code for common correctness, design, and style issues. These are different concerns: code may need a missing return fixed, may use a poor design, or may violate a formatting convention. Its checks contribute to assignment marks; a clean report is not, by itself, proof that the program meets every part of its specification.

The course supplies checker code in the starter file’s main block. Assignment 1 is an explicit exception by part: **Part 1 does not require PythonTA; Parts 2 and 3 do**.

To check the relevant file:
1. Find the supplied PythonTA lines at the bottom of the file.
2. **Uncomment** them when ready to check. The starter comment says to do this; selecting the lines and pressing Ctrl/Cmd + `/` toggles comments in VS Code.
3. Run the **whole Python file**, not a Shift+Enter selection. PythonTA needs the source-file context that copying text into a terminal does not provide.
4. Read the report opened in the browser, fix the issues, and rerun before submission.

The main-block pattern includes `import python_ta` and a supplied `python_ta.check_all(config=...)` call. Keep the provided configuration rather than replacing it with a generic call. In the displayed Assignment 1 Part 2 configuration, `max-line-length` is `120`, `a1_helpers` is an allowed extra import, and `For`, `While`, and `Slice` are disallowed syntax. These are configuration settings for that shown assignment file, not universal Python restrictions.

The demo ran against unfinished starter code and showed:
- **R9711, missing-return-statement:** functions had not yet been implemented with their required returns.
- **E9989, pep8-errors:** the displayed report expected two blank lines after a function definition before following top-level code, but found one.
- **W0511, fixme:** `TODO` comments remained. Complete the required work and remove the leftover placeholders before submission.
- **W0613, unused-argument:** a parameter was never used in a function body. This may indicate unfinished code or a misspelled parameter name; investigate the cause rather than blindly deleting required parameters.

The report distinguishes code errors/forbidden usage, marked high priority, from style/convention issues to fix before submission. The instructor said to fix **all** reported issues to obtain full marks for the PythonTA portion; no numeric deduction schedule was supplied.

A displayed fallback explains that adding `output='pyta_report.html'` to the existing `check_all` call saves the report to that HTML file, which can be opened manually in a browser. The displayed Part 2 starter instructions also say to complete the function bodies; extra doctest examples are optional for your own understanding and testing, not required additions.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
