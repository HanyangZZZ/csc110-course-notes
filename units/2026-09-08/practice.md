# CSC110: Python expressions and Boolean logic — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Console launch, file execution, and comments

8 practice marks

(a) [3 marks] You are using Windows and have no VS Code terminal open. Describe how to open one, give the command that starts the Python console, and identify the prompt indicating that Python is ready for input.

(b) [1 mark] How can you run an entire open Python file using the play button?

(c) [2 marks] What expression result, if any, is displayed when you enter this line in the Python console? Explain.
```python
# calculate later
```

(d) [2 marks] Predict the displayed result and explain which part of the line Python ignores.
```python
6 + 3 # + 100
```

Answer requirements:

- Practice-only: Describe the Windows launch sequence.
- Practice-only: For comment-only input, distinguish no displayed result from a displayed None.

<details><summary>Hints</summary>

Start with the Terminal menu. Opening a terminal and starting Python are separate steps.

On Windows, the launch command is python. The Python readiness prompt has three greater-than signs.

A # starts a comment extending to the end of the line; an expression before it still runs.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Select Terminal → New Terminal, type `python`, and press Enter. Python indicates readiness with this prompt:
```text
>>>
```

(b) Click the play button at the top right to run the open Python file through the interpreter.

(c) No expression result is displayed; the console returns to its prompt. The entire line is a comment. It does not display `None`.

(d) Output:
```text
9
```
Python evaluates `6 + 3` and ignores `# + 100`.

- (a) 3 marks: Terminal → New Terminal (1); enter python and press Enter (1); identify >>> as Python's readiness prompt (1).
- (b) 1 mark: Click the top-right play button to run the entire open Python file.
- (c) 2 marks: No displayed expression result (1); explain that the whole line is a comment (1).
- (d) 2 marks: Output 9 (1); identify # + 100 as ignored while 6 + 3 executes (1).

</details>

### Numeric types, powers, and division

17 practice marks

(a) [12 marks] For each expression below, give its value and its Python type (`int` or `float`). Treat each line independently.
```python
7
7.0
9 ** 0.5
11 / 4
11 // 4
11 % 4
```

(b) [1 mark] Predict the console output:
```python
type(9 ** 0.5)
```

(c) Optional enrichment—negative division [4 enrichment marks, separate from the 13 core marks above; assessment relevance is unconfirmed]: Predict the value and type below. Explain downward rounding and why rounding toward zero would give the wrong answer.
```python
11 // -4
```

Answer requirements:

- Practice-only: Treat each expression independently and include both value and type in part (a).
- Practice-only: Part (c) is a separately labeled enrichment exercise; no negative-remainder rule is required.

<details><summary>Hints</summary>

A whole-number mathematical value can still have type float. Pay attention to decimal points.

** 0.5 computes a square root. / computes division, // rounds the quotient down, and % gives the remainder.

For enrichment, the ordinary quotient is −2.75. Which integer is immediately below it?

</details>

<details><summary>Worked solution and rubric</summary>

(a)

| Expression | Value | Type |
|---|---:|---|
| `7` | `7` | `int` |
| `7.0` | `7.0` | `float` |
| `9 ** 0.5` | `3.0` | `float` |
| `11 / 4` | `2.75` | `float` |
| `11 // 4` | `2` | `int` |
| `11 % 4` | `3` | `int` |

The square root of 9 is 3, with a floating-point result here. Dividing 11 by 4 gives 2.75; its floor is 2, and 11 contains two complete groups of 4 with remainder 3.

(b) Output:
```text
<class 'float'>
```

(c) The value is `-3`, of type `int`. The ordinary quotient is −2.75. Rounding downward gives −3, whereas rounding toward zero gives −2, which is greater than −2.75 and is not its floor.

- (a) 12 marks: For each of the six rows, award 1 for the correct value and 1 for the correct type.
- (b) 1 mark: Correct type-class output, <class 'float'>.
- (c) 4 optional enrichment marks (not core practice marks): Value -3 (1); type int (1); explain flooring −2.75 to −3 (1); contrast with incorrect rounding toward zero to −2 (1).

</details>

### Boolean literals, boundaries, and operators

