# AGENTS.md

## 1. Repository Purpose

This repository stores the robotics skillup learning system:
- Daily lessons.
- Quiz results.
- Learning progress.
- Module completion records.

Preserve historical learning records.

---

## 2. Skillup Workflow

Learning
↓
Quiz
↓
Evaluation
↓
PASS confirmation
↓
Lesson record
↓
Progress update
↓
README synchronization if required

Do not mark a Day complete before Quiz PASS.

---

## 3. File Responsibility

### Lesson files
`docs/lessons/dayXXX.md`

- One Day corresponds to one file.
- Preserve completed history.
- Do not rewrite completed lessons without reason.

### Progress
`docs/PROGRESS.md`

- Historical index.
- Modify only required sections.
- Review diff before commit.

### README

- Synchronize status only.
- Avoid unrelated changes.

---

## 4. Curriculum Authority

Source of truth:

- `docs/03_MASTER_PLAN.md`
- `docs/modules/Mxx_*.md`
- `docs/PROGRESS.md`
- README.md

Before generating a lesson:

1. Read current Day objective.
2. Read module teaching contract.
3. Check previous and next Day connection.

The AI explains the curriculum but does not redesign it.

The AI must not:
- Change Day topics.
- Reorder modules.
- Replace curriculum goals with engineering needs.

---

## 5. Lesson Generation Rules

Do not generate a lesson only from the Day title.

Understand:
- Module capability goal.
- Current Day objective.
- Required scope.
- Teaching boundary.
- Previous dependency.
- Future connection.

Lesson structure is flexible. Choose based on topic:

- Mathematics.
- Algorithms.
- Architecture.
- Source code.
- Engineering systems.
- Robot applications.

Do not force identical templates.

Every lesson should clarify:
- Why this topic appears here.
- What problem it solves.
- How it connects to the learning chain.

Engineering projects are only examples and validation. They cannot decide the next lesson.

Before sending:
- Correct Day.
- Correct topic.
- Correct module position.
- No future topic as main content.
- Quiz only after complete lesson.

---

## 6. Tool and Git Rules

Direct editing:
- New lesson files.
- Small isolated markdown changes.

Work/Codex:
- Large files.
- README.md.
- docs/PROGRESS.md.
- Multi-file changes.
- Code changes requiring tests.

Commit messages describe actual changes:

Lesson:
`docs: add <module/day> lesson`

Progress:
`docs: update <module/day> progress`

README:
`docs: sync README completion markers`

Avoid unrelated changes in one commit.

---

## 7. Verification and Forbidden Actions

Complete only when:

1. File exists.
2. Commit exists.
3. Commit is in target branch.
4. Diff matches requested change.

Forbidden:
- Delete historical records.
- Replace large files without checking diff.
- Modify unrelated files.
- Mark PASS without Quiz evidence.
- Claim GitHub completion without verification.
