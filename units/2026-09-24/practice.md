# CSC110: Conditionals, Boolean simplification, and PythonTA — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Quantifier order

6 practice marks

Let A = {Breanna, Malena, Patrick, Ella} and B = {Sophia, Thelonious, Stanley, Laura}. After the lecture’s change, the people loved are: Breanna → {Thelonious, Stanley}; Malena → {Thelonious, Stanley, Laura}; Patrick → {Stanley}; Ella → {Laura}. Translate and evaluate (i) ∀a ∈ A, ∃b ∈ B, Loves(a,b), and (ii) ∃b ∈ B, ∀a ∈ A, Loves(a,b). Justify both results.

Answer requirements:

- Use only the stated domains and relationships.

<details><summary>Hints</summary>

Does each person have at least one choice? Is any single choice shared by all four?

</details>

<details><summary>Worked solution and rubric</summary>

(i) Everyone in A loves someone in B: true. Choose Thelonious for Breanna and Malena, Stanley for Patrick, and Laura for Ella. (ii) Someone in B is loved by everyone in A: false. Sophia has no supporters; Patrick does not love Thelonious; Ella does not love Stanley; Breanna does not love Laura. These are all four possible choices of b, so none can be a shared witness.

- 1 mark: correct translation of (i).
- 1 mark: true with a valid witness for every a.
- 1 mark: correct translation of (ii).
- 1 mark: false for (ii).
- 2 marks: rules out all four candidates and explains why that exhausts B.

</details>

### Tracing and doctest design

6 practice marks

A function body has line 6: `if age < 18:`, line 7: `return 'Too young to vote'`, line 8: `else:`, and line 9: `return 'Allowed to vote'`, with returns indented in their branches. Give the body-line trace and result for ages 17 and 18. Explain why line 8 is omitted. Why are doctests for ages 1 and 2 alone uninformative about the full behavior? Suggest a better pair.

Answer requirements:

- List only function-body computation lines, not the header.

<details><summary>Hints</summary>

The comparison executes even when false.

Choose examples from different branches.

</details>

<details><summary>Worked solution and rubric</summary>

Age 17: lines 6,7; result 'Too young to vote', because 17 < 18 is true. Age 18: lines 6,9; result 'Allowed to vote', because 18 < 18 is false. Line 8 marks the alternative branch but performs no separate computation. Ages 1 and 2 both show only the too-young branch. Ages 1 and 20, for example, show both possible results.

- 2 marks: correct trace and result for 17.
- 2 marks: correct trace and result for 18.
- 1 mark: explains the structural role of else.
- 1 mark: identifies repeated-case weakness and supplies a pair covering both cases.

</details>

### Implementing a two-case contract

5 practice marks

Implement `format_name(given_name: str, family_name: str) -> str` using an if statement. Return only the given name when the family name is empty; otherwise return the family name, a comma and one space, then the given name. Give results for ('Cher', '') and ('Cherilyn', 'Sarkisian').

Answer requirements:

- Use an if statement.
- Preserve the exact comma-and-space formatting.

<details><summary>Hints</summary>

Compare the family name with ''.

The empty-family-name case must not add punctuation.

</details>

<details><summary>Worked solution and rubric</summary>

```python
def format_name(given_name: str, family_name: str) -> str:
    if family_name == '':
        return given_name
    else:
        return family_name + ', ' + given_name
```
The first call returns 'Cher'; the second returns 'Sarkisian, Cherilyn'. The empty-string check prevents an unwanted leading comma.

- 1 mark: correct empty-string condition.
- 1 mark: returns only given_name in that case.
- 1 mark: correct non-empty name order.
- 1 mark: exact separator and valid branch structure.
- 1 mark: both example results correct.

</details>

### Direct Boolean returns

6 practice marks

Write single-return bodies, without if statements, for (a) `is_odd(n: int) -> bool`, (b) a function true exactly for integer ages 13 through 18 inclusive, and (c) a function checking whether `prefix` begins both `s1` and `s2`. Explain why each expression matches the contract.

Answer requirements:

- No if/elif/else statements.
- Return Boolean expressions, not strings.

<details><summary>Hints</summary>

Oddness is the opposite of remainder zero.

The age function must include both endpoints.

Both prefix tests must pass.

</details>

<details><summary>Worked solution and rubric</summary>