10 practice marks

(a) [7 marks] Predict the console output of each independent expression. Distinguish Boolean values from a type-class display.
```python
True
type(True)
4.0 < 4.0
4.0 >= 4.0
not (6 == 8)
(2 < 5) and (9 != 9)
(2 < 5) or (9 == 9)
```

(b) [3 marks] Assume the lowercase name `true` is undefined. Explain why entering `true` does not produce the Boolean literal, state what happens instead, and give the correct spelling. Exact error-message wording is not required.

Answer requirements:

- Practice-only: Assume the lowercase name true is undefined.
- Practice-only: Exact traceback wording is not required; all Boolean operators here receive Boolean inputs.

<details><summary>Hints</summary>

Boolean literal spelling is case-sensitive. type(...) displays a class rather than a Boolean value.

Equal values fail a strict less-than comparison but satisfy greater-than-or-equal.

Evaluate the comparisons first. not reverses a Boolean, and requires both inputs to be True, and or includes the case where both inputs are True.

</details>

<details><summary>Worked solution and rubric</summary>

(a) Outputs, in order:
```text
True
<class 'bool'>
False
True
True
False
True
```
`True` is a Boolean value, whereas `type(True)` displays its class. Equal values make `4.0 < 4.0` false and `4.0 >= 4.0` true. `6 == 8` is false, so negating it gives true. The conjunction combines true and false, giving false. The disjunction combines true and true, giving true because Python's `or` is inclusive.

(b) Python is case-sensitive: `true` is not the Boolean literal `True`. With that name undefined, entering it raises a `NameError` rather than producing a Boolean value. The correct spelling is `True`.

- (a) 7 marks: Award 1 for each correct output in order; the second must identify the bool class rather than give True.
- (b) 3 marks: Explain case-sensitive literal spelling (1); identify an undefined-name error, with NameError wording optional (1); give True as the correction (1).

</details>

### Arithmetic precedence and float precision

8 practice marks

(a) [2 marks] Predict the output and show the arithmetic value of each side:
```python
(2 + 3 ** 2 * 4) == (2 + ((3 ** 2) * 4))
```

(b) [2 marks] Predict the output and show the arithmetic value of each side:
```python
(2 + 3 ** 2 * 4) == ((2 + 3) ** 2 * 4)
```

(c) [4 marks] Assess this claim: “Because 0.1 + 0.2 equals 0.3 in exact mathematics, Python's `0.1 + 0.2` must equal `0.3` exactly.” State whether the claim is correct, explain using finite storage why exact representation is not always possible, and clarify whether every float must be inexact. You do not need to give the full decimal output of the addition.

Answer requirements:

- Practice-only: Preserve the displayed operators and parentheses when evaluating.
- Practice-only: No full decimal addition output, internal encoding details, or precision-mitigation methods are required.

<details><summary>Hints</summary>

Evaluate parentheses first; otherwise exponentiation comes before multiplication, which comes before addition.

The common left side is 2 + 9 × 4. In part (b), the right side starts by adding 2 and 3.

Finite storage limits precision. This does not mean every floating-point value is inexact.

</details>

<details><summary>Worked solution and rubric</summary>

(a) The left side is 2 + 9 × 4 = 38. The right side is also 2 + (9 × 4) = 38. Output:
```text
True
```

(b) The left side is 38. The right side is 5² × 4 = 100. Output:
```text
False
```

(c) The claim is incorrect: Python's `0.1 + 0.2` does not equal `0.3` exactly. A computer has finite storage and cannot represent every real number exactly; approximations can introduce small arithmetic discrepancies. The infinite decimal expansion of 1/3 illustrates why finite representations can have limits, without implying that floats store decimal digits. Not every float is inexact: for example, `2.5` is represented exactly.

- (a) 2 marks: Output True (1); correctly compute both sides as 38 (1).
- (b) 2 marks: Output False (1); correctly compute the left side as 38 and right side as 100 (1).
- (c) 4 marks: Reject the claim and state that this addition is not exactly 0.3 (1); identify finite storage as a precision limitation (1); connect inexact representation to possible arithmetic discrepancies (1); explain that some floats are exact, with a correct example or equivalent clarification (1).

