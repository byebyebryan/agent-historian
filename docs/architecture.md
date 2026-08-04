# Architecture

## Goal

Give fresh agent sessions useful continuity without pretending that one memory
product is the source of truth.

The system needs to answer two different questions:

1. **What actually happened?** Retrieve original session evidence with a path,
   revision, and location that can be inspected.
2. **What should a new task know?** Assemble a bounded briefing from the current
   checkout, repository guidance, and only then durable, source-backed history.

## Layering

```text
Authoritative project state
  code, tests, documentation, ADRs, repository instructions
                     |
                     v
Agent Bookkeeper
  raw session archive, revision ledger, archive indexing, and provenance search
            |
            v
      evidence retrieval
      MemPalace initially
            |
            v
        Agent task

Authoritative project state
  current checkout, tests, documentation, ADRs, repository instructions
            |
            +---------------------------> Agent task
```

Agent Historian defines the policy and integration boundaries across these
layers. It does not make a search index or a learned-memory database canonical.

## Authority and rebuildability

| Layer | Authority | Rebuild rule |
| --- | --- | --- |
| Project state | Repository and operator-owned sources | Never replace with learned memory. |
| Raw sessions | Bookkeeper archive plus the original client source until archive recovery is accepted | Preserve exact bytes and revisions. |
| Evidence retrieval | Derived index | Rebuild from raw sessions. |
| Optional learned context | Derived interpretation | Rebuild or correct from source evidence; never treat as current project state. |

Every learned or retrieved item should preserve a source reference. A source
reference identifies enough information to find the original session revision,
not merely a generated summary.

## Integration policy

### Evidence retrieval

Use a retrieval system for historical questions, command provenance, design
reasoning, and source-backed search. It should read a committed, read-only raw
projection and remain unable to mutate the archive.

### Learned context

Do not derive long-horizon project context by automatically mining raw
transcripts alone. A later learned-context workflow begins with an agent that
can inspect the relevant current checkout, documentation, validation results,
and retrieved source history. It submits small, revision-scoped memory
candidates—decisions, outcomes, recurring failures, or open risks—with direct
evidence references. A learned-memory system may then store or consolidate
those candidates, but it is not a Bookkeeper consumer and it is never the
authority for current code.

### Transport and archive

Bookkeeper is the only component that accepts canonical session bytes. Its
archive-search backend receives committed revisions and must not compete to
install its own opaque client-side transcript capture or make a source tree
mutable. Any optional learned-context workflow uses Bookkeeper retrieval as
evidence; it does not receive an archive-delivery cursor by default.

## Non-goals

- Replacing Git, code review, ADRs, or repository instructions.
- Treating a semantic search result as authoritative without source inspection.
- Uploading complete transcript history into every prompt.
- Treating transcript mining as an automatically correct model of a repository.
- Coupling a personal deployment topology to this reusable design.
