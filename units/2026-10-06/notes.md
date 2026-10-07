# CSC110: Tabular data, dates, and CSV files

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Learning programming: recognition develops through practice

The lecture introduced a theory of two ways of thinking. **System 1** is fast recognition based on patterns in long-term memory. **System 2** is slower, deliberate reasoning that uses working memory—the information you actively hold while solving a problem.

Experienced programmers have encountered common code patterns repeatedly. They can group related ideas into **chunks**, so recognizing a familiar structure requires less working-memory effort than reasoning through every line separately. Beginners often need to read code line by line and work out what each part does. That is normal, not evidence that they cannot become proficient. Reading, understanding, writing, and working with code builds the patterns that make later recognition possible.

## Representing a table as a nested list

**Tabular data** is data arranged in rows and columns. Earlier examples included a table of truth values for the `Loves` predicate and rows of image pixels. A **nested list** is a list containing other lists: here the outer list is the table and each inner list is one row.

The running example records marriage licences issued at Toronto civic centres. Each row represents one centre in one monthly time period—not one marriage. The four columns have fixed meanings:

| Index | Meaning | Python representation |
|---|---|---|
| `0` | Unique row identifier | `int` |
| `1` | Civic centre code | `str`: `'ET'`, `'NY'`, `'SC'`, or `'TO'` |
| `2` | Number of licences issued | `int` |
| `3` | Monthly time period | `datetime.date` |

The codes refer to Etobicoke, North York, Scarborough, and Toronto. The sample contains January and February 2011 observations:

```python
import datetime

marriage_data = [
    [1657, 'ET', 80, datetime.date(2011, 1, 1)],
    [1658, 'NY', 136, datetime.date(2011, 1, 1)],
    [1659, 'SC', 159, datetime.date(2011, 1, 1)],
    [1660, 'TO', 367, datetime.date(2011, 1, 1)],
    [1661, 'ET', 109, datetime.date(2011, 2, 1)],
    [1662, 'NY', 150, datetime.date(2011, 2, 1)],
    [1663, 'SC', 154, datetime.date(2011, 2, 1)],
    [1664, 'TO', 383, datetime.date(2011, 2, 1)]
]
```

**Reading clarification — 5.1 Tabular Data > Toronto getting married:** the time period is a year-month combination. The first day is used to represent that month; it does not mean every licence was issued on that day.

A Python list is indexed starting at zero. Thus `marriage_data[1]` retrieves the second row, `[1658, 'NY', 136, datetime.date(2011, 1, 1)]`. A second indexing operation selects a cell within that row: `marriage_data[1][0]` is `1658`, and `marriage_data[1][3]` is `datetime.date(2011, 1, 1)`. The row's ID is data stored in a cell, not its list index.

The worksheet's expression-evaluation examples, using the original eight rows, are:

| Expression | Value | Why |
|---|---|---|
| `len(marriage_data)` | `8` | Counts rows in the outer list. |
| `marriage_data[5]` | `[1662, 'NY', 150, datetime.date(2011, 2, 1)]` | Selects the sixth row. |
| `marriage_data[0][0]` | `1657` | First cell of the first row. |
| `marriage_data[6][2]` | `154` | Licence count in the seventh row. |
| `len(marriage_data[3])` | `4` | Counts cells in the fourth row. |
| `max([row[2] for row in marriage_data])` | `383` | Extracts all counts, then selects the greatest. |

These values are derived from the displayed sample. The instructor asked students to compare their predicted evaluations with the Python console rather than taking up every expression verbally.

## Dates: construction, comparison, and differences

`datetime` is a Python **module**, a collection of related functionality that must be imported. `datetime.date` is a data type supplied by that module; a value of this type represents a calendar date.

```python
import datetime

today = datetime.date(2026, 10, 6)
monday = datetime.date(2026, 10, 5)
```

The constructor takes three integers in **year, month, day** order. A date can be assigned to a variable like any other value. `datetime.date.today()` asks for the current date; during the lecture it returned `datetime.date(2026, 10, 6)`, but its result depends on when it is run.

```python
datetime.date.today() > monday
# True during this lecture

datetime.date.today() - monday
# datetime.timedelta(days=1) during this lecture
```

Comparison uses chronological order, so October 6 is later than October 5. Subtracting two dates gives a `datetime.timedelta`: a value representing an interval of time. It is not the integer `1`, even though this particular interval is one day. The lecture required only this limited use of the date library, not detailed knowledge of all its features.

