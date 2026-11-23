# TalaAral — Project Rules

These rules apply to the entire TalaAral repository. Follow [docs/TECHNICAL_SPEC.md](./docs/TECHNICAL_SPEC.md) as the source of truth for architecture and [GitHub Issues](https://github.com/danielvictorioso/TalaAral/issues) as the single source of truth for implementation progress (v1.0 spec: #1; tickets are its sub-issues).

## 1. Architecture & Dependency Boundaries
- **Repository Port Isolation**: All UI and feature code MUST go through `StudyEngineRepository`. Never import `llama_cpp_dart` in web-reachable code, and never call FastAPI HTTP endpoints directly from Flutter widgets.
- **Schema Parity (Drift & Alembic)**: Every table or column change MUST be applied to both `app/lib/core/adapters/storage/app_database.dart` (Drift) and `backend/alembic/versions/` (PostgreSQL) in the same change.
- **Domain Vocabulary**: Use the terms in [GLOSSARY.md](./GLOSSARY.md) in code, UI copy, and docs (e.g. "Notebook", never "deck").
- **UUIDv7 & Soft Deletes**: All primary keys MUST use client-generated UUIDv7. Mutable tables MUST retain `created_at`, `updated_at` and nullable `deleted_at` for v1.1 sync compatibility. Append-only tables (`quiz_attempts`) carry only `created_at`.
- **FSRS Coupling**: Ratings are applied only by `completeQuizSession` (for `timed_exam`) and `recordReview` (which rejects anything but `srs_review`); `practice` records Quiz Attempts only. A `review_progress` row is created at a Question's first Rating, never at Generation (ADR-0001). `is_correct` is always derived from `answer_score >= 0.7`, never from the model's output.
- **Pure FSRS State**: `review_progress` MUST store FSRS fields (`state`, `stability`, `difficulty`, `reps`, `lapses`, `scheduled_days`, `last_review_at`, `next_review_at`, `last_rating`) — do not reintroduce SM-2 `ease_factor`.
- **Stateless Web v1.0**: Keep the v1.0 FastAPI backend stateless (browser `IndexedDB` via Drift Web holds v1.0 web user state; PostgreSQL migrations stay ready for v1.1 opt-in sync).

## 2. Mobile Offline Safety
- **Sub-500 MB Installer Budget**: Never bundle `.gguf` weights or large binary assets inside the Flutter app binary. Keep the release `.aab` / `.ipa` under 45 MB (hard ceiling: 500 MB).
- **Isolate-Only Inference**: All `llama.cpp` inference and PDF parsing on mobile MUST run off the main UI isolate.
- **OS-Compliant Model Storage**: Never store downloaded `.gguf` packs in iOS `Documents/` (avoids iCloud backup / App Store rejection). Use `Application Support` with backup exclusion on iOS and app-private storage on Android.

## 3. Verification & Quality Gate
- Never mark a task complete without running and passing its verification commands:
  - Flutter/Dart changes: `cd app && flutter analyze && flutter test`
  - Python/FastAPI changes: `cd backend && ruff check . && pytest`
- Fix all analyzer warnings and test failures before concluding work.

## 4. Workflow, Task Tracking & Commits
- Work ticket-by-ticket through the sub-issues of #1, picking only tickets with no open blockers (GitHub's native "blocked by" edges). Tick a ticket's acceptance criteria and close it only after its verification commands pass. Tickets labelled `ready-for-human` are not agent-executed.
- Write commit messages in **Conventional Commits** format (`feat(scope): ...`, `fix(scope): ...`, `docs(scope): ...`, `test(scope): ...`, `chore(scope): ...`).
