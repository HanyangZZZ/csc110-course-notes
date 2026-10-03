# CSC110: Introduction to proofs — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Quick revision checklist

- Expand definitions with the correct domains and independent witnesses; d | n is a Boolean predicate, not division.
- Let the outermost operator determine the next proof step. A proof template is not evidence that the claim is true.
- Universal goals use arbitrary elements; existential goals need explicit witnesses with checked domains. Witness choices may depend only on variables already available.
- Distinguish proving from using: one branch can prove an “or,” but using an “or” by cases requires reaching the target in every case.
- Extract an unknown witness from an existential assumption; do not choose its value. Choose and verify a new witness for an existential conclusion.
- To infer Q from P ⇒ Q, establish P. To use a universal fact, supply an element of its domain; the fact alone does not establish existence.
- In a finished direct proof, reason from known facts to the target. Check signs, integer conditions, and possible zero divisors before manipulating equations or inequalities.

## Instructor guidance for writing proofs

- Identify the outermost logical operator before beginning; repeatedly apply the structural rules until the remaining goal needs mathematical reasoning.
- For a universal goal, an opening such as “Let x ∈ S” is acceptable shorthand for taking an arbitrary element. An existential goal instead requires an actual choice of witness.
- Explicitly choose the witness for an existential conclusion, for example “Let k = k₁ + k₂.” The implicit choice in the model even-sum proof was valid but less ideal and harder to mark.
- State that the chosen witness belongs to its required domain. For an obvious integer sum, a lengthy explanation of closure is unnecessary, but the membership statement is recommended.
- Use distinct bound-variable names when they improve clarity. When asked to unfold definitions, retain the surrounding structure rather than moving quantifiers.
- Separate exploratory backward rough work from the finished direct proof, which should proceed from known facts to the desired conclusion.
- Clear line-by-line proofs are preferred; paragraphs and conclusion/justification tables are also valid.
- “Want to show” statements are optional in CSC110. They can help track changing goals, but this course does not require that wording.

## Course actions from this lecture

- Continue Assignment 1: due **Friday, October 2, 2026, at 1 p.m.**, not midnight. This deadline has passed as of October 3.
- Prepare for **Test 1 on Monday, October 5, 2026**. The slide lists Chapters 1–3. Check the Quercus test information page for coverage, supplied sample tests, and your assigned room; students are not all in the same room. No specific test-question format is established by this packet.
- Check the Quercus office-hours information page and course calendar for the extra TA hours announced for the assignment and test.
- Reading plan: this lecture covered part of §4.6; the announced next lecture, **Thursday, October 1**, would continue §4.6 and include material from §4.7. The spoken correction was Thursday, not the slide's “tomorrow.”
- Complete Exercise 4, part 2: write the negation of the displayed product-divisibility claim and prove that negation. The worksheet points to §3.2's negation rules for review.