## Extracting columns, filtering rows, and computing extrema

A **comprehension** builds a collection by evaluating an expression for each element of another collection. In `[row[2] for row in marriage_data]`, `row` takes on each inner list in turn, and `row[2]` contributes that row's licence count. The result is a column of counts:

```python
[80, 136, 159, 367, 109, 150, 154, 383]
```

A **filtering comprehension** adds an `if` condition. Only rows that pass the condition contribute to the result. What goes before `for` determines what is retained:

```python
[row for row in marriage_data if row[1] == 'TO']
```

This keeps whole rows, not just centre names. On the original data it returns:

```python
[[1660, 'TO', 367, datetime.date(2011, 1, 1)],
 [1664, 'TO', 383, datetime.date(2011, 2, 1)]]
```

The exercise asked for these additional expressions:

- **Etobicoke's February count:** `marriage_data[4][2]` gives `109`. Inspection locates the fifth row, then the third cell. This answer depends on the displayed row ordering.
- **Date for ID 1662:** `marriage_data[5][3]` gives `datetime.date(2011, 2, 1)`. The exercise specifically requested list indexing to access the correct row.
- **Alternative ID search:** `[row for row in marriage_data if row[0] == 1662]` returns `[[1662, 'NY', 150, datetime.date(2011, 2, 1)]]`. This finds matching rows without knowing their positions, but returns a list of rows rather than directly returning a date. Treating the answer as one uniquely identified row relies on unique IDs. Use `==` for comparison, not `=`; the instructor also corrected the initially entered ID `1663` to the requested `1662`.
- **Minimum licence count in a recorded centre/month:** `min([row[2] for row in marriage_data])` gives `80`. It compares individual row counts; it does not first total all centres for each month.
- **Rows with more than 300 licences:** `[row for row in marriage_data if row[2] > 300]` returns the same two Toronto rows above. The final condition is `> 300`, not `>= 300`: a count of exactly 300 would not qualify.

For `min`, using `{row[2] for row in marriage_data}` instead of a list would give the same minimum: removing duplicate copies of a value cannot change which value is smallest. This does **not** mean lists and sets are interchangeable for every numerical computation. A **set** stores distinct values; a list preserves repeated observations. The averaging example needs those repetitions.

## Expressing data constraints with all and any

A nested list alone does not guarantee a usable table. A **data constraint** specifies a property that the representation must satisfy, such as the number of cells per row or the type of a cell. A **precondition** is a requirement that callers are expected to satisfy before using a function.

`all` checks whether every supplied Boolean value is true. A comprehension can produce one Boolean check per row, allowing an English requirement about every row to become a Python expression:

```python
# Every row has four cells.
all({len(row) == 4 for row in marriage_data})

# Every row ID is an integer greater than zero.
all({isinstance(row[0], int) and row[0] > 0
     for row in marriage_data})

# Every centre is one of the permitted codes.
all({row[1] in {'TO', 'ET', 'NY', 'SC'}
     for row in marriage_data})

# Every fourth cell is a date.
all({isinstance(row[3], datetime.date)
     for row in marriage_data})
```

`isinstance(value, type)` tests whether a value is an instance of the given type. The ID check combines two separate requirements with `and`: merely being positive does not establish the required type. These are translations of separate constraints; the indexed checks assume the relevant cells exist, so the shape requirement matters.

**Membership trap:** `row[1] in 'TO ET NY SC'` does not mean that the cell equals one of four codes. Membership in a string tests for a **substring**, a consecutive piece of that string. For example, `'TO ET' in 'TO ET NY SC'` is `True`, even though `'TO ET'` is not a valid single code; the shorter `'O '` also passes. Put the codes in a set or list to test membership among whole elements instead.

The final exercise requirement is different: **at least one row has Toronto as its centre**. This is not an inherent requirement of every possible marriage dataset. `any` asks whether at least one supplied Boolean is true:

```python
any([row[1] == 'TO' for row in marriage_data])
```

Each row contributes `True` or `False`; one Toronto row is enough. Lists or sets of these Booleans work equally well with `any` and `all`, because repeating a Boolean does not change whether a true or false value exists.

Three other valid existence checks were discussed:

