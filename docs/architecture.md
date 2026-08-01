# Architecture

## Goal

Give fresh agent sessions useful continuity without pretending that one memory
product is the source of truth.

The system needs to answer two different questions:

1. **What actually happened?** Retrieve original session evidence with a path,
   revision, and location that can be inspected.
2. **What should a new task know?** Recall a small, relevant set of durable
   decisions, preferences, experiences, and open questions.

## Layering

```text
Authoritative project state
  code, tests, documentation, ADRs, repository instructions
                     |
                     v
Agent Bookkeeper
  raw session archive, revision ledger, delivery to consumers
            |                              |
            v                              v
Evidence retrieval                     Learned context
MemPalace or equivalent                Hindsight or equivalent
verbatim search + provenance           retain, recall, consolidation
            |                              |
            +-------------> Agent task <---+
                         bounded context
```

Agent Historian defines the policy and integration boundaries across these
layers. It does not make a search index or a learned-memory database canonical.

## Authority and rebuildability

| Layer | Authority | Rebuild rule |
| --- | --- | --- |
| Project state | Repository and operator-owned sources | Never replace with learned memory. |
| Raw sessions | Bookkeeper archive plus the original client source until archive recovery is accepted | Preserve exact bytes and revisions. |
| Evidence retrieval | Derived index | Rebuild from raw sessions. |
| Learned context | Derived interpretation | Rebuild or correct from source evidence; optionally back up if it later becomes operationally valuable. |

Every learned or retrieved item should preserve a source reference. A source
reference identifies enough information to find the original session revision,
not merely a generated summary.

## Integration policy

### Evidence retrieval

Use a retrieval system for historical questions, command provenance, design
reasoning, and source-backed search. It should read a committed, read-only raw
projection and remain unable to mutate the archive.

### Learned context

Use a learned-memory system for bounded prompt-time recall, project conventions,
repeated failures, temporal facts, and consolidated observations. It must:

- use project-scoped banks by default;
- retain bounded user/assistant content, with raw tool output excluded unless a
  policy explicitly allows it;
- limit recall injection and local-model concurrency;
- fail open on timeout or service failure; and
- preserve provenance to the Bookkeeper session/revision.

### Transport and archive

Bookkeeper is the only component that accepts canonical session bytes. Retrieval
and learning consumers receive committed revisions and keep independent cursors.
They must not compete to install their own opaque client-side transcript capture
or make a source tree mutable.

## Non-goals

- Replacing Git, code review, ADRs, or repository instructions.
- Treating a semantic search result as authoritative without source inspection.
- Uploading complete transcript history into every prompt.
- Coupling a personal deployment topology to this reusable design.
- Enabling automatic learned-memory extraction before archive, scope, and
  failure behavior are demonstrated in real work.
