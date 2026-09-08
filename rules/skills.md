## Skills

> **This file covers where skills are *stored*. How they get *found and used* is
> [`capability-retrieval.md`](capability-retrieval.md) — read it too.** For a long time
> this file was the framework's only word on skills, which meant Matrix had a filing
> system and no index: a full venture session once ran research → strategy → build →
> deploy and invoked **zero** of 100+ available skills. Storage without retrieval is
> half a system.

Skills are reusable workflows that any session can invoke via `/<skill-name>`. They live in a separate, shareable repo — not in any single project's `.claude/skills/`.

### Where skills live

- All skills must be **shareable** and **reusable** by other users.
- Save them to a dedicated skills repo (e.g. `~/.local/share/skill-mix/sources/skills@<your-handle>/`), not to any specific project's `.claude/skills/` directory.
- Project-local skills are an anti-pattern: they cannot be discovered by agents working in other contexts.

### What skills must NOT contain

- **No config.** No URLs, no chat IDs, no API endpoints, no project-specific path mappings, no per-org IDs.
- Config belongs in the relevant org or project CLAUDE.md.
- Skills must stay generic and reusable across instances.

If a skill needs config, it should reference it abstractly (e.g. "the Control Center project number — see your private CLAUDE.md") rather than hardcode a value.

### Naming and review

- Before publishing a new skill, always review it with `/skill-creator`.
- Skill names should be verbs or noun-phrases that describe what they do (e.g. `daily-review`, `process-inbox`, `image-from-gemini`).
- A skill SKILL.md must specify: trigger conditions, step-by-step process, inputs/outputs, templates used, integration points.

A well-written skill lets anyone (human or AI) reproduce the workflow from scratch without having met the original author.

### Every new skill gets a workbook row

Publishing a skill is not finished until it is **findable**. A skill nobody can locate at
the moment of need is functionally identical to a skill that does not exist — that is the
exact failure `capability-retrieval.md` exists to fix.

So the last step of publishing is: add a row to `ops/capability/workbook.md` giving the
**situation** the skill serves, its **trigger words** (the words an agent would actually
use when it has that situation, not the skill's own name), and the **owning agent**.
Architect owns the workbook and will accept or reshape the row.
