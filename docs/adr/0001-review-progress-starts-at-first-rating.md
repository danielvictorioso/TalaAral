# Review Progress starts at the first Rating; only Timed Exams bring Questions into review

A Question has no Review Progress row until it receives its first Rating, and a Timed Exam is the only Study Mode that can give a never-reviewed Question that first Rating. SRS Review shows only Questions that are already due, and Practice never touches Review Progress. So a student who has only generated Questions, or only used Practice, sees an empty SRS Review queue, whose empty state points them to a Timed Exam. We chose this so that FSRS learns only from deliberate, exam-condition recall: drilling in Practice can't inflate intervals, and Review Progress rows record exactly the real review history.

## Considered Options

- **Create a `new` Review Progress row at Generation.** Rejected. Every Question, including ones only ever used in Practice, would carry an empty row, and the soft-delete cascade and v1.1 sync would have more rows to handle, without the scheduler learning anything.
- **SRS Review introduces up to N never-reviewed Questions per day** (Anki-style "new cards per day"). Deferred, not ruled out. It adds a setting and changes what SRS Review means. Revisit it if students rely on Practice and never take Timed Exams.

## Consequences

- Questions left unanswered in a Timed Exam get no Rating, so a Question that is never answered never enters review.
- `recordReview` refuses any session that isn't `srs_review`; Timed Exam Ratings are applied by `completeQuizSession`.
