# CSC110: Tabular data, dates, and CSV files — practice

Study questions; not official assessments. Marks are for self-checking.

## Practice

### Nested-list indexing and filtering

8 practice marks

Use the original eight-row marriage_data from the notes. Give the values of len(marriage_data), marriage_data[5], and marriage_data[6][2]. Then write a filtering list comprehension returning the full rows with more than 300 licences and identify their IDs. Explain why >= 300 is not the requested condition.

Answer requirements:

- Use the original eight-row sample, not the later dataset without Toronto.
- Use a filtering list comprehension for the row selection.

<details><summary>Hints</summary>

The outer index chooses a row; index 2 within a row chooses its count.

More than excludes equality.

</details>

<details><summary>Worked solution and rubric</summary>

len(marriage_data) is 8. marriage_data[5] is [1662, 'NY', 150, datetime.date(2011, 2, 1)]. marriage_data[6][2] is 154. The comprehension is [row for row in marriage_data if row[2] > 300], returning the rows with IDs 1660 and 1664. Keeping row before for retains each full row. >= 300 would also accept exactly 300, which does not satisfy more than 300; this sample happens to have no count of exactly 300.

- 1 mark: outer length 8.
- 2 marks: complete sixth row.
- 1 mark: count 154.
- 2 marks: comprehension preserves rows and uses row[2] > 300.
- 1 mark: IDs 1660 and 1664.
- 1 mark: explains the equality boundary even though the sample cannot distinguish the two operators.

</details>

### Constraints and membership

10 practice marks

Write expressions checking (a) every row has four cells, (b) every ID is a positive int, (c) every centre is one of the four permitted codes, and (d) at least one row is for Toronto. Explain with a counterexample why membership in 'TO ET NY SC' is not a correct replacement for (c).

Answer requirements:

- Use all or any with comprehensions.
- For indexed checks, assume the rows have the required cells.

<details><summary>Hints</summary>

isinstance tests type.

Membership in a string asks about substrings.

</details>

<details><summary>Worked solution and rubric</summary>

(a) all({len(row) == 4 for row in marriage_data})
(b) all({isinstance(row[0], int) and row[0] > 0 for row in marriage_data})
(c) all({row[1] in {'TO', 'ET', 'NY', 'SC'} for row in marriage_data})
(d) any([row[1] == 'TO' for row in marriage_data])
For example, 'TO ET' in 'TO ET NY SC' is True, but 'TO ET' is not a permitted code. String membership accepts this substring; set membership compares against the four complete codes.

- 2 marks: correct universal shape expression.
- 2 marks: combines integer type and positivity for every ID.
- 2 marks: uses a collection of whole permitted codes.
- 2 marks: existential Toronto check with any.
- 2 marks: valid counterexample and explanation of substring versus element membership.

</details>

### Date construction and conversion

8 practice marks

Implement str_to_date for valid slash-separated year/month/day strings using split. Explain why it also handles '2014/1/1', state its result for that input, and state the value and type of datetime.date(2026, 10, 6) - datetime.date(2026, 10, 5).

Answer requirements:

- Assume valid calendar dates and three slash-separated integer components.
- Use split and int rather than fixed-position slicing.

<details><summary>Hints</summary>

Splitting returns strings.

A date difference represents a duration.

</details>

<details><summary>Worked solution and rubric</summary>

def str_to_date(date_string: str) -> datetime.date:
    """Convert a valid slash-separated year/month/day string to a date."""
    components = date_string.split('/')
    return datetime.date(int(components[0]), int(components[1]), int(components[2]))

Splitting locates separators rather than relying on the lengths of month and day fields, so '2014/1/1' gives ['2014', '1', '1'] and returns datetime.date(2014, 1, 1). The subtraction returns datetime.timedelta(days=1), of type datetime.timedelta, not int.

- 1 mark: appropriate signature and docstring.
- 1 mark: splits on '/'.
- 3 marks: converts all components and uses year/month/day order.
- 1 mark: explains variable-width handling.
- 1 mark: correct 2014 date result.
- 1 mark: correct subtraction value and type.

</details>

### Accumulator tracing

8 practice marks

Trace this function on [4, -2, 1]. Identify the loop variable and accumulator; record the accumulator before the loop and after every iteration; give the returned value and the result for an empty list.

def sum_of_squares(numbers: list[int]) -> int:
    sum_so_far = 0
    for number in numbers:
        sum_so_far = sum_so_far + number ** 2
    return sum_so_far

Answer requirements:

