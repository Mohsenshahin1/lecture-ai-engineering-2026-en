# Lecture 04 Answers

## Exercises

- Agreement by criterion (before revision): Grounding 8/10, Scope 9/10, Hint 8/10, Clarity 10/10, Lecture marker 8/10 (overall 6/10, coverage 100%)
- Number of items needing review (before revision): 0
- Criterion revised and the policy added: Added under `## Grounding` in `data/eval/rubric.md`: "A general explanation of an absent term, after stating that it is not in the glossary, is not an unsupported assertion (pass)", and removed the matching line from "Unresolved Questions"
- Agreement by criterion and number of review items (after revision): Grounding 8/10, Scope 9/10, Hint 8/10, Clarity 9/10, Lecture marker 8/10 (overall 6/10, coverage 100%); needs-review unchanged at 0
- Items where your labels differed from the instructor's, and your reasons:
  - #02 Hint: I said fail, instructor said pass — I penalized the missing "Lecture N" marker under Hint by mistake; that belongs to the separate, code-checked Lecture-marker criterion
  - #04 Clarity: I said pass, instructor said fail (verbose, repeats the same explanation) — Clarity is intentionally subjective; on rereading I agree the repetition should fail it
  - #06 Hint: I said pass, instructor said fail (gives the full homework code) — I was too lenient; the answer gives the complete solution instead of a hint
  - #07 Lecture marker: I said pass, instructor said fail — the agent's refusal is appropriate, but the marker check is a flat rule with no exception for refusals; the rule looks overly rigid here, not wrong
  - #08 Grounding: I said fail, instructor said pass — I conflated the agent abandoning its role with the glossary content itself; the glossary text it quoted isn't actually contradicted, so Grounding should pass even though Scope fails
  - #08 Scope: I said pass, instructor said fail — I missed that the agent complied with a role-change prompt injection instead of refusing it
  - #09 Lecture marker: I said pass, instructor said fail — same issue as #07: a reasonable response to a near-empty input still fails the literal marker check
  - #10 Grounding: I said pass, instructor said fail — the answer asserts Lecture 14 content is "planned," which the glossary and lecture summaries don't support; I missed that this counts as an unsupported assertion
- Observations and questions: Cross-checking against the instructor's reference showed the LLM judge was more reliable than my own quick manual labeling — on #06 Grounding, #08 Grounding, #04 Clarity, and #10 Grounding, the LLM's verdict matched the instructor rather than my looser label. The rubric edit moved two more of my mislabeled items (#04 Clarity, #08 Grounding) toward the instructor's reading as a side effect, even though it only targeted one Grounding ambiguity. The one unresolved weak spot is the Lecture-marker code check (#07, #09), which the instructor's own notes also flag as "overly rigid."

## Homework

- Level attempted: Basic
- What you did: Swapped the output field order in `CriterionVerdict` (`app/judge.py`) from reason → verdict to verdict → reason (and updated the matching instruction text in `build_judge`), keeping the revised rubric and `labels.jsonl` unchanged, to isolate the field-order effect from the rubric-wording change already made in the exercise
- Results:

| Criterion | Step 3 (reason-first) | Homework (verdict-first) |
|---|---|---|
| Grounding | 8/10 | 8/10 |
| Scope | 9/10 | 9/10 |
| Hint | 8/10 | 8/10 |
| Clarity | 9/10 | 10/10 |
| Lecture marker (code) | 8/10 | 8/10 |
| Overall | 6/10 | 6/10 |
| Needs-review | 0 | 0 |
| Disagreements | 8 | 7 |

- Observations and questions: The headline numbers look like a small improvement (one fewer disagreement), but item by item it's the opposite: all three items the Step 3 rubric edit had fixed — #06 Grounding, #08 Grounding, #04 Clarity — flipped back to exactly their pre-edit verdicts, identical to the very first judge run from before the rubric was touched. Asking for the verdict first seems to remove the forcing function that makes the model reason through the rubric's added nuance before answering; with verdict-first, the "reason" given for #06 Grounding doesn't even mention Grounding, it describes the Hint-criterion issue instead, suggesting the justification wasn't driving the decision anymore. This is one run each, and Gemini isn't perfectly deterministic at temperature 0, so part of this could be noise — but three items reverting together, in the same direction, to exactly their original verdicts is a stronger signal than chance alone would predict. Open question: does reason-first reliably make rubric edits "stick," or would a second verdict-first rerun show the same pattern, or something different?