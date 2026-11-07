# Lecture 02 Answers

## Exercises

- Model and settings used (from `config` in `result.json`): Qwen/Qwen3.5-0.8B, lora_r=16, lora_alpha=32, lr=0.0002, num_train_epochs=20, seed=42 (default notebook settings)
- Did you run training yourself (`executed_training_myself`): Yes
- Before training: `format_rate` = 0.952 (95.2%) / `content_rate` = 0.381 (38.1%)
- After training: `format_rate` = 1.0 (100%) / `content_rate` = 0.905 (90.5%)
- Questions that regressed: None (the `regressed` list in result.json is empty)
- Failures remaining after training and their types: One remaining assertion-without-evidence failure. The question "When is the final assignment deadline for this course?" is absent from the FAQ (the FAQ only defines a deadline for "the assignment", not specifically "the final assignment"), yet the model confidently asserted the same date from the FAQ both before and after training, instead of requesting confirmation (needs_review: true).
- Results of knowledge questions (section 6; classification before → after training) and observations: 3 of 4 questions kept the same correct classification before and after training: the in-FAQ control question stayed correctly "asserted", and two genuinely out-of-FAQ questions (instructor's name, next lecture topic) stayed correctly "needs_review" (their wording also became more consistent/templated after training). The 4th question ("final assignment deadline") stayed incorrectly "asserted" both before and after, showing the model conflated it with the similarly worded in-FAQ question.
- What changed and what stayed the same with training: What changed: content_rate rose sharply from 38% to 91% and format_rate reached 100%, with zero regressions. What stayed the same: the model's tendency to confidently assert an answer when a question closely resembles a known FAQ item, but is actually subtly different and unanswered, was not corrected by training.

## Homework

- Level attempted: Basic
- Settings or data changed: Changed `num_train_epochs` from 20 (original run) to 5, while keeping `lora_r=16` and `lr=0.0002` unchanged.
- Results (comparison tables or numbers):

| Metric | 20 epochs (original) | 5 epochs (changed) |
|---|---|---|
| before: format_rate / content_rate | 95.2% / 38.1% | 95.2% / 38.1% (identical, as expected since no training had happened yet at the "before" measurement point) |
| after: format_rate | 100% | 100% |
| after: content_rate | 90.5% | 71.4% |
| regressed questions | 0 | 1 ("When will grades be released?") |
| training loss at step 10 | 1.691687 | 1.713 (final; training stopped here) |
| final training loss | 0.014826 (step 40, end of training) | 1.713 (step 10, end of training) |
| training time | 183 seconds (Steps: 40, Epochs: 20.0) | 50 seconds (Steps: 10, Epochs: 5.0) |

- Discussion of when fine-tuning is and is not suitable: Reducing num_train_epochs from 20 to 5 kept format_rate at a perfect 100% in both cases, showing the model learns the required JSON structure almost immediately, even with very little training. However, content_rate dropped noticeably from 90.5% to 71.4%, and a new regression appeared (a question that was correctly handled before training became incorrect after only 5 epochs of training). The training loss curve makes this very clear: at the same step (step 10), both runs had an almost identical loss (~1.69-1.71), but the 20-epoch run continued training down to a final loss of 0.0148, while the 5-epoch run stopped at that same early, barely-reduced loss. This shows that under-training does not just plateau at a lower score, it can also destabilize behavior on questions the untrained model had actually answered correctly. This matches the lecture's core point: fine-tuning is well suited for teaching a consistent output format or behavior, which converges very fast (format_rate hit 100% in both runs), but is much less reliable for making the model's actual content/knowledge consistently correct, where the amount of training matters far more and insufficient training can even hurt rather than simply underperform.
  
