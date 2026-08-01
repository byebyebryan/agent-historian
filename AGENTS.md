# Repository Instructions

## Command execution

Always prefix interactive shell commands with `rtk`.

## Scope

- Keep this repository reusable: do not commit personal hostnames, private
  network addresses, transcript contents, database exports, tokens, or rendered
  deployment configuration.
- Agent Historian is the architecture and integration layer. It does not own
  canonical session bytes, vector indexes, or a learned-memory database.
- Treat raw session evidence, retrieval indexes, and learned context as
  separate layers with explicit provenance and rebuild boundaries.
- Keep deployment-specific Compose, secret, and host policy in the operator's
  infrastructure repository, not here.

## Editing

- Update the design and adoption documents when a decision changes.
- Preserve alternatives and their trade-offs; do not present an unselected
  product as a dependency.
- Run the documented validation before committing.