(a) `return n % 2 != 0`: nonzero remainder on division by 2 means odd. (b) `return 13 <= age <= 18`: both lower and upper bounds hold, including equality. (c) `return s1.startswith(prefix) and s2.startswith(prefix)`: a common prefix must begin both strings, not just one.

- 2 marks: correct oddness expression and explanation.
- 2 marks: correct inclusive range and explanation.
- 2 marks: correct conjunction of startswith tests and explanation.

</details>

## Review quiz

### Constrained programming and boundaries

10 practice marks

Original programming self-test in the evidenced worksheet-style format: implement `get_status(scheduled: int, estimated: int) -> str` with if/elif/else. Assume same-day integer hours 0–23. Early or equal is 'On time', positive lateness below four hours is 'Delayed', and four or more hours late is 'Cancelled'. Give results for (10,9), (10,10), (10,13), and (10,14), and explain why your delayed branch cannot accidentally include early flights.

Answer requirements:

- Use if/elif/else.
- Do not add overnight handling.
- Use the lecture’s four-hour boundary.

<details><summary>Hints</summary>

Handle on-time and early departures before using the shortened lateness check.

</details>

<details><summary>Worked solution and rubric</summary>

```python
def get_status(scheduled: int, estimated: int) -> str:
    if scheduled >= estimated:
        return 'On time'
    elif estimated - scheduled < 4:
        return 'Delayed'
    else:
        return 'Cancelled'
```
The results in order are 'On time', 'On time', 'Delayed', and 'Cancelled'. Reaching the elif means the first comparison was false, so estimated > scheduled and lateness is positive. Combined with < 4, this describes only late flights below the cancellation threshold.

- 2 marks: correct first condition and result.
- 2 marks: correct delayed comparison and result.
- 1 mark: correct cancellation fallback.
- 1 mark: valid if/elif/else implementation.
- 2 marks: four correct example results, 0.5 each.
- 2 marks: explains failed first condition and consequent positive lateness.

</details>

### Return types and ties

8 practice marks

Original multipart written/programming self-test: implement `larger_sum(nums1: list, nums2: list) -> list` for lists of floats, returning the original list with larger sum and choosing nums1 on ties. Explain why `return max(sum(nums1), sum(nums2))` is not equivalent. Give the result for [2.0,1.0] and [1.0,2.0]. Explain when your else can be removed.

Answer requirements:

- Use an if statement.
- Return an original list, not a sum.
- Do not use additional selection techniques beyond those taught.

<details><summary>Hints</summary>

Tie handling belongs in the comparison.

A return exits the function, not just the branch.

</details>

<details><summary>Worked solution and rubric</summary>

```python
def larger_sum(nums1: list, nums2: list) -> list:
    if sum(nums1) >= sum(nums2):
        return nums1
    else:
        return nums2
```
The >= includes equal sums in the nums1 branch. The attempted max expression returns a numeric sum rather than an input list. The given lists both sum to 3.0, so return the original [2.0,1.0]. The else can be replaced with an unindented `return nums2` after the if because a true condition already returns nums1 and exits the function; only a false condition reaches the fallback.

- 2 marks: compares sums with correct tie behavior.
- 2 marks: returns the correct original lists in valid code.
- 1 mark: explains number-versus-list contract mismatch.
- 1 mark: correct tie example.
- 2 marks: correct unindenting transformation and early-return justification.

</details>

### Module execution and checking workflow

8 practice marks

Original written self-test: `status_demo.py` contains only `print(__name__)`. In separate fresh runs, what is printed when (a) this file is run directly and (b) another file imports status_demo? Now place that print inside `if __name__ == '__main__':` and give both outcomes again. Explain why checks go in this block, and why Shift+Enter is inappropriate for the supplied PythonTA workflow.

Answer requirements:

- Assume an ordinary module filename and separate fresh runs.
- Use the exact double-underscore names.

<details><summary>Hints</summary>

Imported modules have their module name; the directly run program has a special name.

</details>

<details><summary>Worked solution and rubric</summary>

Unguarded: (a) prints __main__; (b) prints status_demo. Guarded: (a) still prints __main__; (b) prints nothing from status_demo, because its name is not '__main__'. The main block lets direct runs perform checks without performing them when another program merely imports the functions. Shift+Enter copies selected text into a terminal rather than running that source file; PythonTA needs the file context, so run the whole file with the supplied checking lines enabled.

