# Research and decision lineage

This document preserves the reasoning that led to Agent Historian's current
archive-first boundary. It is historical design evidence, not a second
architecture specification. Current decisions live in
[architecture](architecture.md), the
[archive-search pivot](archive-search-pivot.md), and the
[adoption plan](adoption-plan.md).

## Original problem

The investigation started with a practical continuity problem: task-scoped
agent sessions are easier to operate than project-long sessions, but a new
session should not have to rediscover every prior decision, failed approach,
convention, and open question.

The desired system combined:

- verbatim search over old sessions;
- provenance back to original evidence;
- compact context for a fresh task;
- stable project separation across machines;
- bounded local-model work whose request size does not grow with total
  history; and
- failure isolation so memory infrastructure never blocks ordinary agent work.

The initial hypothesis was a three-layer system:

1. raw agent session files as canonical evidence;
2. MemPalace as a searchable historical archive; and
3. Hindsight as an automatically learned, consolidated context layer.

AgentMemory and Mem0 were retained as alternatives. AgentMemory appeared
closer to an out-of-the-box coding-memory product, including tool and file
history, while Mem0 offered a flexible memory API for a custom extraction and
recall pipeline. Both implied different tradeoffs: AgentMemory overlapped more
with archive/search, while Mem0 left more ingestion and policy machinery to
the integrator.

These product observations are a snapshot of the evaluation period. They are
not current compatibility claims; see the
[market landscape](market-landscape.md) before making a new product choice.

## Questions the pilots tested

The learned-context investigation deliberately separated mechanical questions
from product-capability questions.

### Ingestion mechanics

The experiments asked whether low-quality output came from an avoidable input
mistake:

- Was raw Codex JSONL being rendered as useful user/agent conversation text?
- Were tool events and transport noise excluded?
- Were sessions split at stable logical seams rather than arbitrary bytes?
- Did each extraction receive enough surrounding context?
- Would larger chunks or overlapping context preserve a longer line of
  reasoning?
- Were project identity and source provenance retained?
- Were document text, observations, and consolidation features enabled?

Turn-oriented and bounded multi-turn chunks were preferable to character-only
splitting because they preserve predictable conversational boundaries.
Increasing the model context window and trying larger chunks were valid
diagnostics. They improved input coherence, but did not resolve the central
authority problem below.

### Model and resource envelope

The pilot also considered whether a larger local model or context window would
materially improve learned facts. The governing rule was:

```text
more history -> more bounded jobs
not
more history -> an unbounded request
```

Model size, context length, concurrency, retries, and consolidation batch size
affect extraction quality and throughput. They do not make a transcript a
complete representation of a changing software project. That distinction
prevented continued model and chunk tuning from masking a product-boundary
problem.

## Decisive finding

A coding transcript records what the user and agent said. It does not reliably
contain the current code, complete diffs, repository instructions, test
results, later changes made by other sessions, or every externally observed
fact.

Consequently, unattended transcript mining can produce memories that are:

- locally plausible but detached from current repository state;
- over-specific to one partial task;
- stale after subsequent implementation changes;
- noisy because operational dialogue is mistaken for a durable conclusion; or
- confidently incomplete because the source of truth was never in the input.

This is not merely a bad chunk-size setting. Larger chunks and larger models
may improve summaries of the transcript, but they cannot recover missing
authoritative state. A transcript-only learned layer therefore should not be
presented as a current project model.

## Authority and provenance

The evaluation established the authority order now used by Historian:

1. current checkout, tests, and observed runtime state;
2. current repository instructions, design documents, ADRs, and operator
   runbooks;
3. deliberately curated, revision-scoped context;
4. archive retrieval as historical evidence.

Archive evidence remains valuable precisely because it can answer where and
why something was discussed without pretending that the discussion is still
true. A derived statement should retain enough identity to find its source
session and should state which repository revision or current-state review
supported it.

Stable scope matters as much as extraction. A useful identity is based on a
repository's durable identity, not merely a directory basename, and memories
from unrelated projects must not share an accidental global namespace.

## Accepted pivot

The accepted product boundary is now:

```text
Agent Bookkeeper
  -> canonical session evidence
  -> archive indexing and provenance-bearing retrieval

Agent Historian
  -> overall continuity architecture
  -> authority, integration, and adoption policy

Optional future learned-context workflow
  -> an agent reviews current code, docs, tests, and retrieved evidence
  -> the agent deliberately authors revision-scoped context
```

MemPalace earned its place as a Bookkeeper archive-search backend. It supports
useful semantic and keyword discovery over a unified raw archive while keeping
the original session as the recovery source.

Hindsight's unattended transcript-mining lane was retired. Hindsight or
another memory database could still store curated context later, but that
workflow is independent of Bookkeeper and must not regain ownership of
transcript transport merely because it is a possible consumer.

## What was superseded

The following ideas from the original combined research are intentionally not
current design:

- Hindsight as a standing consumer of every transported transcript;
- automatic recall and retain hooks as the first production boundary;
- raw transcript mining as an authoritative learned project model;
- one generic memory tool or namespace spanning archive evidence and curated
  knowledge; and
- tuning chunking or model size indefinitely before defining what learned
  context is allowed to claim.

The original transport investigation was split into Agent Bookkeeper. Private
deployment paths, hostnames, benchmarks, and recovery procedures belong in the
operator's deployment repository. Backend-specific importer behavior belongs
with that backend. This keeps Historian focused on system boundaries and
decision quality.

## Revisit gates

A learned-context lane should be reconsidered only with a bounded proof that:

- an agent can inspect the current repository state when authoring context;
- every durable statement carries source and revision provenance;
- stale or superseded context has an explicit lifecycle;
- project scope is stable and collision-resistant;
- recall is small, relevant, and advisory;
- raw archive/search continues to work independently; and
- failure or unavailability never blocks ordinary agent work.

Until those gates have a concrete design and useful evaluation, archive/search
is the complete adopted product rather than an incomplete prelude to automatic
learning.