- Show the intermediate accumulated values, not just the final sum.

<details><summary>Hints</summary>

Square the negative value before adding it.

An empty list executes the body zero times.

</details>

<details><summary>Worked solution and rubric</summary>

The loop variable is number; the accumulator is sum_so_far. Initially sum_so_far is 0. With number = 4, it becomes 0 + 16 = 16. With number = -2, it becomes 16 + 4 = 20. With number = 1, it becomes 20 + 1 = 21. The function returns 21 after the loop. For [], no updates occur, so it returns its initialized value 0.

- 1 mark: identifies number.
- 1 mark: identifies sum_so_far.
- 1 mark: initial value 0.
- 3 marks: correct values 16, 20, 21 with corresponding elements.
- 1 mark: returns 21 after all iterations.
- 1 mark: empty input returns 0 with zero-iteration explanation.

</details>

## Review quiz

### CSV consumption self-test

8 practice marks

Original self-test, not a claimed test-format replica. A CSV contains exactly these three lines:
ID,Civic Centre,Marriage Licenses Issued,Time Period
1657,ET,80,2011/01/01
1658,NY,136,2011/01/01

Inside an open-file block, a fresh reader executes:
header = next(reader)
data = [row for row in reader]
again = [row for row in reader]
last = next(reader)

Give header, data[0], and again. Explain what happens on the last line and whether this deletes anything from the CSV.

Answer requirements:

- Assume the file opens successfully and the reader starts at its beginning.

<details><summary>Hints</summary>

Each read starts at the current position.

All cell values supplied by this reader are strings.

</details>

<details><summary>Worked solution and rubric</summary>

header is ['ID', 'Civic Centre', 'Marriage Licenses Issued', 'Time Period']. data[0] is ['1657', 'ET', '80', '2011/01/01']. The data comprehension consumes the two remaining rows, so again is []. The last line raises StopIteration instead of assigning a value to last: next requested a row after the reader was exhausted. Consumption advances the reading position; it does not delete the CSV contents.

- 2 marks: complete header list.
- 2 marks: first data row with string-valued numeric cells.
- 1 mark: again is [].
- 2 marks: StopIteration and exhaustion explanation.
- 1 mark: distinguishes reader advancement from deletion.

</details>

### Average function self-test

10 practice marks

Original self-test. Implement average_licenses_issued(marriage_data, cc) using the lecture's list-comprehension approach and len(filtered) == 0 guard. Return 0.0 if there are no matches. Explain why the guard is needed and calculate the original sample's ET result.

Answer requirements:

- Assume correctly formatted rows.
- Keep matching counts in a list.
- Include a docstring.

<details><summary>Hints</summary>

The denominator is the number of matching observations.

The ET counts are 80 and 109.

</details>

<details><summary>Worked solution and rubric</summary>

def average_licenses_issued(marriage_data: list[list], cc: str) -> float:
    """Return cc's mean licence count, or 0.0 when cc has no rows."""
    filtered = [row[2] for row in marriage_data if row[1] == cc]
    if len(filtered) == 0:
        return 0.0
    return sum(filtered) / len(filtered)

No matches makes the denominator zero; without the early return, division fails. Reaching the final return guarantees a nonzero length. The ET result is (80 + 109) / 2 = 94.5. The fallback is a specified output for missing data, not a mathematical empty-sample average.

- 1 mark: signature and useful docstring.
- 3 marks: filters by centre and extracts count values into a list.
- 2 marks: correct empty guard and 0.0 return.
- 2 marks: correct sum/length return outside the guard.
- 1 mark: division-by-zero explanation.
- 1 mark: ET calculation 94.5.

</details>

### Universal checks self-test

10 practice marks

Original self-test. Implement civic_centre_meets_threshold(data, civic_centre, num) using all and a filtering comprehension. Assume valid rows, an allowed centre, and num > 0. Only recorded rows matter. Give and explain the results for original-sample ET with num = 80 and num = 100, and for a centre with no rows. Does True prove that centre appears in the data?

Answer requirements:

- For this question, absent centres are explicitly allowed.
- Do not add checks for missing calendar months.

<details><summary>Hints</summary>

At least includes equality.

all of an empty collection is True.

</details>

<details><summary>Worked solution and rubric</summary>

def civic_centre_meets_threshold(data: list[list], civic_centre: str, num: int) -> bool:
    """Return whether every recorded row for civic_centre meets num."""
    return all({row[2] >= num for row in data if row[1] == civic_centre})

