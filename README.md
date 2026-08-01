# Agent Historian

Agent Historian is a reusable architecture and integration project for
cross-session agent continuity.

It separates three concerns that are often conflated:

```text
Agent Bookkeeper
  -> canonical session evidence, revisions, and consumer delivery

MemPalace or equivalent retrieval system
  -> verbatim evidence search and provenance

Hindsight or equivalent learned-memory system
  -> distilled, scoped context for future tasks
```

The project is intentionally not a replacement for source control, repository
documentation, or human judgement. Current code, repository instructions, and
decision records remain authoritative over learned context.

## Status

Design and integration bootstrap. No runtime, storage provider, retrieval
engine, or learned-memory product is required by this repository.

## Principles

- Preserve raw session evidence before deriving indexes or learned summaries.
- Keep archive/search and learned memory independently rebuildable.
- Make source revision and provenance visible to every derived result.
- Fail open: a memory-system outage must not prevent an agent from working.
- Scope recall and retention by project, user, and agent identity.
- Keep transport, storage, and deployment configuration operator supplied.

## Documents

- [Architecture](docs/architecture.md)
- [Market landscape](docs/market-landscape.md)
- [Adoption plan](docs/adoption-plan.md)

The companion [Agent Bookkeeper](https://github.com/byebyebryan/agent-bookkeeper)
project defines the session-evidence transport and consumer data plane.

## Validation

Run:

```bash
rtk git diff --check
```