</details>

## Review quiz

### Interpreter and workflow selection

8 practice marks

(a) You are at a macOS shell prompt in an open VS Code terminal. What command launches the Python console, and what prompt indicates that it is ready for Python input? [2 marks]

(b) Distinguish Python the programming language from Python the interpreter. [2 marks]

(c) Choose the console or a file for each task, and briefly justify each choice: testing one expression with immediate feedback; preserving an organized program for later use. [2 marks]

(d) Predict the displayed result and explain which portion of the line Python ignores. [2 marks]
```python
8 * 2 # - 7
```

Answer requirements:

- Exercise-only scope: assume the terminal is already open at a macOS shell prompt; installation steps and versions are not requested.

<details><summary>Worked solution and rubric</summary>

(a) Enter `python3` at the shell prompt. The Python console readiness prompt is:
```text
>>>
```

(b) The language supplies the rules for writing Python code. The interpreter is the computer program that executes that code.

(c) Use the console to test one expression because it gives immediate feedback. Use a file to preserve an organized program for later use.

(d) Output:
```text
16
```
Python evaluates `8 * 2`; `# - 7` is a comment and is ignored.

- (a) 1 mark for python3; 1 mark for the three-angle-bracket Python prompt. Total: 2.
- (b) 1 mark for identifying the language as rules for writing code; 1 mark for identifying the interpreter as a program that executes code. Total: 2.
- (c) 1 mark for console with immediate-feedback justification; 1 mark for file with preservation/organization justification. Total: 2.
- (d) 1 mark for 16; 1 mark for identifying # - 7 as ignored comment text. Total: 2.

</details>

### Numeric expressions, types, and grouping

17 practice marks

(a) Give the value and Python type of each expression, in order. [10 marks]
```python
9 - 9.0
16 ** 0.5
14 / 5
14 // 5
14 % 5
```

(b) Give the displayed output of this expression. [1 mark]
```python
type(16 ** 0.5)
```

(c) Predict each Boolean result. For each comparison, show the numeric value of its left side and its right side. [6 marks]
```python
(3 + 2 ** 3 * 2) == (3 + ((2 ** 3) * 2))
(3 + 2 ** 3 * 2) == ((3 + 2) ** 3 * 2)
```

Answer requirements:

- Exercise-only restriction: evaluate the expressions exactly as written, preserving their operators and parentheses.
- Exercise-only scope: division operands are positive integers.

<details><summary>Worked solution and rubric</summary>

(a)

| Expression | Value | Type |
| --- | --- | --- |
| `9 - 9.0` | `0.0` | `float` |
| `16 ** 0.5` | `4.0` | `float` |
| `14 / 5` | `2.8` | `float` |
| `14 // 5` | `2` | `int` |
| `14 % 5` | `4` | `int` |

Mixed integer/float subtraction gives a float. The exponent `0.5` computes a square root. Ordinary division gives the quotient as a float; floor division gives 2, and the remainder is 4 because 14 = 5 × 2 + 4.

(b) Output:
```text
<class 'float'>
```

(c) Exponentiation precedes multiplication, which precedes addition. The common left side is 3 + 8 × 2 = 19.

First comparison: the right side is also 3 + 8 × 2 = 19. Output:
```text
True
```
Second comparison: the right side is 5 ** 3 × 2 = 125 × 2 = 250. Output:
```text
False
```

- (a) Award 1 mark for each correct value and 1 mark for each correct type across the five rows. Total: 10.
- (b) 1 mark for <class 'float'> rather than a numeric result or Boolean value. Total: 1.
- (c) First comparison: 1 mark for left-side value 19, 1 for right-side value 19, and 1 for True. Second comparison: 1 mark for left-side value 19, 1 for right-side value 250, and 1 for False. Total: 6.

</details>

### Comparison syntax and Boolean combinations

8 practice marks

(a) Predict the displayed result of each expression, in order. [5 marks]
```python
6.0 > 6.0
6 <= 6.0
not (not (7 != 7))
(3 == 3) and (8 < 2)
(3 == 3) or (8 == 8) or (10 < 1)
```