ET with 80 returns True because both 80 and 109 are at least 80. ET with 100 returns False because 80 fails, even though 109 passes. An absent centre returns True: there is no selected row violating the threshold. This vacuous truth does not prove existence or activity outside the recorded rows.

- 1 mark: function signature/docstring.
- 2 marks: selects only the queried centre.
- 2 marks: >= comparison aggregated with all.
- 1 mark: True for ET/80 with equality included.
- 1 mark: False for ET/100 with failing observation identified.
- 2 marks: absent-centre True and vacuity explanation.
- 1 mark: explicitly rejects existence or unrecorded-activity inference.

</details>

### CSV function self-test

10 practice marks

Original self-test. Write read_csv_file(filename) returning a tuple consisting of the header list and the remaining raw row lists. Include its annotation and a docstring precondition. In the lecture's project-root working directory, what filename argument reaches the CSV under lectures/week05? Explain why reading the header after the data comprehension would fail.

Answer requirements:

- Use csv.reader, next, a list comprehension, and with open.
- Assume a valid CSV with a header.
- Do not convert the raw cell strings.

<details><summary>Hints</summary>

Read in the order header, then remaining data.

The tuple has two differently structured components.

</details>

<details><summary>Worked solution and rubric</summary>

import csv

def read_csv_file(filename: str) -> tuple[list[str], list[list[str]]]:
    """Return the header and remaining raw rows.

    Preconditions:
        - filename refers to a valid CSV file with headers
    """
    with open(filename) as file:
        reader = csv.reader(file)
        header = next(reader)
        data = [row for row in reader]
        return (header, data)

Use 'lectures/week05/marriage_data.csv'. It is relative to the demonstrated project-root working directory. Collecting all rows first consumes the header along with the data; next then raises StopIteration rather than returning the header.

- 2 marks: correct tuple annotation and header precondition.
- 2 marks: with open and csv.reader on its file object.
- 2 marks: header read before remaining rows.
- 1 mark: raw-row list comprehension.
- 1 mark: tuple return inside the with block.
- 1 mark: correct relative path and starting-directory explanation.
- 1 mark: correct exhaustion explanation for reversed order.

</details>

## Challenge quiz

### When removing duplicates changes a computation

12 practice marks

A student claims: 'The lecture allowed a set instead of a list for min, so I can also use a set of monthly counts when averaging.' Evaluate the claim using a hypothetical centre whose three recorded counts are 10, 10, and 40. Give both means, explain the lost information, and explain why the analogous min replacement is valid. Then explain how fixed-centre and observed-centre dictionary keys differ when TO is absent.

Answer requirements:

- Treat repeated counts as distinct monthly observations.
- Keep the discussion within the lecture's min, average, and key-selection operations.

<details><summary>Hints</summary>

The average weights every observation equally, not every distinct value.

A missing key differs from an existing key with value 0.0.

</details>

<details><summary>Worked solution and rubric</summary>

The claim is false. The list mean is (10 + 10 + 40) / 3 = 20.0. The set is {10, 40}, so its mean is (10 + 40) / 2 = 25.0. The set removes the fact that 10 was observed twice, changing both the sum and the observation count. For min, both collections have minimum 10: duplicating or removing extra copies does not change the smallest available value. With TO absent, {row[1] for row in data} excludes TO, so an observed-key comprehension omits that key. The fixed four-centre wrapper retains 'TO' and calls the helper, which returns its specified fallback 0.0.

- 2 marks: list calculation 20.0.
- 2 marks: set calculation 25.0.
- 2 marks: explains lost multiplicity and observation weighting.
- 2 marks: explains why minimum is unchanged.
- 2 marks: observed-key approach omits TO.
- 2 marks: fixed-key approach includes TO: 0.0 via the helper.

</details>

### Combining existence with a universal condition

12 practice marks

For a new specification, return True exactly when the queried centre has at least one recorded row AND every one of its recorded rows has at least num licences. Write a Boolean expression using the taught comprehensions, any, and all. Evaluate it for no matching rows, matching counts [100, 100] with num = 100, and matching counts [99, 101] with num = 100. Explain why the lecture's threshold function alone is insufficient for this new specification.

Answer requirements:

- This is a new self-test specification, not a correction to the lecture's recorded-month specification.
- Assume correctly formatted rows, allowed civic_centre, and num > 0.

<details><summary>Hints</summary>

A universal condition does not assert that anything was selected.

Use and to require both independently meaningful checks.

</details>