- 2 marks: both unguarded outputs correct.
- 2 marks: both guarded outcomes correct.
- 2 marks: explains direct-run/import distinction and purpose of guarding checks.
- 2 marks: explains Shift+Enter’s missing file context and appropriate workflow.

</details>

### Short-circuit evaluation

10 practice marks

Original written/programming self-test: write a single-return implementation of `same_corresponding_values(mapping: dict, key1: str, key2: str) -> bool`, false if either key is missing and otherwise true exactly when their values match. Trace it for mapping = {'a':4,'b':4,'c':7} with keys ('a','b'), ('a','c'), and ('x','b'). Why is placing the equality first unsafe?

Answer requirements:

- No if statements.
- Use dictionary membership checks and short-circuit evaluation.
- Do not use get or exception handling.

<details><summary>Hints</summary>

The lookups must be reached only after both membership checks succeed.

</details>

<details><summary>Worked solution and rubric</summary>

```python
def same_corresponding_values(mapping: dict, key1: str,
                              key2: str) -> bool:
    return (key1 in mapping and key2 in mapping
            and mapping[key1] == mapping[key2])
```
For ('a','b'), both membership checks pass and 4 == 4 is true, so return True. For ('a','c'), both pass but 4 == 7 is false, so return False. For ('x','b'), the first membership test is false and evaluation stops; no lookup is attempted, so return False safely. Equality first would attempt mapping['x'] before any guard could help, raising KeyError.

- 3 marks: correct ordered expression with both membership tests and equality.
- 2 marks: explains left-to-right stopping at the first false operand.
- 3 marks: one mark for each correct trace/result.
- 2 marks: identifies premature missing-key lookup and KeyError in reordered version.

</details>

## Challenge quiz

### Counterexamples and branch-order reasoning

10 practice marks

A proposed simplification is:
```python
if estimated - scheduled < 4:
    return 'Delayed'
elif estimated <= scheduled:
    return 'On time'
else:
    return 'Cancelled'
```
Under the lecture’s same-day hour contract, find an early-flight counterexample and an exactly-on-time counterexample. Explain why the on-time branch cannot ever execute for an input that satisfies its condition. Repair the program with cancellation checked first, and explain why all allowed cases are covered.

Answer requirements:

- Keep the original status definitions and four-hour boundary.
- Use cancellation as the first branch in the repaired program.
- No overnight interpretation.

<details><summary>Hints</summary>

If estimated <= scheduled, what can you say about their difference?

After excluding cancellation, positive lateness has an upper bound.

</details>

<details><summary>Worked solution and rubric</summary>

Counterexamples: (10,9) has lateness -1 and incorrectly returns 'Delayed'; (10,10) has lateness 0 and also incorrectly returns 'Delayed'. Any input satisfying estimated <= scheduled has difference <= 0, hence < 4, so the first branch captures it before the on-time elif can run.
```python
if estimated - scheduled >= 4:
    return 'Cancelled'
elif estimated > scheduled:
    return 'Delayed'
else:
    return 'On time'
```
The second branch is reached only for lateness < 4; its own condition additionally makes lateness positive. The else has lateness < 4 and not positive, hence <= 0, which means early or equal. These intervals—at least 4, strictly between 0 and 4, and at most 0—cover every permitted difference.

- 2 marks: two valid counterexamples with incorrect outcomes.
- 2 marks: explains why every would-be on-time input is captured first.
- 3 marks: correct cancellation-first repaired implementation.
- 3 marks: establishes each remaining interval and exhaustive coverage.

</details>

### Exhaustive cases and assumptions

10 practice marks

Implement rock_paper_scissors using one tie branch, one combined Player 1 win branch, and a Player 2 fallback. Explain why the fallback is justified by the stated domain. Then assess the claim: 'Because the fallback is correct, the function correctly handles any pair of strings.' Give a counterexample to that stronger claim. Name one other taught case structure that could implement the valid-input contract.

Answer requirements:

- Valid inputs are 'rock', 'paper', or 'scissors'.
- Use only taught comparisons and Boolean operators.
- Do not add input-validation code; evaluate the stronger claim instead.

<details><summary>Hints</summary>

Count the ordered pairs.

A fallback relies on which alternatives have been ruled out, not on its label.

</details>

