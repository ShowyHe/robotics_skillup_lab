# AGENTS.md

## 1. Repository Purpose

This repository is a robotics skillup learning record.

It stores:
- Daily robotics learning lessons.
- Quiz results.
- Learning progress.
- Module completion status.

The AI agent must preserve historical learning records.

---

## 2. Learning Completion Workflow

Each learning Day follows:

Learning
↓
Quiz
↓
Evaluation
↓
PASS confirmation
↓
Create lesson document
↓
Update progress index
↓
Sync README status if required

Do not mark a Day as completed before Quiz PASS.

---

## 3. File Responsibility

### Lesson Files

Path:
`docs/lessons/dayXXX.md`

Rules:
- One Day corresponds to one file.
- New lesson files can be created directly.
- Completed lessons should not be rewritten without reason.
- Corrections should preserve history.

### Progress File

Path:
`docs/PROGRESS.md`

Rules:
- This is a historical index.
- Never rewrite blindly.
- Preserve previous records.
- Modify only required sections.
- Review diff before commit.

### README

Rules:
- Only synchronize status markers.
- Do not modify unrelated sections.

---

## 4. Tool Selection Rules

Use direct GitHub editing for:
- New lesson files.
- Small isolated markdown changes.

Use Work/Codex for:
- Large files.
- README.md.
- docs/PROGRESS.md.
- Multi-file modifications.
- Code changes requiring testing.

Large file workflow:

Read original file
↓
Modify locally
↓
Review git diff
↓
Commit

---

## 5. Git Commit Rules

Commits should describe the actual change.

Examples:

Lesson:
`docs: add <module/day> lesson`

Progress:
`docs: update <module/day> progress`

README synchronization:
`docs: sync README completion markers`

Avoid mixing unrelated changes in one commit.

---

## 6. Verification Rules

A task is complete only when:

1. File exists.
2. Commit exists.
3. Commit is in the target branch.
4. Diff matches the requested modification.

Never claim push completion without commit verification.

---

## 7. Forbidden Operations

The AI agent must not:
- Delete historical learning records.
- Replace large files without checking diff.
- Modify unrelated files.
- Mark lessons as PASS without Quiz evidence.
- Update progress before lesson completion.
- Claim GitHub completion without commit SHA.

---

## 8. Skillup Record Rules

Learning sequence:

Theory
↓
Engineering mapping
↓
Quiz
↓
Correction
↓
Repository record

Engineering experience can accelerate explanation but cannot replace theory.

Existing knowledge should be recorded as mastered, not removed.

---

## 9. Confirmation Rule

If repository, branch, and target files are already confirmed:

Do not repeatedly request confirmation.

Continue execution until:
- completed;
- blocked by missing information;
- blocked by technical failure.

---

## 10. Learning Schedule Authority

The official Day topic sequence is defined by README.md and docs/PROGRESS.md.

When preparing a new lesson:

- Always follow the repository schedule.
- Do not replace the scheduled topic with a related but different topic.
- Do not advance future topics because they are more related to current engineering work.
- If a topic is technically related but belongs to a later Day, keep it for its assigned Day.

The learning schedule has priority over the AI agent's own topic recommendation.
