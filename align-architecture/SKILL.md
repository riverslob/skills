---
name: align-architecture
description: After business knowledge is clear, read global architecture and domain docs, align task-level architecture with the user, then write decisions into the feature spec and optionally update global docs.
disable-model-invocation: true
---

# Align Architecture

Business alignment is already done. Align the technical shape of this feature with the existing architecture and the user.

Do not re-interview the business. Do not invent new requirements.

## Steps

1. Read current context:
   - `.scratch/<feature>/spec.md` if it exists (otherwise use the conversation)
   - Relevant files under `docs/domain/` and `docs/architecture/`
   - Relevant ADRs in `docs/adr/`
   - `CONTEXT.md`

2. Identify architectural impact:
   - Which existing modules will be touched or reused?
   - Does this require a new module? Why can’t it be absorbed into an existing one?
   - Where are the main seams?
   - What data or schema changes are involved?

3. Align with the user in one short round:
   - Propose a concrete architectural approach (prefer reuse of existing modules).
   - Ask at most 2–4 clarifying questions about modules, seams, or boundaries.
   - Wait for the user before writing.

4. Write the decisions:
   - Update or create the **Implementation Decisions** section in `.scratch/<feature>/spec.md` using the structure below.
   - If the decision affects the global architecture, update the relevant file in `docs/architecture/` or `docs/domain/`.
   - Write a new ADR only for decisions that are hard to reverse or affect multiple features.

## Implementation Decisions Structure

```markdown
## Implementation Decisions

### Modules Touched
- Reuse: ...
- Modify: ...
- New: ... (justify why not reuse)

### Seams & Interfaces
- Primary test seam: ...
- Interface changes: ...

### Data / Schema
- ...

### Global Impact
- Update `docs/architecture/xxx.md`: yes/no
- New ADR: yes/no

### Explicit Non-Goals
- ...

## Rules
- Prefer reuse of existing modules. Justify every new module.
- Keep the alignment round short. If the user wants deeper discussion, stop and suggest /grilling.
- Do not write file paths or code snippets unless a prototype already produced a decision-rich fragment.
- Global docs stay sparse. Only update them when the feature changes the shared model.
- Use codebase-design vocabulary where it helps (module, seam, depth, leverage, locality).
- After writing, tell the user what was written and which files changed.