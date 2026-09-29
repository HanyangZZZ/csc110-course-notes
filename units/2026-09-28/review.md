# CSC110: Function specifications and property-based testing — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Revision checklist and traps

- State correctness conditionally: every **valid** input must produce the specified result. Invalid calls have no promised result under the contract.
- Parameter annotations are preconditions; a return annotation is a postcondition. An outer collection type does not specify its members, and a member type does not specify length or value bounds.
- Distinguish one RGB list (`list[int]`) from a row of RGB lists (`list[list[int]]`).
- Ordinary Python does not enforce annotations/docstring contracts automatically. PythonTA adds checks for actual calls; partial postconditions still do not fully specify the answer.
- Write generated tests that call the function under test. Match `@given` keywords to parameter names and strategies to the stated domain.
- In `assert is_even(2*x)`, a bug at argument 4 requires generated `x=2`.
- A passing sample is not a proof. One valid counterexample refutes a universal claim.
- For an implication, use `not p or q` or conditionally assert `q` when `p` holds. Preserve positivity restrictions; both negative and zero counterexamples defeated the unrestricted divisor bound.

## Instructor requirements and recommendations

- Write additional preconditions under exactly `Preconditions:`, with each condition on a following dash-prefixed line. Use valid Boolean Python expressions whenever possible.
- Use specific collection annotations for homogeneous data; retain general collection annotations for mixed contents or when contained types are irrelevant to the function.
- Separate `Postconditions:` blocks are not required by default. The ordinary function description still states what the result must mean.
- For the pay exercise, the instructor retained `start < end` but said it could be omitted in a test answer if the supplied description did not state that restriction. Do not treat this optional inference as a universal rule about pay functions.
- Property tests should call the implementation being tested, not substitute a related function or built-in operation.
- Normally separate implementation and test files. When running pytest through a file-path argument, use your own actual test path.
- The instructor permitted ignoring the two warnings seen in the demonstrated pytest/Hypothesis runs; this does not cover arbitrary warnings.

## Course actions and assessment scope

- Continue Assignment 1; it is due Friday.
- Term Test 1 is next Monday. Check the Term Test 1 page for your assigned room; students are split between the lecture room and the exam centre.
- Chapter 4 is **not on Term Test 1**. This lecture covered sections 4.1–4.4; proofs were scheduled for the following lectures.
- Check the Quercus office-hours page or calendar for additional TA office hours.
- No current test format, timing, marks, or permitted aids were verified in the supplied assessment context. The questions in this package are topic-matched self-tests, not predictions of that assessment.
