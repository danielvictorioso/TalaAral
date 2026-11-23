# TalaAral

A study platform that turns lecture PDFs and class notes into practice questions, timed exams, and spaced-repetition reviews.

## Study material

**Notebook**:
The top-level container for one course or exam topic, owning its Documents and Question Bank.
_Avoid_: Deck, course, folder

**Document**:
One ingested source inside a Notebook — either a text-based PDF or pasted notes — identified by a title.
_Avoid_: File, upload, filename

**Source Chunk**:
A short, overlapping passage of a Document that grounds the Questions generated from it and the grading of answers to them.
_Avoid_: Segment, passage, context

## Questions

**Question Bank**:
The durable set of all Questions belonging to a Notebook; Questions persist and are reused across Quiz Sessions.
_Avoid_: Quiz, question pool

**Question**:
A single practice item of one Question Type, grounded in exactly one Source Chunk.
_Avoid_: Card, item, flashcard

**Question Type**:
The format of a Question: MCQ, true/false, cloze, short answer, or code trace.
_Avoid_: Question format, kind

**Canonical Answer**:
The single answer shown to the student as correct for a Question.
_Avoid_: Solution, key

**Accepted Answers**:
Additional answer variants (cloze only) that also count as correct, beyond the Canonical Answer.
_Avoid_: Alternates, synonyms

**Generation**:
The act of producing new Questions from selected Source Chunks and adding them to the Question Bank.
_Avoid_: Quiz creation

## Studying

**Quiz Session**:
One sitting in which the student answers Questions drawn from the Question Bank, in a given Study Mode.
_Avoid_: Quiz, test, practice test, exam (when meaning the sitting itself)

**Study Mode**:
How a Quiz Session behaves: Timed Exam, Practice, or SRS Review.
_Avoid_: Quiz type

**Timed Exam**:
A Study Mode with a countdown, graded on submit, whose results update Review Progress automatically; the only way a never-reviewed Question enters review.
_Avoid_: Mock exam, test

**Practice**:
A Study Mode for untimed drilling whose results never affect Review Progress.
_Avoid_: Cram, free mode

**SRS Review**:
A Study Mode presenting due Questions, where the student confirms a Rating that updates Review Progress.
_Avoid_: Review session, flashcards

**Quiz Attempt**:
One immutable record of a student's answer to one Question within a Quiz Session.
_Avoid_: Response, answer record

**Answer Score**:
A single 0–1 measure of how correct an answer is; deterministic Question Types score exactly 0 or 1.
_Avoid_: AI score, grade (when meaning the number)

**Rating**:
The FSRS judgment of recall for a reviewed Question: Again, Hard, Good, or Easy.
_Avoid_: Grade, difficulty

**Review Progress**:
The spaced-repetition memory state of one Question, deciding when it is next due. A Question has none until it receives its first Rating.
_Avoid_: SRS data, schedule, ease

## Offline models

**Model Pack**:
A downloadable on-device language model (Lite or Standard) that powers Generation and grading on mobile.
_Avoid_: Model, weights, GGUF (in user-facing language)
