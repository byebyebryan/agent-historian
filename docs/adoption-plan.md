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
   The initial reference profile uses Hindsight with one local extraction
   request at a time.
2. Create a distinct Bookkeeper consumer subscription. It receives only a
   verified lease-scoped revision; Hindsight never mounts or owns the raw
   archive.
3. Render only user/assistant messages. Use a stable Bookkeeper record source
   ID as Hindsight's replace/upsert document ID, attach source/revision/event
   metadata, and disable raw document-text storage in Hindsight.
4. Bound a controller run to one small, retained session. Retain synchronously;
   write the Bookkeeper receipt only after Hindsight accepts the request. An
   outage leaves the delivery queued for a later manual run.
5. Keep observations/consolidation and automatic prompt injection disabled.
   Manual recall is the only allowed read path during the pilot.
6. Validate one source-backed recall and one irrelevant-query negative case,
   then review relevance, staleness, provenance, latency, and resource use
   across meaningful real tasks.
7. Add project-specific banks, controlled historical backfill, consolidation,
   or automatic recall only after the initial pilot is useful and its failure
   and correction paths have been reviewed.

## Exit criteria

Do not call the system useful merely because services are running. It should
make fresh tasks faster or more accurate while retaining inspectable evidence,
bounded context, graceful failure, and clear correction paths.
