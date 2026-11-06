# Parsing & Multi-part Simulation: Progressive Questions

**Asked at (2025–26 reports):** Stripe, Apple, Booking, Google

**Signal:** evaluate an expression, decode a format, process records in several steps.

**Core idea:** Use a stack for nesting. Write a small tokenizer first. Keep each step in its own function so the next requirement is easy to add.

**Level:** starts at Medium, since you're already intermediate. (P) = needs LeetCode Premium.

**Already solved in NeetCode 150 (redo only if rusty):** 20 Valid Parentheses, 150 Evaluate Reverse Polish Notation

**Also practice (Stripe style):** take a CSV or JSON file, parse it, validate it, then report on it. Add one new rule per stage. That exercise is in the LLD Questions file.

## Stage 1: Core (Medium)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [227 Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) | M | + − * / without brackets. Apple asked a calculator-style parser. |  |  |
| [394 Decode String](https://leetcode.com/problems/decode-string/) | M | stack of (count, partial string) |  |  |
| [71 Simplify Path](https://leetcode.com/problems/simplify-path/) | M | split, then a stack |  |  |
| [722 Remove Comments](https://leetcode.com/problems/remove-comments/) | M | state machine over characters |  |  |
| [640 Solve the Equation](https://leetcode.com/problems/solve-the-equation/) | M | parse each side into ax + b |  |  |

## Stage 2: Twists (Hard)

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [224 Basic Calculator](https://leetcode.com/problems/basic-calculator/) | H | adds brackets and unary minus |  |  |
| [726 Number of Atoms](https://leetcode.com/problems/number-of-atoms/) | H | stack of count maps |  |  |
| [1096 Brace Expansion II](https://leetcode.com/problems/brace-expansion-ii/) | H | recursive descent; builds on your brace-expansion practice |  |  |
| [591 Tag Validator](https://leetcode.com/problems/tag-validator/) | H | careful state machine |  |  |

## Stage 3: Interview-hard

| Problem | Level | Hint | Status | My Notes |
|---|---|---|---|---|
| [68 Text Justification](https://leetcode.com/problems/text-justification/) | H | pure simulation; clean code matters |  |  |
| [736 Parse Lisp Expression](https://leetcode.com/problems/parse-lisp-expression/) | H | recursion + scoped variables |  |  |
| [770 Basic Calculator IV](https://leetcode.com/problems/basic-calculator-iv/) | H | polynomial terms; very hard, optional |  |  |

## You've mastered it when

- You can solve a Stage 3 problem in about 40 minutes without hints.
- You can say the signal and the template out loud before you code.