<details><summary>Worked solution and rubric</summary>

any([row[1] == civic_centre for row in data]) and all({row[2] >= num for row in data if row[1] == civic_centre})

The first operand establishes existence; the second checks the threshold for every matching row. No matching rows gives False and True, hence False. Counts [100, 100] at 100 give True because the centre exists and equality passes. Counts [99, 101] at 100 give False because 99 is a counterexample to the universal threshold. The lecture's all-only function returns True for an empty selection, so it cannot by itself meet a specification that also demands existence.

- 3 marks: correct existence check.
- 3 marks: correctly filtered universal >= check.
- 1 mark: combines checks with and.
- 1 mark: absent-centre False.
- 1 mark: equality case True.
- 1 mark: mixed case False with 99 identified.
- 2 marks: explains all-only vacuity and why the new existence requirement changes the specification.

</details>

### Combining reading, conversion, and analysis

12 practice marks

Assume the completed read_csv_file, process_row, and average_licenses_by_centre from the notes are available. A valid CSV with headers contains the original eight observations. Write code that reads it, converts every data row, and computes the centre averages. Explain three mistakes: iterating over the entire returned tuple as if it were the table; converting the header with process_row; and passing raw string counts to the averaging helper. Give the expected TO mean. This integration is a new application, not a claim that the lecture processed the whole file in one demonstrated call.

Answer requirements:

- Use the lecture's path relative to the project-root working directory.
- Use a list comprehension to apply process_row to the data rows.
- Assume valid input and do not add unrelated file APIs.

<details><summary>Hints</summary>

The raw rows are component 1 of the returned tuple.

Separate file structure from cell-type interpretation.

</details>

<details><summary>Worked solution and rubric</summary>

result = read_csv_file('lectures/week05/marriage_data.csv')
header = result[0]
raw_rows = result[1]
processed_rows = [process_row(row) for row in raw_rows]
averages = average_licenses_by_centre(processed_rows)

The tuple contains two components—a header list and a whole row-list—not individual observations. Iterating over that tuple confuses those structural levels. Passing the header to process_row attempts int('ID'), which is not a numeric identifier. Raw counts such as '367' are strings, whereas the averaging helper's sum needs numerical counts; converting them enables the intended arithmetic. With the original Toronto observations restored from this CSV, its mean is (367 + 383) / 2 = 375.0, not the later missing-centre fallback.

- 2 marks: correct read call and path.
- 2 marks: selects result[1] as the raw table and separates the header.
- 2 marks: converts every row with a list comprehension.
- 1 mark: calls the average wrapper on processed rows.
- 1 mark: explains tuple/table structural error.
- 1 mark: explains header integer-conversion failure.
- 1 mark: explains raw-count type problem.
- 2 marks: TO mean 375.0 and distinction from absent-TO data.

</details>

### Applying the accumulator pattern to strings

12 practice marks

Complete the displayed review task as a self-test: implement long_greeting(names: list[str]) -> str using a for loop. Each name contributes exactly 'Hello <name>! ', including the trailing space. Trace the accumulator for ['Bahar', 'Paul'], give the empty-list result, and explain why initializing inside the loop or returning inside the loop would break the intended accumulation.

Answer requirements:

- Use a for loop and string accumulator, not join or a comprehension.
- Include a docstring.
- Preserve the final trailing space.

<details><summary>Hints</summary>

The starting value should be the empty-input result.

The updated string must retain earlier greetings.

</details>

<details><summary>Worked solution and rubric</summary>

def long_greeting(names: list[str]) -> str:
    """Return one greeting per name, each ending with a space."""
    greeting_so_far = ''
    for name in names:
        greeting_so_far = greeting_so_far + 'Hello ' + name + '! '
    return greeting_so_far

Initially the accumulator is ''. After Bahar it is 'Hello Bahar! '. After Paul it is 'Hello Bahar! Hello Paul! ', which is returned. On [], no iteration runs and the function returns ''. Resetting the accumulator inside every iteration would discard earlier greetings. Returning inside the body would exit after the first iteration on a nonempty input, so later names would never contribute.

- 1 mark: correct signature and docstring.
- 2 marks: empty-string accumulator initialized before the loop.
- 1 mark: loops over names.
- 2 marks: appends correctly formatted greetings while preserving prior text.
- 1 mark: return after the loop.
- 2 marks: correct two-step trace including spaces.
- 1 mark: empty input returns ''.
- 1 mark: explains resetting inside the loop.
- 1 mark: explains premature return.

</details>