(b) Does the following input produce a Boolean comparison result or a syntax error? Explain the issue, then repair it to compare the two numbers for equality and give the repaired expression's result. [3 marks]
```python
8 = 3
```

Answer requirements:

- Exercise-only restriction: exact traceback wording is not required.
- Source-based scope: no explanation of the internal mechanism for mixed integer/float comparisons is required.

<details><summary>Worked solution and rubric</summary>

(a) Outputs, in order:
```text
False
True
False
False
True
```
Equal values do not satisfy strict greater-than, but do satisfy less-than-or-equal, including this integer/float comparison. `7 != 7` is False; the inner `not` makes it True and the outer `not` makes it False again. The `and` expression combines True with False, so it is False. The repeated inclusive `or` combines True, True, and False, so it is True.

(b) The input causes a syntax error, not a False comparison result. A single `=` is not the equality-comparison operator, and a numeric literal cannot occupy that left-hand position. Repair:
```python
8 == 3
```
Output:
```text
False
```

- (a) 1 mark per correct result in order: False, True, False, False, True. Total: 5.
- (b) 1 mark for syntax error; 1 mark for explaining that = is not equality comparison and the numeric-literal left side is invalid; 1 mark for repairing to 8 == 3 and giving False. Total: 3.

</details>

### Numeric equality and floating-point limitations

8 practice marks

(a) Predict the result below. Identify each operand's type and explain whether equal numeric values must have matching types. [3 marks]
```python
12.0 == 12
```

(b) A calculation of `(2 ** 0.5) ** 2` produced the value `2.0000000000000004`. Using that supplied value, predict the result below and explain why. [2 marks]
```python
2.0000000000000004 == 2
```

(c) In two or three sentences, explain why finite computer storage cannot guarantee exact representation of every real number. Use the infinite decimal expansion of one third as an analogy, and state whether this means every float value is inexact. [3 marks]

Answer requirements:

- Exercise-only restriction: use the supplied computed value; do not calculate or recall its digits independently.
- Exercise-only scope: no precision-mitigation methods or internal encoding details are requested.

<details><summary>Worked solution and rubric</summary>

(a) Output:
```text
True
```
`12.0` has type `float`; `12` has type `int`. Their numeric values are equal despite their different types, so matching types are not necessary for numeric equality.

(b) Output:
```text
False
```
The supplied computed value is slightly greater than 2, not exactly equal to it. Exact equality does not treat nearby values as equal.

(c) One third needs infinitely many digits when written as a decimal, illustrating why a finite representation can fall short of an exact real value. Computer storage is finite, so floating-point representations cannot represent every real number exactly, and approximations can introduce errors in calculations. This does not mean every float is inexact: some values, such as 2.5, are exactly representable.

- (a) 1 mark for True; 1 mark for both types correctly identified; 1 mark for explaining that equal numeric values can have different types. Total: 3.
- (b) 1 mark for False; 1 mark for explaining that the supplied value differs from 2 despite being nearby. Total: 2.
- (c) 1 mark for the infinite-decimal one-third analogy; 1 mark for connecting finite storage to limits on exact representation and possible approximation error; 1 mark for explicitly rejecting the claim that every float is inexact. Total: 3.

</details>

## Challenge quiz

### Execution diagnosis and numeric representation

10 practice marks

(a) [4 marks] A Linux learner enters the following at a shell prompt and assumes Python evaluated it:
```python
15 - 15.0 # + 1
```
Explain why the shell prompt does not establish that Python is ready. Give the corrected sequence, including opening a VS Code terminal if needed, launching Python, recognizing its readiness prompt, and entering the expression.

(b) [2 marks] Predict the expression's value and type when entered in the Python console. Explain the effect of the comment.

(c) [2 marks] Explain how the play button runs an open `.py` file. Does file execution require a different interpreter from console execution?

(d) [2 marks] Identify the type of this literal without performing arithmetic. Explain whether its length alone requires a different numeric type, and qualify any claim about storage:
```python
111111111111111111111111
```

Answer requirements:

- Practice-only restriction: do not predict a shell response to the mistaken input.
- Practice-only restriction: identify the large literal's type without arithmetic; do not reconstruct the contents of the open file.

