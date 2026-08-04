# Market Landscape

Status: design input, not a dependency lock.

Agent Historian uses a layered approach because archive retrieval and learned
memory optimize for different outcomes. The comparison below records the
current rationale and should be refreshed before selecting a production
implementation.

| Project | Best role | Strength | Limitation in this architecture |
| --- | --- | --- | --- |
| [MemPalace](https://github.com/MemPalace/mempalace) | Agent Bookkeeper archive/search backend | Semantic/keyword retrieval over source material with provenance-oriented workflow | Derived index; canonical evidence remains the Bookkeeper archive. |
| [Hindsight](https://github.com/vectorize-io/hindsight) | Optional curated learned context | Retain, recall, and consolidation of facts, experiences, entities, and observations | Transcript-only extraction does not model mutable repository state; revisit only with agent-led, evidence-backed curation. |
| [AgentMemory](https://github.com/rohitg00/agentmemory) | Integrated coding-agent memory | Rich lifecycle/tool/file capture, replay-like history, and learned summaries | Overlaps materially with the separate raw archive and retrieval layer; retain as a comparison candidate. |
| [Mem0](https://github.com/mem0ai/mem0) | Custom memory API | Straightforward add/search/update/delete memory interface | Pushes more ingestion, scope, and recall design into the caller. |
| Syncthing, rsync, rdiff-backup | File replication/recovery | Mature transport and retention primitives | They do not define session identity, consumer cursors, or learned-memory policy. |

## Current conclusion

Use Agent Bookkeeper with an archive/search backend first. That is a complete
and useful product without a learned-memory dependency. Hindsight remains a
separate candidate for deliberate, agent-led curation rather than an automatic
Bookkeeper transcript consumer. AgentMemory remains the strongest all-in-one
comparison if detailed tool/file timelines prove more valuable than the layered
boundary.

## Evaluation questions

Before adopting any product, verify:

- Can every result lead back to a raw session revision?
- Are project, user, and agent scopes isolated by default?
- Does a failure preserve normal agent work?
- Are input, output, recall, and concurrency budgets bounded?
- Can raw evidence and derived state be independently rebuilt?
- Does the product reduce repeated context work without injecting stale or
  irrelevant claims?
