# CSC110: Programming and proofs — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Revision checklist and common traps

- Read quantifiers in order: an existential witness can depend only on already introduced variables. Prove a negation to disprove a statement.
- Expand Even(t) to an integer witness t = 2k. Do not choose a universally quantified variable to suit the goal.
- A universal statement supplies properties of a given element, not evidence that an element exists. Empty domains make universal claims vacuously true.
- Check the actual hypothesis and conclusion: the converse is not equivalent to an implication; the contrapositive is.
- A true theorem may have a bad proposed proof. Square-root manipulation must retain signs and match the definition being established.
- To justify code, connect every branch to the specification, including zero and domain restrictions. Tests alone do not establish all-input correctness.
- Primality requires p > 1 and no divisor in the inclusive interval [2, √p]. Guard the square root and include the range endpoint.
- In a function-property proof, assume the behavior predicate before using its exhaustive branch cases.

## Instructor guidance for proof writing and preparation

- Introduce variables in quantifier order. Choose witnesses only from available information and establish that each chosen value belongs to its required domain.
- Rough work can proceed backward from the goal; write the actual proof as a justified forward argument. Witness names such as p or k are both acceptable.
- For the parity exercise, begin the disproof by writing the negation. For the AbsDiff nonnegativity proof, consider both cases in its definition.
- The familiar fact that an integer is exactly one of even or odd was allowed in the parity proof. The instructor said assessments would aim to clarify what facts may be used; this is not a blanket permission to assume any desired result.
- Try the prime-characterization biconditional proof before comparing with Section 4.7. Read the referenced divisibility proofs for examples of valid structure and technique.
- Waterproof is optional and will not be tested. Paper proofs remain the assessment medium for proofs; include crucial reasoning even if Waterproof automates it.
- Test-preparation clarification: know the basics of the `math` library, but do not memorize its functions; the instructor said students would not need to use those functions on the upcoming test. Know the small amount of PythonTA use previously covered, not how to write its invocation statements. Know how to write doctests and pytest tests, but do not memorize the code that runs all doctests or pytest tests. Paths and directories were described as technical setup rather than testable material. A transcript item rendered as “Daytime,” apparently referring to `datetime`, was described as tutorial-only rather than official material; consult the posted test scope if that distinction matters.

## Course guide: dates, preparation, and optional support

- **Assignment 1:** due Friday, October 2, 2026, at **1 p.m.**, not midnight. This announced deadline has passed as of October 3.
- **Test 1:** Monday, October 5, covering Chapters 1–3. Check Quercus for your assigned room, final reference sheet, and cover page. The cover page contains question counts and marks, but those details are not supplied here; the self-tests below are not a reconstruction of the test.
- **Extra TA office hours:** consult the Quercus calendar and office-hours page. The instructor was unsure which weekend day hosted the extra sessions; use the posted schedule rather than guessing.
- **Homework:** complete the IsPrime if-and-only-if proof and compare with course notes Section 4.7 after attempting it. Try disproving the strict-endpoint variant. Week 5 preparation was scheduled for release on Quercus on the evening of October 1.
- **Further practice:** work on the remaining AbsDiff exercises. The instructor also suggested trying the divisibility bound with its necessary conditions, deriving parity classification from quotient-remainder, and applying the prime definition to 3 and 4.
- **Coffee & Co-Working:** Mondays, 2–4 p.m., BA 3200, with Milena, the CS learning strategist. The CS undergraduate Quercus site also has appointment-booking information for study-skills and time-management support.
- **Optional Waterproof:** use the Quercus page “Optional: Waterproof for Learning Proofs.” Follow the full installation instructions; installing the VS Code extension alone is not enough. An online option is also listed. Download the CSC110 tutorial, open it in VS Code, and try the exercises alongside paper proofs. Chapter 4 exercises were planned for the end of Week 4. Do not treat every topic in the tool's generic tutorial as current course coverage.
