# CSC110: Defining functions and methods — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Key points and common traps

These are study priorities inferred from the teaching, not claims that particular questions will appear on a test.

- Trace multiple `for` clauses from left to right; the rightmost variable changes fastest. The expression before `for` controls the output, not traversal order.
- A definition makes a function available; a call runs its body. Parameters are definition names; arguments are supplied values.
- `return` evaluates an expression and immediately leaves the function. A later statement in that body is unreachable.
- Parameter and return annotations describe a contract; ordinary Python does not enforce them. A body can raise an error for an unsupported operation, and an editor can flag a problem before runtime.
- Use the corrected rule: different argument counts do not make ordinary same-name Python definitions coexist as overloads.
- Apply the whole design recipe. Match the requested input interface and rounding requirement; do not jump straight to implementation.
- Docstrings show representative uses; comprehensive tests also need other cases. Skipping live testing for time is not permission to omit it in your work.
- Return a Boolean comparison directly for `is_same_length`; equal lengths do not require equal elements.
- The tax worksheet permits ignoring rounding differences. Rounding each item and rounding only the total are not generally equivalent.
- In `object.method(other_args)`, the receiver supplies the first object argument. Keep `()` and supply any remaining required argument, such as the target for `count`.
- Default `split()` uses whitespace. Set display order is not a result to memorize. `count(value)` and `len(...)` answer different questions.

## Professor conventions and submission habits

- For course functions, include the complete header with parameter/return annotations, an indented docstring, and doctest examples. Their omission in a fast demo is not a waiver.
- Write `def name(parameter: type, ...) -> return_type:`; use consistent indentation. Choose descriptive lowercase names with underscores where useful.
- Begin descriptions with “Return …”; explain the result and parameter roles, including rounding, rather than reciting the implementation.
- Format each doctest as `>>> function(arguments)` followed by the expected result on the next line. One or two useful normal examples suffice for a simple function; test additional cases separately.
- Follow all five recipe steps: examples, header, description, body, tests. Preserve the problem’s required interface.
- Use receiver method calls such as `s.lower()` for the rest of the course.
- For the displayed worksheet, calculate by hand before checking in Python; a different set display order is acceptable.
- To run selected definitions in VS Code for this lesson, use Run Python → Run Selection/Line in Python Terminal or Shift+Enter, then call the function.

## Learning advice and lecture announcements

**Learning and memory.** Unlike reading stored computer data, recalling a human memory can strengthen and modify it (**reconsolidation**). Review strengthens recall, and new learning can refine old understanding. **Spreading activation** connects related memories; this can cause mix-ups but also help a solution occur later while doing something else. Revisit earlier ideas and connect them with new ones.

**September 15 announcement:** the first checkpoint is tomorrow, September 16. Read the Quercus announcement and attend your enrolled tutorial timeslot: the MarkUs quiz appears for the assigned slot.

**Coming later:** Thursday covers more systematic testing and file-running/import details. Detailed type enforcement, combined/Any annotations and side effects were previews.