```python
len([row for row in marriage_data if row[1] == 'TO']) > 0

[row for row in marriage_data if row[1] == 'TO'] != []

any([True for row in marriage_data if row[1] == 'TO'])
```

The first counts matches; in the original sample there are two, so the count is positive. The second asks whether the filtered list is nonempty. The third inserts a `True` for each matching row; with no matches, it produces an empty list and `any` returns `False`. The instructor regarded the first, direct Boolean-comprehension version using `any` as the clearest, while accepting all four.

Bind a whole row with `for row in marriage_data` and then access `row[1]`. The attempted `any(['TO' for row[1] in marriage_data])` did not implement this: the demonstrated evaluation failed because `row` was not defined. It also places the constant string `'TO'` in the result rather than expressing the needed equality check.

## A larger computation: average licences by centre

The goal is a **dictionary**, a mapping from keys to values: each centre name is a string key and its average licence count is a float value. Break the work into smaller steps rather than trying to write the entire computation at once:

1. Decide which centre names must appear as keys.
2. For one centre, filter the table to its rows and extract their counts.
3. Compute the average of those counts.
4. Repeat the centre-specific calculation to build the dictionary.

Initially, the class proposed `{row[1] for row in marriage_data}` to collect distinct names. This is appropriate for finding centres **present in the data**. However, the final specification requires **all four fixed centres**, even if a centre has no rows. These are different requirements: extracting observed names would omit a missing centre altogether.

For a centre `cc`, this expression combines filtering with column extraction:

```python
filtered = [row[2] for row in marriage_data if row[1] == cc]
```

For Toronto in the original sample, it produces `[367, 383]`. Their sum is 750 and there are two observations, so the average is `750 / 2`, or `375.0`. The denominator counts matching rows—not every row in the table.

A **helper function** performs one part of a larger computation so that the outer function can reuse it. The completed lecture helper uses this empty-list guard:

```python
def average_licenses_issued(marriage_data: list[list], cc: str) -> float:
    """Return cc's mean licence count, or 0.0 if there are no matching rows."""
    filtered = [row[2] for row in marriage_data if row[1] == cc]
    if len(filtered) == 0:
        return 0.0
    return sum(filtered) / len(filtered)
```

Without the guard, no matching rows would produce `sum([]) / len([])`, hence division by zero—not an automatic result of zero. The early return handles that case. Reaching the final return means `filtered` is nonempty, so its length cannot be zero. Returning `0.0` for missing data is the specified fallback, not a mathematical mean for an empty sample.

The wrapper calls the helper once for each required centre:

```python
def average_licenses_by_centre(marriage_data: list[list]) -> dict[str, float]:
    """Return a mean for each of TO, ET, NY, and SC.

    Preconditions:
        - marriage_data satisfies all of the conditions from Exercise 2
    """
    civic_centres = {'TO', 'ET', 'NY', 'SC'}
    return {cc: average_licenses_issued(marriage_data, cc)
            for cc in civic_centres}
```

The **dictionary comprehension** creates one `key: value` pair for each `cc`. The lecture retained the Exercise 2 precondition in the wrapper; that includes the allowed-centre constraint. Its literal inclusion of every Exercise 2 condition also requires a Toronto row. Nevertheless, the instructor explicitly discussed and demonstrated the implementation's missing-centre behaviour. Distinguish the written precondition from what the body can handle: the demonstration without Toronto rows is outside that literal Toronto-existence requirement.

On the original eight rows, the calculated mapping is `{'TO': 375.0, 'ET': 94.5, 'NY': 143.0, 'SC': 156.5}`. In the later live demonstration, the instructor had **removed both Toronto rows**, so the displayed mapping instead had `'TO': 0.0`, with the other means unchanged. These are different inputs, not conflicting calculations. Dictionary display order here is not part of the required result.

**Reading clarification — 5.1 Tabular Data > A worked example:** the count collection is a list so repeated monthly counts remain repeated observations. A set would discard repetitions and could change the mean. The reading's helper checks `issued_by_civic_centre == []`; the lecture's completed version above checks `len(filtered) == 0`. Both detect an empty list, but the code above preserves the lecture version.

The instructor noted that a for loop or a single larger function could also implement this task. A comprehension works here because it repeats an expression—the helper call—for each centre.

## Functions for distinct centres and monthly thresholds

The first Exercise 3 function returns only centres actually found in its argument. A set comprehension removes repeated names:

```python
def civic_centres(data: list[list]) -> set[str]:
    """Return the set of civic centres found in data."""
    return {row[1] for row in data}
```

Use the parameter `data` so the function works on the table passed to it. On the original sample the result contains `'TO'`, `'ET'`, `'NY'`, and `'SC'`; if all Toronto rows are removed, the result excludes `'TO'`. That is correct for this function, unlike the fixed-key average wrapper.

The second function asks whether one given centre issued **at least** `num` licences in **every recorded month for that centre**. Missing months are explicitly outside its scope:

```python
def civic_centre_meets_threshold(data: list[list],
                                 civic_centre: str,
                                 num: int) -> bool:
    """Return whether every recorded row for civic_centre meets num.

    Ignore missing months.

    Preconditions:
        - num > 0
        - data satisfies all of the properties described in Exercise 2
        - civic_centre in {'TO', 'NY', 'ET', 'SC'}
    """
    return all({row[2] >= num for row in data
                if row[1] == civic_centre})
```

The filter selects the relevant centre first. The expression `row[2] >= num` then produces a Boolean for each selected observation. **At least** includes equality: just as a 16-year-old is at least 16, a count equal to `num` passes. Finally, `all` requires every selected check to be true. The completed lecture code uses a set of Booleans; a list would give the same truth result.

If the centre has no recorded rows, the comprehension is empty and `all` returns `True`. This is **vacuous truth**: a statement that every selected row passes has no failing row when there are no selected rows. It does not establish that the centre actually issued licences or that every calendar month is represented.

The lecture demonstrated `civic_centre_meets_threshold(marriage_data, 'TO', 1)` returning `True` after Toronto's rows had been removed. The spoken discussion treats an absent queried centre as allowed. The displayed Exercise 2 precondition reference, taken literally, conflicts specifically with absence of Toronto because Exercise 2 also asks for a Toronto row. The intended empty-selection behaviour and the demonstrated result are still clear; the function's scope is the rows present, not missing months.

## Plain-text files, open, with, and corrected file paths

**File I/O** means file input/output: reading data from files and writing results to files. Until now, inputs were often typed into source code or supplied manually when calling functions. Files provide persistent storage, so data can survive between program runs instead of needing to be re-entered. This lecture implemented reading; it did not demonstrate a file-writing procedure.

Before reading a file, open it:

```python
filename = 'example.txt'
with open(filename) as my_file:
    # Code that reads from my_file goes here.
    pass
```

`filename` is a string describing where the file is. `open` accesses that file and returns a **file object**, a Python value representing the open file. `as my_file` binds that object to the name used inside the block. The indented block is part of the `with` compound statement—a statement containing other statements. Put operations that need the open file inside it. `with` automatically closes the file when the block exits.

**Important correction to the slides:** do not assume that a relative filename is looked up beside the Python module. A **relative path** describes a location starting from the program's current working directory. In the demonstrated VS Code setup, this was the open `csc110` project folder, not `lectures/week05`, where the Python file and CSV were located.

Consequently, this initially failed:

```python
with open('marriage_data.csv') as my_file:
    pass
# FileNotFoundError in the demonstrated setup
```

This path succeeded in that setup:

```python
with open('lectures/week05/marriage_data.csv') as my_file:
    pass
```

It tells Python to descend from the project folder into `lectures`, then `week05`, then open the CSV. This is still a relative path, not an absolute path beginning at a filesystem root. A `FileNotFoundError` can mean the file is absent, or simply that the supplied path does not point to it from the directory used to run the program. The instructor corrected the original same-folder claim after the live failure; use the corrected behaviour when arranging your files.

## CSV readers: raw strings, headers, and consumption

**CSV** stands for **comma-separated values**. It is a plain-text format for tabular data: commas separate cells and, in the simple examples here, lines separate rows. A **header row** labels the columns; it is metadata about the table rather than a marriage observation.

A slide example begins:

```text
ID,Civic Centre,Marriage Licenses Issued,Time Period
1657,ET,80,January 2011
1658,NY,136,January 2011
```

The actual file used slash-separated dates such as `2011/01/01`, which are easier to convert with the date helper developed later.

**Parsing** means turning text into a useful structured representation. Import `csv`, open the file, then pass its file object to `csv.reader`:

```python
import csv

with open('lectures/week05/marriage_data.csv') as my_file:
    reader = csv.reader(my_file)
    data = [row for row in reader]
```

The reader supplies each CSV row as a list of strings, separating its cells for you. Thinking of it as a sequence of row lists helps explain a comprehension, but it is not a reusable stored table: reading advances it. The comprehension above creates the stored list of rows.

If the three-line slide example is read from the beginning without skipping its header, `data[0]` is the header, `data[1]` is `['1657', 'ET', '80', 'January 2011']`, and `data[2]` is `['1658', 'NY', '136', 'January 2011']`. **All cells are strings**, even text that looks numeric. Thus `int(data[1][0])` converts `'1657'` to the integer `1657`; reading alone does not infer integers or dates.

`next(reader)` retrieves one row and advances the reader. It is useful for separating a header before processing data:

```python
with open('lectures/week05/marriage_data.csv') as my_file:
    reader = csv.reader(my_file)
    header = next(reader)
    data = [row for row in reader]
```

After `next(reader)`, the header has already been consumed, so `data[0]` is the first actual observation: `['1657', 'ET', '80', '2011/01/01']`. **Consumption** means the internal reading position moves forward; it does not mean the file's contents are deleted. A later read starts where the previous read stopped, not at the beginning.

The lecture contrasted these cases:

- Repeated `next(reader)` calls return successive rows.
- A comprehension reads all remaining rows, leaving the reader exhausted.
- Calling `next(reader)` after exhaustion raises `StopIteration`, because it was asked to supply one more row and cannot.
- A new `[row for row in reader]` after exhaustion returns `[]`, because there are zero remaining rows to collect. It does not raise an error merely for having no iterations.
- If the comprehension runs **before** reading the header separately, it includes the header as its first list. A subsequent `next(reader)` still raises `StopIteration`; it does not recover the first row.

Keep headers separate before converting numeric fields: `'ID'` is a column label, not a numeric identifier. The class also asked about commas inside cell text and reading without advancing. No concrete technique for those cases was taught, so this lecture does not supply implementations for them; the instructor's stated course focus was reading CSV data into lists.

## Reading a CSV and converting its rows and dates

Exercise 4 separates three tasks: read raw CSV text into rows; convert one row's cell types; and convert a date string. This separation keeps file access independent from the marriage-specific interpretation of the data.

### 1. Return the header and remaining rows

```python
import csv
import datetime


def read_csv_file(filename: str) -> tuple[list[str], list[list[str]]]:
    """Return the CSV header and the remaining raw string rows.

    Preconditions:
        - filename refers to a valid CSV file with headers
    """
    with open(filename) as file:
        reader = csv.reader(file)
        header = next(reader)
        data = [row for row in reader]
        return (header, data)
```

A **tuple** groups multiple values into one result; here `(header, data)` is a two-element tuple. Its annotation specifies the type of each position: the first contains `list[str]`, and the second contains `list[list[str]]`. The outer result is **not** a list, despite inconsistent wording in the worksheet. Both reading operations and the return are inside the `with` block in the completed lecture implementation. The precondition assumes a header exists; this is not an implementation for a completely empty file.

The filename requirement concerns an external file, rather than just the shape of the string argument; the worksheet states it as an English precondition. In the demonstrated setup, calling with only `'marriage_data.csv'` failed, while `'lectures/week05/marriage_data.csv'` found the file.

### 2. Convert one row

```python
def process_row(row: list[str]) -> list:
    """Return a new marriage-data row with suitable cell types.

    Preconditions:
        - row has the correct format for the marriage licence dataset
    """
    return [int(row[0]), row[1], int(row[2]), str_to_date(row[3])]
```

The ID and count become integers. The centre code is already a useful string, so it is unchanged. The final cell is delegated to a helper that returns a date. The result is a new list; this function processes one row, not an entire dataset.

For example, `['1658', 'NY', '136', '2011/01/01']` becomes `[1658, 'NY', 136, datetime.date(2011, 1, 1)]`. If `data = read_csv_file(...)`, the documented example `process_row(data[1][1])` first chooses the tuple's data component, then its second row. Later, the live demonstration used `process_row(marriage_data[1][3])` with `marriage_data` holding the returned tuple; it converted the fourth raw row to `[1660, 'TO', 367, datetime.date(2011, 1, 1)]`. The extra indexing level comes from the `(header, data)` wrapper, not from an extra column in the table.