<details><summary>Worked solution and rubric</summary>

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
There are nine permitted ordered pairs: three ties, three Player 1 wins, and three Player 2 wins. The first two branches eliminate the first six, leaving exactly the Player 2 cases. The stronger claim is unsupported: ('lizard','rock') reaches the fallback despite no rule declaring rock the winner against lizard. This is outside the contract, so it does not refute correctness on the promised inputs; it refutes the claimed extension to arbitrary strings. Another taught structure is to branch first on Player 1’s move and then use nested branches for Player 2’s move.

- 4 marks: correct tie, three winning pairs, grouping, and result strings.
- 3 marks: counts nine pairs and explains the exhaustive three-way partition.
- 2 marks: valid out-of-domain counterexample and distinction from a valid-input bug.
- 1 mark: names a correct taught alternative structure.

</details>

### Evaluating test support

8 practice marks

A faulty porridge function uses `if temperature >= 50.0` for too hot, `elif temperature < 49.0` for too cold, and else for just right. It passes tests at 65.5, 30.0, and 49.5. Explain why these tests cover all three branches yet do not establish correctness. Supply a failing boundary test with its required exact output, repair the comparison, and explain the just-right interval. Relate this to the lecture’s warning about choosing examples.

Answer requirements:

- Use the lecture contract: strictly above 50 is too hot and strictly below 49 is too cold.
- The branch-coverage distinction is a supplementary reading application.

<details><summary>Hints</summary>

Which input distinguishes > from >=?

</details>

<details><summary>Worked solution and rubric</summary>

65.5 reaches the hot branch, 30.0 the cold branch, and 49.5 the middle branch, so all three branches execute across the tests. But none tests equality to 50.0, where the bug lies. `porridge_satisfaction(50.0)` must return 'This porridge is just right! Yum!!', whereas the faulty version returns the hot message. Replace >= 50.0 with > 50.0. The final else then means temperature <= 50.0 and temperature >= 49.0, so the just-right interval is [49.0,50.0]. The lecture recommends examples showing different behavior; the supplementary reading adds that even visiting every branch does not cover every important case or prove correctness.

- 2 marks: maps the three tests to three branches.
- 2 marks: correct failing boundary test and exact expected output.
- 1 mark: correct comparison repair.
- 1 mark: explains both inclusive endpoints.
- 2 marks: distinguishes useful case coverage from proof of correctness and attributes the supplementary extension.

</details>

### Learning strategy and PythonTA diagnosis

10 practice marks

A student says: 'My working memory is fixed, so practice cannot improve my programming. I ran Part 2’s PythonTA lines with Shift+Enter; after deleting the TODO comments, I can assume the assignment is correct.' Evaluate both claims. Use chunking and cognitive load for the first. For the second, give the proper workflow and explain how to respond to missing-return, unused-argument, and blank-line reports without changing required function signatures.

Answer requirements:

- Do not invent a numeric marking penalty.
- Preserve the supplied assignment configuration and required headers.
- Do not claim PythonTA proves correctness.

<details><summary>Hints</summary>

A fixed number of chunks need not contain a fixed amount of knowledge.

A missing placeholder comment is not the same as a finished function.

</details>

<details><summary>Worked solution and rubric</summary>

The memory claim confuses capacity with effective use. Practice builds long-term knowledge and allows larger meaningful chunks, as familiar gmail.com can be one unit while unfamiliar text is several. Experience also helps identify relevant information and reduce extraneous load; clear organization can help without removing the task’s intrinsic requirements.
For Part 2, uncomment the supplied PythonTA block, preserve its configuration, and run the whole file. Shift+Enter does not supply the same source-file context. Inspect the browser report, fix issues, and rerun before submission. A missing return calls for implementing the function’s required result; an unused argument calls for checking unfinished logic or misspellings while preserving the required signature; the demonstrated spacing report calls for the missing blank line so two separate the definition from following top-level code. Complete the TODO work before removing its comment. A clean report addresses checker findings but does not prove the contract is satisfied; check actual results with appropriate tests too.

- 2 marks: explains long-term learning and increased information per chunk.
- 2 marks: distinguishes intrinsic/extraneous load and how experience or organization helps.
- 2 marks: correct whole-file, supplied-configuration PythonTA workflow.
- 3 marks: appropriate response to each of the three report types without changing required signatures.
- 1 mark: rejects TODO deletion or a clean report as proof of correctness.

</details>
