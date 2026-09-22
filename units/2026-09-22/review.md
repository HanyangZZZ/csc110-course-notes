# CSC110: Quantifiers, filtering, and nested statements — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Revision checklist

- Identify the domain before interpreting a quantifier: ℕ starts at 0; ℤ⁺ starts at 1.
- Expand ∀ as AND and ∃ as OR. One counterexample refutes ∀; one witness establishes ∃.
- Check each variable occurrence's scope. A sentence can still be false.
- Filter a universal with implication and an existential with AND; explain what excluded inputs contribute.
- In Python, distinguish building Booleans from filtering data. Select inputs, transform them, then aggregate.
- Remember `all([]) == True` and `any([]) == False`.
- Negation flips ∀/∃ and negates the body, without changing the domain.
- For mixed quantifiers, ask whether the witness may vary with an earlier variable or must work for everyone at once.

## Instructor guidance and exercise requirements

- Use the course convention ℕ = {0, 1, 2, …}; do not omit zero.
- The instructor recommended filtering comprehensions when filtering in Python, rather than unnecessarily encoding implication or conjunction.
- Keep an explicit set or list comprehension inside `all`/`any` for this course treatment; the bracket-omitting feature was not being covered. Either collection type is acceptable for the demonstrated Boolean aggregations.
- For the quantifier-expansion exercise, expand using iterated conjunction/disjunction; if a formula is not a sentence, identify that instead.
- For `longest_cool_string`, complete the doctest and implement with a filtering comprehension. Preserve the assumption that at least one string contains `'cool'`.
- The instructor said test examples would use parentheses to clarify ambiguous grouping; this does not establish a quiz question format.
- **Official reading guidance — Commas: avoid them!** Use explicit logical connectives, not commas, to join propositions. Commas belong in quantification syntax and predicate argument lists.
- **Displayed Assignment 1 starter-file guidance:** follow the handout, complete function bodies, and treat additional doctests as optional aids to understanding/testing. The displayed file is for students' personal/private use and prohibits redistribution of the supplied code or modifications.

## Actions and course logistics

- **Room change:** Thursday morning lectures move from BA 1160 to **ES 1050 for the rest of term**, including the next Thursday, September 24. Tuesday lectures remain in the current room.
- **Quiz tomorrow, September 23:** attend on the regular schedule. The instructor stated a 25-minute quiz block and a 25-minute Quiz 1 retake block for those taking the retake. Tomorrow's material is Chapter 2 except its final application section.
- **Format guidance:** consult the Quercus checkpoint-quiz page for the paper/computer schedule. The instructor identified Week 3 as computer-based programming and Quiz 4 as paper-based, covering Chapter 4 on proofs. These announcements do not establish the format of a future assessment on this lecture's logic material.
- **Assignment 1:** read the released handout and begin using the starter files. At lecture time submission was not yet open; opening later that day or the next was an expectation, not a confirmed deadline. No assignment due date was supplied here.
- **Readings:** today's sections are 3.2, 3.3, and 3.7; next class's are 3.4, 3.5, and 3.6.
- **Homework:** complete the previous Monday's predicate-logic translation exercise; solutions were to be posted later. Finish the multiple-quantifier worksheet, including explaining why mixed quantifier orders need not agree and changing the table to alter truth values.
- **Help:** office hours, Ed Discussion, Victoria College tutoring in Bahen (also open to other colleges), and CS learning-strategy appointments are available. Consult course announcements for schedules and booking links.
