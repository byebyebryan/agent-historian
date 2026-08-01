# Adoption Plan

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

## Phase 3: learned-context pilot

1. Choose a learned-memory implementation and an operator-supplied model/API.
2. Use only fresh, task-scoped sessions initially.
3. Start with project-scoped recall, low context budget, one extraction worker,
   and no raw tool-output retention.
4. Run a meaningful set of real tasks, then review relevance, staleness,
   provenance, and failure behavior.
5. Add selected historical backfill only after the live pilot is useful.

## Exit criteria

Do not call the system useful merely because services are running. It should
make fresh tasks faster or more accurate while retaining inspectable evidence,
bounded context, graceful failure, and clear correction paths.
