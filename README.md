# Agent Historian

> **Retired — October 7, 2026.** Development and adoption of the custom Agent
> Historian / Agent Bookkeeper / MemPalace stack have ended. This repository
> preserves the research, implementation, and unfinished plans as historical
> material; they are not an active roadmap or deployment instructions. Native
> agent memory and local session history remain outside this retirement.

Agent Historian is a reusable architecture and integration project for
cross-session agent continuity.

It separates three concerns that are often conflated:

```text
Agent Bookkeeper
  -> capture, canonical session evidence, archive indexing, search, and provenance

Optional learned-context workflow
  -> deliberate, agent-authored, evidence-backed project briefings
```

The project is intentionally not a replacement for source control, repository
documentation, or human judgement. Current code, repository instructions, and
decision records remain authoritative over learned context.

## Status

Archive/search is the accepted first product boundary. The Hindsight transcript
mining pilot established useful limits, but is not an adopted integration:
transcript-only extraction cannot authoritatively model a mutable codebase.
Any later learned-context workflow is separate from Bookkeeper and is fed by
deliberate agent curation over current project state and source evidence.

## Principles

- Preserve raw session evidence before deriving indexes or learned summaries.
- Keep archive/search and learned memory independently rebuildable.
- Make source revision and provenance visible to every derived result.
- Treat the current checkout, repository documentation, and decision records as
  authoritative; learned context is supporting, revision-scoped interpretation.
- Fail open: a memory-system outage must not prevent an agent from working.
- Keep transport, storage, and deployment configuration operator supplied.

## Documents

- [Architecture](docs/architecture.md)
- [Archive-search pivot and learned-context boundary](docs/archive-search-pivot.md)
- [Market landscape](docs/market-landscape.md)
- [Adoption plan](docs/adoption-plan.md)

The companion [Agent Bookkeeper](https://github.com/byebyebryan/agent-bookkeeper)
project is the concrete capture, archive, and search product.

## Validation

Run:

```bash
rtk git diff --check
```