<details><summary>Worked solution and rubric</summary>

(a) A shell prompt indicates that the shell is ready, not necessarily the Python interpreter. Open `Terminal → New Terminal` if needed. At the Linux shell, enter `python3`. Recognize the Python console readiness prompt:
```text
>>>
```
Then enter:
```python
15 - 15.0 # + 1
```

(b) Output:
```text
0.0
```
Its type is `float`: subtracting these mixed integer and float operands produces a float. The `# + 1` portion is ignored, so no addition occurs.

(c) The play button runs the open Python file with the interpreter, executing the file's code. This is another way to supply code to an interpreter; it does not inherently require a different interpreter from the console.

(d) The literal has type `int`. Python integers can represent very large integers without switching to a different numeric type merely because they are long. This does not imply unlimited physical storage.

- (a) 4 marks: distinguish shell readiness from Python readiness (1); open a terminal if needed (1); give `python3` (1); identify `>>>` and enter the expression there (1).
- (b) 2 marks: give `0.0` and type `float` (1); explain that the comment is ignored and the mixed subtraction produces a float (1).
- (c) 2 marks: explain that the play button executes the open file through the interpreter (1); state that a different interpreter is not inherently required (1).
- (d) 2 marks: identify `int` and explain that length alone does not require another numeric type (1); reject unlimited physical storage (1).

</details>

### Division, grouping, and negative-operand caveats

10 practice marks

(a) [3 marks] Predict each value. For ordinary division, an exact quotient description is sufficient; all displayed float digits are not required.
```python
17 / 6
17 // 6
17 % 6
```

(b) [3 marks] Evaluate both expressions and explain why their values differ:
```python
1 + 2 ** 3 * 3
(1 + 2) ** 3 * 3
```

(c) Optional enrichment [2 enrichment marks, separate from the 8 core marks in (a), (b), and (d); assessment relevance is unconfirmed]: Predict the following and explain why rounding toward zero gives the wrong result:
```python
17 // -6
```

(d) [2 marks] The lecture demonstrated that the following produced the output shown:
```python
5 % -2
```
Output:
```text
-1
```
Use this observation to critique the claim that a remainder is always positive. State what this single observation does not establish.

Answer requirements:

- Practice-only restriction: preserve the given operators and parentheses.
- Practice-only restriction: use the supplied negative-remainder observation only; do not calculate another negative-remainder example or state a general sign rule.

<details><summary>Worked solution and rubric</summary>

(a) In order: a `float` approximation to the quotient 17/6 (about 2.8333); `2`; `5`. Six fits into seventeen twice, leaving five.

(b) The first expression evaluates the power first: 2 ** 3 gives 8, multiplication gives 24, and addition gives 25. The second evaluates the parenthesized sum first: 1 + 2 gives 3, the power gives 27, and multiplication gives 81. Thus the values are 25 and 81, not equal.

(c) Output:
```text
-3
```
The quotient is approximately −2.8333. Floor division rounds downward to −3, not toward zero to −2.

(d) The claim is false: the observed remainder −1 is negative. This is a counterexample to an always-positive claim, but one observation alone does not establish a general remainder-sign rule for all operands.

- (a) 3 marks: float approximation to 17/6 or an appropriate quotient description (1); floor-division value 2 (1); remainder 5 (1).
- (b) 3 marks: first value 25 (1); second value 81 (1); explain the power–multiplication–addition order and how parentheses change it (1).
- (c) 2 optional enrichment marks (not core challenge marks): value −3 (1); explain downward rounding versus rounding toward zero (1).
- (d) 2 marks: reject the claim using the supplied negative remainder (1); distinguish the counterexample from a general sign rule (1).

</details>

### Multistage Boolean reasoning and corrected grouping misconception

8 practice marks

(a) [3 marks] Predict the output, showing the value of each comparison and each Boolean-combination stage:
```python
not (((4.0 <= 4) and (7 != 7)) or (2 > 9))
```

(b) [3 marks] Predict the output, showing the value of each comparison and each `or` stage:
```python
((9 == 9.0) or (5 < 5)) or (1 > 6)
```

