# Archive-search pivot and learned-context boundary

Status: accepted architecture decision.

## Decision

Agent Bookkeeper plus its archive-search backend is the first complete product:

```text
agent session capture
    -> durable raw archive and revision ledger
    -> MemPalace-indexed evidence search
    -> provenance-bearing retrieval for later agent work
```

MemPalace is the initial archive-search backend within the Bookkeeper system,
not a separately required end-user product. It remains replaceable: raw session
bytes, identity, revisions, and commit receipts are the recovery boundary;
indexes are derived and rebuildable.

## Why the Hindsight transcript-mining integration is retired

The bounded pilot established that transcript-derived facts can be inspectable
and source-provenanced, but it did not establish a useful current project model.
Mechanical choices such as chunk overlap and consolidation can improve recall,
but cannot supply code, repository documentation, test results, or later source
changes absent from the retained input. A transcript fact can be historically
correct yet stale for the current checkout.

Accordingly, Agent Bookkeeper does not own a learned-memory adapter, Hindsight
controller, or learned-memory delivery cursor.

## Deferred learned-context direction

If learned context is revisited, its author is an active agent with a bounded
question, not an unattended transcript miner. The agent should:

1. inspect current code, repository instructions, documentation, and validation
   results;
2. retrieve relevant historical evidence from Bookkeeper search;
3. submit a small candidate only for a durable decision, outcome, recurring
   lesson, preference, or open risk; and
4. attach repository/revision and evidence references, plus an explicit
   supersession or correction path.

The prompt-time context composer resolves current checkout and repository
guidance first. Learned context is supporting interpretation, never authority.

## Consequences

- Preserve raw transcripts and archive-search provenance.
- Keep Hindsight pilot data and research evidence as disposable historical
  artifacts, but remove it from Bookkeeper and default deployment surfaces.
- Do not build additional transcript chunking, overlap, consolidation, or model
  tuning machinery until an agent-led curation pilot has a concrete task-level
  acceptance test.
