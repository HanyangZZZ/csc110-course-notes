# CSC110: Comprehensions and function calls — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Revision checklist

- Read a comprehension as “compute this expression for each input.” Choose output type → identity comprehension → transformed expression.
- Input and output types need not match. Lists retain traversal order and repeated results; sets remove duplicates. A list comprehension over a set does not sort it.
- Keep `** 3` (cube) separate from `* 3` (triple). Parenthesize the whole denominator in `x / (x + 1)`.
- Design dictionary keys and values separately, using the actual input values. Dictionary inputs supply keys; use a key lookup to retrieve a value.
- In `f: A → B`, A is the domain and B is the codomain. In Python, distinguish a function name, its call, its arguments, and its return value.
- A call evaluates to a value; an assignment containing a call is still a statement. Evaluate inner calls and variable values before surrounding operators.
- `sorted` always returns a list. Adding two separately sorted lists concatenates them; it does not globally sort. Sets do not support `+`.
- `range` includes its start and excludes its stop. For increasing integer endpoints its size is stop − start; reversed endpoints give an empty range. The professor called stopping one early a common off-by-one error.
- Multiple `for` clauses generate all input combinations. With ordered inputs the rightmost variable changes fastest; repeated output values can collapse in a set.

## Professor requirements and recommendations

- **Mathematical notation:** the professor takes natural numbers to include `0`; in `f: A → B`, use **domain** for A and **codomain** for B.
- **Memory tables:** write evaluated values, such as `[1, 10, -5]`, not variable names such as `[1, 10, n]`. Preserve list order; set display order does not matter.
- **Follow the requested type:** if a prompt asks for a set, use a set comprehension. For the reciprocal question that specified no type, the professor explicitly accepted either a list or a set.
- **Test resources and permitted material:** tests will provide a reference sheet of covered functions and constructs. Typically, only covered material is permitted, and the instructor said it is sufficient for the questions.
- **Working method:** use the comprehension design recipe. Evaluate worksheet expressions by hand first, then check in Python; consult Appendix A.1 for built-in functions.

## Course actions and checkpoint preparation

### First checkpoint quiz
- Announced for **Wednesday, September 16**, computer-based, with **50 minutes rather than 25** to allow for technical adjustment.
- The announced scope is **Chapter One**: variables, basic data types, and comprehensions. The professor said the **functions topic is excluded**; use Quercus → checkpoint quizzes for the stated chapter scope.
- Attend your **registered tutorial lab and time**; another slot will not give you access to your quiz. Use your TCard for lab access.
- The announced arrangement starts ten minutes past the hour in the tutorial's second hour. Consult the Quercus schedule for exact timing and paper/computer weeks; the professor said timing may change later.

### Preparation and homework
- Weekly preparation quizzes are **ungraded but expected** throughout the semester: lectures assume you have completed the preparation.
- MarkUs preparation scores do **not** contribute to the final grade. You can keep trying and running the automated tests after the deadline.
- Complete **Exercise 3: Comprehensions and range** for homework. Try the final three-variable ordering example in Python.

### Getting help
- Come to course office hours early if you are falling behind; no previous programming experience is assumed. The pace is six lecture hours per week.
- CS undergraduate advising is in **BA4207**. CS Undergrads Quercus contains program/course FAQs, advising and career resources, and the weekly community newsletter; use it for current contact details and hours.
- **Learning Strategy Support**, under CS Undergrads Quercus → Academic & Learning Support, offers individual appointments for studying, test preparation, focus, and procrastination, plus coffee/co-working sessions. Seek help early in the semester.
- The **Undergraduate Peer Tutor Program** offers free drop-in help with **unmarked work**, including course-note problems and past-test practice. Cynthia's announced CSC110/111 sessions were Monday, Thursday, and Friday; contact [contact details omitted] for the schedule.
