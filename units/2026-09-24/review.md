# CSC110: Conditionals, Boolean simplification, and PythonTA — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Revision checklist and traps

- Explain why `∀a ∃b` permits different witnesses but `∃b ∀a` requires one shared witness; use the changed Ella–Stanley entry as a counterexample to equivalence.
- Trace the condition and the selected branch. `else:` is not an extra computation; `return` exits the entire function.
- Preserve boundaries: flights four hours late are cancelled; porridge at 49 or 50 is just right; the worksheet teenager range is 13–18 inclusive.
- When using a final else, explain why the remaining possibilities exhaust the allowed input domain.
- Simplify Boolean-returning branches by identifying the exact condition for true. Use `min`/`max` only if they return the kind of result the contract requests.
- Put dictionary membership checks before lookups in an `and` chain; short-circuiting is what makes this safe.
- Know the difference between running a module, importing it, and sending selected text to a terminal. Run the whole assignment file for PythonTA.

## Instructor guidance and exercise requirements

- Choose doctest examples that illustrate different cases of a function’s behavior, rather than repeating one branch.
- Worksheet 1’s implementation tasks request an `if` statement. Worksheet 2 requests `elif` branches; the instructor recommends this flatter layout over unnecessary nesting for the demonstrated three-way choices.
- For worksheet body-line traces, list the condition and executed body statements, not the `else:` label. The instructor also accepted treating the header as a call-entry step when discussing it separately.
- Prefer removing genuinely redundant conditionals; for oddness, the instructor preferred `n % 2 != 0` over negating the equality because it is clearer.
- Run and check the worksheet implementations yourself; the name-formatting and larger-sum code was not run during its explanation.
- Use the supplied PythonTA configuration, run the file rather than Shift+Enter, and fix all reported issues before submission. Assignment 1 Part 1 has no PythonTA requirement; Parts 2 and 3 do.
- The shown Assignment 1 Part 2 instructions require completing function bodies; additional doctests are optional. Its displayed configuration permits `a1_helpers`, sets a 120-character line limit, and disallows `For`, `While`, and `Slice` syntax. Do not generalize these settings to unrelated work.
- Avoid experimenting with special module names such as `__main__` in this workflow; the live exploration was not a definitive account of special-name imports.

## Course actions and Test 1 information

### Work to complete
- Review Assignment 1 now, estimate the time needed, and schedule it. The lecture says it is due “next Friday”: **October 2, 2026**, inferred from the September 24 lecture date. No submission time is supplied here; check the assignment handout.
- Read course-note sections **3.4, 3.5, and 3.6** for this lecture.
- Week 4/Chapter 4 prep was to become available after **5 p.m. September 24**; complete it before **Monday, September 28’s lecture**.
- Optional extra practice: redo the flight examples using `datetime.time` from Section 2.5. No implementation was taught here. The instructor said prior knowledge of that type would not be assumed for Test 1; needed information would be supplied if it appeared.
- Before submitting Assignment 1 Parts 2 and 3, enable and run the supplied PythonTA checks and fix all reported issues. Part 1 intentionally has no PythonTA check.

### Test 1: October 5
The displayed current information page specifies **Monday, October 5, 11:15 a.m.–12:55 p.m. (100 minutes)**.
- Family names **A–S: EX 100**; **T–Z: MY 150**.
- Arrive about ten minutes early, not much earlier. Do not enter until an instructor or TA admits you.
- Bring your T-card and place it on your desk. The page says another photo ID can be reviewed if you forget it.
- Closed book: no personal aids; no calculators are allowed or required. A standard reference sheet will be provided.
- You may leave early before the final ten minutes; no one may leave during those last ten minutes.

### Coverage and preparation
- Chapters **1–3**, through September 24, including relevant course-note material not covered as deeply in lecture. **Chapter 4 and later material is neither covered nor allowed for this test.**
- Excluded sections: **1.8, Representing Colour; 1.9, Representations of Natural Numbers; 2.9, Representing Text**. Qualification: an Assignment 1-related question may still involve colours because the assignment does.
- One question will cover topics similar or related to Assignment 1. Review and understand your work; the page says memorizing the assignment is not required.
- The displayed page describes a **mixture of programming and written questions**, similar in style and content to lecture worksheets and checkpoint quizzes. Exact question counts and marks were not yet announced; review the cover page when posted.
- At the lecture, last year’s reference sheet was posted, with this year’s version to follow. Review the sheet and use it while studying. The page says specific testing setup statements, such as pytest imports and doctest options, need not be memorized, but you must know how to write the actual unit tests. The instructor also said needed math-library function information would be on the reference sheet, while basic understanding remains expected.
- Attempt past tests **before** reading solutions. Do not assume the new test will match them in questions, topic distribution, or timing per question. The 2023 and 2024 tests also covered Chapter 4; the 2025 test included Section 1.9. Filter practice by this year’s coverage.
- Read the posted Studying for Tests page and check the current Test 1 page for the final reference sheet and cover page.