(c) [2 marks] Compare the outputs of these expressions. Assess the claim that omitting the parentheses in this particular case necessarily causes an error:
```python
not 9 == 8
not (9 == 8)
```

Answer requirements:

- Practice-only restriction: show intermediate Boolean values, not just final outputs.
- Scope restriction: apply the demonstrated `not`–comparison grouping relationship only; no general Boolean precedence rules are required.

<details><summary>Worked solution and rubric</summary>

(a) The comparisons yield `True`, `False`, and `False`, respectively. The `and` stage is `True and False`, giving `False`. The `or` stage is `False or False`, giving `False`. Finally, `not False` gives:
```text
True
```

(b) The comparisons yield `True`, `False`, and `False`. Equal numeric values can compare equal despite their different numeric types. The inner `or` gives `True`; the outer `or` also gives:
```text
True
```

(c) Both outputs are:
```text
True
```
In this case, `not 9 == 8` is interpreted as `not (9 == 8)`. The equality is `False`, and negation makes it `True`. The omitted parentheses do not cause an error here; explicit parentheses make the intended grouping clearer. This does not justify removing parentheses from every expression.

- (a) 3 marks: comparison values True, False, False (1); `and` and `or` stages both False (1); final output True (1).
- (b) 3 marks: comparison values True, False, False (1); inner `or` True (1); outer `or` and final output True (1).
- (c) 2 marks: both outputs True (1); explain the demonstrated grouping relationship and reject the error claim for this case without generalizing to every expression (1).

</details>

### Integrated equality and limits of generalization

10 practice marks

(a) [4 marks] Predict the output. Show the intermediate arithmetic steps, both arithmetic values, the two comparison results, and the final conjunction:
```python
((4 + 2 ** 3 * 2) == (4 + ((2 ** 3) * 2))) and (10.0 == 10)
```

(b) [2 marks] Critique the claim: “Every mathematically reversible float calculation necessarily restores its input exactly.” Explain using finite storage and the infinite decimal expansion of 1/3 as an analogy.

(c) [2 marks] Critique the claim: “Finite representation means no number can be represented exactly.” Give one exact float example.

(d) [2 marks] Critique the claim: “Successful numeric comparisons imply every future pair of types can be compared meaningfully.” Relate your answer to the lecture's set/integer warning without proposing a Python operation on those types.

Answer requirements:

- Practice-only restriction: explain float limitations conceptually; no new computed float discrepancy or mitigation method is required.
- Scope restriction: discuss the set/integer warning only; do not supply an operator, runtime result, or error class for future-type operations.

<details><summary>Worked solution and rubric</summary>

(a) The power gives 8 and the multiplication gives 16. Each arithmetic side therefore has value 20, so the first comparison is `True`. The second comparison is also `True`: 10.0 and 10 have equal numeric values despite different types. The conjunction is `True and True`, giving:
```text
True
```

(b) The claim is false. Finite storage cannot represent every real number exactly, so approximations can prevent a mathematically reversing calculation from restoring an input exactly. The infinite decimal expansion of 1/3 illustrates why a finite written representation can be insufficient; it is an analogy, not a claim that floats store decimal digits.

(c) The claim is false. An inability to represent every number exactly does not mean that no number is exact. For example, the float `2.5` represents its value exactly.

(d) The claim is false. Successful comparisons of numeric types do not establish meaningful comparisons for every pair of types. The lecture warned that a set and an integer would not necessarily support a meaningful comparison; that warning limits the generalization without requiring a particular operation or runtime outcome.

- (a) 4 marks: both arithmetic values 20 with power and multiplication steps (1); first comparison True (1); numeric comparison True (1); final conjunction True (1).
- (b) 2 marks: reject guaranteed exact restoration because approximation can intervene (1); explain finite storage using the infinite decimal expansion of 1/3 as an analogy (1).
- (c) 2 marks: distinguish not all numbers being exact from no numbers being exact (1); supply an exact float example such as 2.5 (1).
- (d) 2 marks: reject generalization from numeric comparisons to all type pairs (1); relate this to the set/integer warning without inventing an operation or runtime outcome (1).

</details>
