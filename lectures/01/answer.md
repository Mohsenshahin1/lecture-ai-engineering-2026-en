# Lecture 01 Answers

## Exercises

- Model's answer to the course-specific question (the 3 homework levels; a summary is fine): The model explicitly said it does not have access to real-time information or specific details about "the AIE2026 course" and stated it could not provide the three homework levels for that course.

- What you noticed when comparing it with the correct meaning in the glossary: The model did not hallucinate or guess an answer; it correctly recognized that it had no training data about this specific course and said so honestly. The real answer, found in data/glossary.json, is that the course has three homework levels (Basic, Implementation, Advanced), and completing any one level is sufficient to complete the homework (it is not graded). This is the opposite failure mode from what I expected: instead of confidently making up a plausible-sounding but wrong answer, the model openly admitted the gap in its knowledge. This also matches the clear contrast with the "What is LoRA?" question, where the model gave a long, confident, and correct answer (600+ output tokens) because LoRA is general public knowledge it was trained on, while this course-specific question only produced 42 output tokens because there was simply nothing in its training data to draw on.

- Token counts you displayed (input / output): input=18, output=42

## Homework

- Level attempted: Basic

- What you did: Ran the same question ("What is LoRA?") three times using three different models via the AIE_MODEL environment variable: the default model (gemini-3.5-flash-lite), gemini-3.5-flash, and gemini-3.1-flash-lite.

- Results: Input tokens stayed identical at 6 across all three models, since the question text was unchanged. Output tokens varied significantly: gemini-3.5-flash-lite produced 638 output tokens, gemini-3.5-flash produced 1023 output tokens (the longest, about 60% more than flash-lite), and gemini-3.1-flash-lite produced 778 output tokens. The full "flash" model gave the most technically detailed answer, including a concrete numerical example of the matrix decomposition (reducing a 4000x4000 matrix update into two much smaller 4000x8 and 8x4000 matrices), a mention of QLoRA as a related technique, and a disambiguation note about "LoRa" (lowercase) referring to unrelated IoT radio technology. The two "lite" models gave more concise answers that still correctly covered the core idea (freezing the base model and training small low-rank adapter matrices) and used a similar analogy (comparing the base model to a textbook and LoRA to sticky notes added to it), but with fewer extra details and examples.

- Observations and questions: Even though the input was identical every time, just switching the model changed the output length by up to 60%, with no change to the question itself. This shows that model choice alone can significantly affect both answer depth and token cost, independent of prompt engineering. All three models agreed on the core technical explanation, so the difference was mainly in depth and extra context rather than correctness. This makes me wonder how much of this verbosity difference comes from model size/capability versus a deliberate "lite" design choice aimed at conciseness and lower cost.
