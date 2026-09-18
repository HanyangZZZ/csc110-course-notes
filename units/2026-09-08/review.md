# CSC110: Python expressions and Boolean logic — review and guidance

Unofficial student study notes. Consult official course materials for current requirements. Practice questions are not official assessments.

## Key points and traps

### The one-minute revision check

| Remember | Trap to avoid |
|---|---|
| `2` is an `int`; `2.0` is a `float`. | Equal numeric values need not have the same type. |
| `5 / 2` → `2.5`; `5 // 2` → `2`; `5 % 2` → `1`. | Division, floored quotient, and remainder are different operations. |
| `**` means power; grouping changes the result. | Reading an expression strictly left to right. |
| `True` and `False` need capitals. | Treating undefined lowercase `true` as a Boolean. |
| `==` compares; `!=` means not equal. | Using single `=` in an equality question. |
| `<` excludes equality; `<=` includes it. | Missing the equality boundary. |
| `and` needs both; `or` needs at least one. | Assuming `or` means exactly one, or two false inputs make `and` true. |
| Two `not`s restore the original Boolean. | Losing track of successive reversals. |
| Some float calculations are approximate. | Assuming `(2 ** 0.5) ** 2 == 2` must be `True`. |
| A comment-only line has no expression output. | Answering `None` or `0`. |

### What has assessment evidence?

The professor explicitly showed **six sample quiz/test expressions** covering `/`, `//`, arithmetic grouping, numeric equality, and floating-point equality. Their worked explanations are in the notes. Practise predicting those kinds of outputs before running Python.

The other traps above are revision priorities inferred from the teaching, not promises about the next quiz. Negative division is an optional aside; its assessment relevance is unclear. Some term-test questions will resemble assignment questions, so work through assignments yourself even when using help.

## Professor’s conventions

### Answering and presenting code

- **Output questions:** write the actual result (`2`, `2.5`, `True`, etc.). Include a type or explanation when requested.
- **In-class practice:** predict the answers first, then check them in Python.
- **Save submitted programs in files.** Use parentheses when they improve readability.

### What affects marks and available resources?

| Assessment | Instructions from the lecture |
|---|---|
| Coding checkpoint | Pass **all automated tests**; you can see test results and resubmit. Standard VS Code is provided in the lab; no internet or installing other software. |
| Paper checkpoint | Closed book; the answer must be correct, with minor mistakes tolerated. |
| Assignments | Judged on **correctness, design/style, and communication**: code and written responses should be understandable, and code should be documented. |
| Term tests | Closed book with a provided reference sheet; practise **handwriting code**. Minor syntax mistakes are tolerated. |

Checkpoint quizzes are closed book in both formats. The retake schedule and eligibility rules are in the course guide.

### Using help and citing AI

AI assistance is allowed for **weekly preps and assignments**. When you copy AI-generated code or other content, identify what came from AI—a comment or note identifying the affected lines/content is sufficient for the format discussed here. **No specific citation style was required.** Editor-integrated AI still needs attribution.

Standard course materials and Python documentation do not need citations under the guidance given. Do not copy solutions verbatim from friends or online sources. Use the course academic-integrity page for the full rules.

- **Check your own learning when using AI:** after reading a solution, practise producing and explaining it independently; recognizing an answer is different from generating one on a test. Check the correctness of output you use and whether its citations actually exist. Reading, writing, and debugging code yourself builds the fundamentals needed to assess AI output.

## If you missed class: actions and assessment logistics

### Do before the next class

- Complete **Week 1 Preparation on Quercus by Thursday's class**: reading plus a short quiz. The next lecture assumes that preparation.
- Review the syllabus and finish the software installation guide. Use the course's Python **3.13** setup; the professor cited library compatibility as the reason. Follow the guide for the exact version.
- Join **Ed Discussion** through Quercus, add a profile picture, and introduce yourself in the Welcome thread.
- Bring a laptop/tablet or a printed worksheet. **Worksheets are ungraded practice.** The Welcome Survey was encouraged but optional.

### Checkpoint quizzes and retakes

Checkpoints are **25 minutes** and worth **2% each**, using either a computer or paper. Tutorials put the current quiz attempt in the first 25 minutes of their second hour, and the previous week's retake in the second 25 minutes.

You get up to **three attempts without penalty**. Attempts one and two are pass/retake; the third is a guided verbal interview with course staff, where partial credit is available. You must participate in **at least one of the first two attempts** to qualify for the third. Missing attempt one still allows attempt two. If you miss both, use the syllabus's special-consideration process; reweighting is not automatic.

### Term tests and support

Term tests are on paper, including handwritten code; the lecture estimated roughly **100 minutes**. The final has the same general format, is **three hours**, and is cumulative. See **Professor's conventions** for resource and marking rules.

Slides and exercises are posted on Quercus before class. Use the course notes for reading and course staff/Ed for questions. The lecture opened with a five-minute icebreaker.
