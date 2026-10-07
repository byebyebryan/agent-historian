# Adoption Plan

> **Retired — October 7, 2026.** Development and adoption of the custom Agent
> Historian / Agent Bookkeeper / MemPalace stack have ended. This repository
> preserves the research, implementation, and unfinished plans as historical
> material; they are not an active roadmap or deployment instructions. Native
> agent memory and local session history remain outside this retirement.

## Phase 1: evidence first

1. Configure an operator-supplied durable store and a Bookkeeper client for one
   agent/workstation.
2. Preserve raw session bytes and establish revision/provenance checks.
3. Connect an evidence-retrieval engine read-only.
4. Backfill a bounded archive candidate and validate retrieval against original
   sources.
5. Measure resource use, latency, and usefulness during ordinary tasks.

## Phase 2: archive control plane

1. Add Bookkeeper catalog state: stable session identity, revision, location,
   tombstone, and consumer cursor.
2. Automate incremental evidence ingestion only for stable, committed revisions.
3. Keep all maintenance resource-bounded and independently resumable.
4. Prove that archive/unarchive, deletion policy, offline clients, and consumer
   rebuilds behave as documented.

## Deferred: agent-led learned context

Learned context is not on the critical path for archive/search. If it is
revisited, run a small, explicit curation pilot rather than a raw-transcript
backfill:

1. Give an agent a bounded project question or end-of-task checkpoint.
2. Have it inspect the current checkout, repository guidance, validation
   evidence, and relevant Bookkeeper search results.
3. Require revision-scoped, source-backed candidates for durable decisions,
   outcomes, recurring failures, preferences, and open risks.
4. Store candidates in a separate learned-context system only after they have
   explicit provenance and a correction/supersession policy.
5. Evaluate whether a fresh task becomes more accurate without injecting stale
   claims. Current checkout and repository docs always win conflicts.

## Exit criteria

Do not call the system useful merely because services are running. Archive/search
must make historical investigation faster while retaining inspectable evidence,
bounded retrieval, graceful failure, and clear rebuild paths. Any future learned
context must additionally demonstrate that it improves fresh tasks without
misrepresenting current project state.
