# CSC110: Tabular data, dates, and CSV files — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Revision checklist and common traps

- Know the row layout: ID, centre, count, date. A row is one centre/month observation.
- Distinguish extracting a cell, extracting a column, and filtering whole rows. `>` means more than; `>=` means at least.
- Test whole centre codes using membership in a collection, not substring membership in one string.
- Separate `any` (at least one) from `all` (every selected item). An empty selection makes `all` true, not evidence of recorded activity.
- Preserve repeated counts when averaging; guard against an empty filtered list. Observed-centre keys and four fixed keys are different specifications.
- Resolve file paths from the working directory. CSV cells begin as strings; read the header before numeric conversion.
- A reader advances: exhausted `next` raises `StopIteration`, while collecting remaining rows gives `[]`.
- Build dates in year/month/day order. Date subtraction returns a `timedelta`.
- For accumulators: initialize before the loop, update inside it, return after it.

## Instructor guidance and exercise requirements

- Practise reading, understanding, and writing code; beginners should expect to reason through it line by line.
- Predict Exercise 1 expression results, then check them in the Python console. Where the exercise requests list indexing, use indexing; an ID-filtering alternative does not satisfy that particular method instruction.
- Include docstrings in your own functions. The instructor's temporary omission during the helper demonstration was not a model to copy.
- For the Toronto-existence check, the instructor preferred the direct `any([row[1] == 'TO' for row in marriage_data])` version for clarity, while accepting the other demonstrated versions.
- Run the file-reading exercise on your computer to see the behaviour and expose bugs.
- Prefer `date_string.split('/')` over the equivalent `str.split(date_string, '/')` form.
- Name accumulators for the value they track; use an appropriate starting value, usually the empty-input result.
- For the final greeting review exercise, retain the required space after every `!`, including at the end of the returned string.

## What to do after this lecture

- **Wednesday, October 7:** the Chapter 4 checkpoint quiz is on paper; bring something to write with. The second attempt for Chapter 3 remains on the computer.
- Chapter 4 Waterproof proof exercises are available on the Waterproof page on Coursera for additional practice.
- Review **5.1, 5.4, and today's slides**. CSV/file-reading content in the slides is expected knowledge even though there is no separate course-notes reading for it.
- For **Thursday, October 8**, read **5.2–5.5** and review Exercise 5 on identifying loop components, tracing accumulation, and implementing the greeting function. Loops will continue next lecture.
