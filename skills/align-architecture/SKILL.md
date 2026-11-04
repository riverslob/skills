---
name: align-architecture
description: After business knowledge is clear, read global architecture and domain docs, align task-level architecture (module division, responsibilities, reuse and adjustments, domain and call diagrams) with the user, then write decisions into the feature spec and optionally update global docs.
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
   - Module division for this feature, and one responsibility per module.
   - For every existing module in scope: reuse unchanged, or the concrete adjustment. Name what is reused and what changes.
   - New module only when an existing one cannot absorb the work. State why.
   - Static relationships between domains (for the class diagram).
   - Dynamic call relationships between modules or components (for the sequence diagram).
   - Main seams, and data or schema changes.

3. Align with the user in one short round:
   - Propose the module map: division, responsibility, reuse, adjustment, new.
   - Include a draft domain class diagram and a draft call sequence diagram (mermaid).
   - Ask at most 2–4 clarifying questions about modules, seams, or boundaries.
   - Wait for the user before writing.

4. Write the decisions:
   - Update or create the **Implementation Decisions** section in `.scratch/<feature>/spec.md` using the structure below. Every in-scope existing module appears under Reuse or Adjust. Both diagrams are present.
   - If the decision affects the global architecture, update the relevant file in `docs/architecture/` or `docs/domain/` with the same module map and diagrams.
   - Write a new ADR only for decisions that are hard to reverse or affect multiple features.

## Implementation Decisions Structure

Write this section. Put each diagram in its own fenced `mermaid` block.

    ## Implementation Decisions

    ### Module Design
    - Division: ...
    - `<module>`: <responsibility>

    ### Existing Modules
    - Reuse `<module>`: <what is reused, unchanged>
    - Adjust `<module>`: <what changes and why>

    ### New Modules
    - `<module>`: <responsibility>. Why not absorbed: ...

    ### Relationships

    #### Domain
    classDiagram of domains and the relationships between them.

    #### Calls
    sequenceDiagram of calls between modules or components.

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
- List every in-scope existing module as Reuse or Adjust. Implicit reuse is incomplete.
- Diagrams are mermaid. Class diagram: domain relationships. Sequence diagram: calls between modules or components. Only participants and relations decided in alignment.
- Keep the alignment round short. If the user wants deeper discussion, stop and suggest /grilling.
- Do not write file paths or code snippets unless a prototype already produced a decision-rich fragment.
- Global docs stay sparse. Only update them when the feature changes the shared model.
- Use codebase-design vocabulary where it helps (module, seam, depth, leverage, locality).
- After writing, tell the user what was written and which files changed.
