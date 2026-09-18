# CSC110: Scope, debugging and testing — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Key points and common traps

- Parameters are local variables. Identical names in separate call tables can refer to different values.
- Evaluate the complete right-hand side before assigning its result. At the first Marker C, neither `distance` nor `dx_squared` exists yet.
- A debugger highlight marks the next statement, before it executes. Step Over still respects active breakpoints.
- A bare expression can display a result in the console while displaying nothing in a file. Use `print` when output is intended.
- Silent doctest success is normal; failed assertions become test failures. Passing tests usually provide confidence, not proof.
- Compare sets/dictionaries by equality in doctests, and computed floats with a suitable tolerance.
- Module names use dots; file-path strings use slashes. Check the execution method and working directory when imports or test paths fail.

## Conventions used in this lecture

- Draw separate value-based memory tables for `__main__` and active function calls; mark the current frame and retire returned frames. Row order is immaterial.
- Use arguments and return values to move information between functions; avoid relying on globals in this course.
- Write doctests in console format, including `>>>` and correctly aligned expected output.
- Name test files and test functions with the `test_` prefix. Give each test a purpose-revealing name/docstring and `-> None`; the examples take no parameters.
- Keep runnable test commands in the bottom `if __name__ == '__main__':` block. Put imports needed by the test functions, such as `pytest` for `approx`, at the top.
- Prefer `assert actual == pytest.approx(expected)` for computed floats; write the expected value on the right for readability.
- The September 17 distance worksheet uses `round(..., 2)`.

## Reading and September 17 announcements

Complete preparation before lectures, then finish the week's assigned course-note chapters. **Finish Chapter 2, including [§2.6: Type Conversion Functions](https://www.teach.cs.toronto.edu/~csc110y/fall/notes/02-functions/06-type-conversions.html)**, which was not covered in this lecture. Complete the `rank_absolute_values` testing homework; practise memory diagrams and debugger inspection.

**September 17 announcements:** second and third checkpoint attempts carry no penalty; use the feedback to study. Week 3 Quercus and MarkUs preparation is for practice. Assignment 1 covers Chapters 1–3; check course announcements for its release and the syllabus for its deadline.

If Shift+Enter is unreliable, run the whole file and account for the display/import differences explained in the notes.
