---
name: align-architecture
description: After business knowledge is clear, read global architecture and domain docs, align task-level architecture with the user, then write decisions into the feature spec and optionally update global docs. Prefer diagrams over long prose.
disable-model-invocation: true
---

# Align Architecture

Business alignment is already done. Align the technical shape of this feature with the existing architecture and the user.

Do not re-interview the business. Do not invent new requirements.

Prefer diagrams over long prose to reduce ambiguity.

## Steps

1. Read current context:
   - `.scratch/<feature>/spec.md` if it exists (otherwise use the conversation)
   - Relevant files under `docs/domain/` and `docs/architecture/`
   - Relevant ADRs in `docs/adr/`
   - `CONTEXT.md`

2. Identify architectural impact. Before proposing any change, apply these checks in order:
   - Does this need to exist at all for the current feature? (YAGNI — if not, skip it)
   - Already in this codebase? Reuse it.
   - Can an existing module absorb this with a small interface change?
   - Does this require a new module? Justify why it cannot be absorbed.
   - Where are the main seams?
   - What data or schema changes are involved?

   When reasoning about modules, seams, depth, or leverage, call the `codebase-design` skill if it is available.

3. Align with the user in one short round:
   - Propose a concrete architectural approach that prefers reuse and the smallest possible surface.
   - Explicitly list the existing modules you checked for reuse.
   - Use a class diagram and/or sequence diagram to show the proposal (Mermaid).
   - Ask at most 2–4 clarifying questions about modules, seams, or boundaries.
   - Wait for the user before writing.

4. Write the decisions:
   - Update or create the **Implementation Decisions** section in `.scratch/<feature>/spec.md` using the structure below.
   - Prefer Mermaid class diagrams and sequence diagrams over long prose.
   - If the decision affects the global architecture, update the relevant file in `docs/architecture/` or `docs/domain/`.
   - Write a new ADR only for decisions that are hard to reverse or affect multiple features.
   - If the user insists on a new module despite reuse being possible, record the reason and the risk in the decision.

5. Confirm with the user:
   - Show what was written and which files changed.
   - Ask the user to confirm the Implementation Decisions before proceeding to `/to-spec`.

## Implementation Decisions Structure

```markdown
## Implementation Decisions

### Modules Touched
- Reuse: ...
- Modify: ...
- New: ... (justify why not reuse)
- Modules checked for reuse: ...

### Seams & Interfaces
- Primary test seam: ...
- Interface changes: ...

### Class Diagram
```mermaid
classDiagram
    ...
```

### Sequence Diagram
```mermaid
sequenceDiagram
    ...
```

### Data / Schema
- ...

### Global Impact
- Update `docs/architecture/xxx.md`: yes/no
- New ADR: yes/no

### Explicit Non-Goals
- ...

### Risks (if any)
- ...
```

## Rules

- Prefer the smallest change that solves the current feature. Justify every new module, abstraction, or interface.
- No unrequested abstractions: no new interface with one implementation, no factory for one product, no config for a value that never changes.
- Prefer deletion or simplification over addition when both work.
- Boring over clever. The first lazy solution that works is the right one.
- Prefer diagrams (class + sequence) over long prose to avoid ambiguity.
- Keep the alignment round short. If the user wants deeper discussion, stop and suggest `/grilling`.
- Do not write file paths or code snippets unless a prototype already produced a decision-rich fragment.
- Global docs stay sparse. Only update them when the feature changes the shared model.
- Use codebase-design vocabulary where it helps (module, seam, depth, leverage, locality).
- After writing, tell the user what was written and which files changed.