### 3. Convert a slash-separated date

```python
def str_to_date(date_string: str) -> datetime.date:
    """Convert a yyyy/mm/dd string to a date.

    Preconditions:
        - date_string has format yyyy/mm/dd and represents a valid date
    """
    components = date_string.split('/')
    return datetime.date(int(components[0]),
                         int(components[1]),
                         int(components[2]))
```

`split('/')` divides a string at each slash and returns a list of the pieces. For `'2014/01/01'`, the components are `['2014', '01', '01']` in **year, month, day** order. Each is still a string; `int` converts each piece before passing it to `datetime.date`. The specified result is `datetime.date(2014, 1, 1)`.

The instructor considered extracting characters by fixed positions but pointed out the problem with a variant such as `'2014/1/1'`: the positions of later pieces change when leading zeroes are absent. Splitting on the separators works for both spellings. That does not make this a general date-language parser: a phrase such as `'January 2011'` is a different format. Constructing a date also requires a valid calendar date; merely obtaining three integers is not enough.

The method-call forms `date_string.split('/')` and `str.split(date_string, '/')` perform the same splitting operation here. The instructor preferred the first form. The worksheet hint's month/day/year wording should not be followed: the supplied input format and final code require year/month/day.

## For loops, control flow, and the accumulator pattern

**Control flow** describes which statements execute and in what order. The recap distinguished:

- **Sequential execution:** statements run one after another. The displayed distance example computes `(x2 - x1) ** 2` and `(y2 - y1) ** 2`, adds them, takes the square root with `** 0.5`, and returns the distance rounded to one decimal place. Later steps use values calculated by earlier steps.
- **Conditional execution:** an `if` chooses which statements run. The displayed `get_status` example returns `'On time'` when `estimated <= scheduled`, otherwise `'Delayed'`.
- **Repeated execution:** a `for` loop runs a block of statements once for each element of a collection.

A comprehension repeats an **expression**, which produces a value. A for loop can repeat **statements**, including assignments and entire `if` statements, so it can express multi-step work that is not just one repeated expression.

```python
for variable in collection:
    statement
    # More statements may follow inside this block.
```

The indented statements form the **loop body**. The **loop variable** receives the first collection element, then the body runs. It next receives the second element and the body runs again, continuing through the collection. Each repetition is a **loop iteration**; the loop **iterates over** the collection. For a list, repeated equal values in different positions still give separate iterations.

### Keeping track of a result

An **accumulator** is a variable holding the result built so far. The **accumulator pattern** has three stages: initialize it before the loop, update it during each iteration, and return it after the loop.

```python
def my_sum(numbers: list[int]) -> int:
    """Return the sum of numbers."""
    # ACCUMULATOR sum_so_far: sum of the elements seen so far.
    sum_so_far = 0
    for number in numbers:
        sum_so_far = sum_so_far + number
    return sum_so_far
```

The update uses both the previous accumulated value and the current element. Starting at zero gives the correct sum before any numbers have been seen, including when the input is empty. The return belongs after the loop so all elements contribute.

The generalized pattern is:

```text
<x>_so_far = <default_value>
for element in <collection>:
    <x>_so_far = ... <x>_so_far ... element ...
return <x>_so_far
```

These angle-bracket names are placeholders, not executable Python. Name the accumulator for what it stores. Its starting value is usually the desired result for an empty collection. Updating it may require more than one statement, and the result need not be numeric.

### Final exercise shown, assigned for review

The class did not complete Exercise 5 during the recording. Its first displayed example was `sum_of_squares([4, -2, 1])`, with expected result `21`: the update is `sum_so_far = sum_so_far + number ** 2`. Students were asked to identify the loop variable and accumulator and fill in a loop accumulation table—a record of their values across iterations.

The second prompt asked for a loop-based greeting function. Each greeting must have the form `Hello <name>! `, including the final space, and the returned string must concatenate all greetings. The displayed example is `long_greeting(['Bahar', 'Paul'])` returning `'Hello Bahar! Hello Paul! '`. These are review tasks, not completed classroom solutions; the practice in the interactive reader develops the same pattern.

Reference: David Liu and Mario Badr, [Foundations of Computer Science: CSC110/CSC111 Course Notes](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/). Linked course materials remain the property of their respective authors.
