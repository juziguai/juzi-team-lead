# Codex Adapter

Use this adapter when the host is Codex.

## Capability Mapping

| Abstract role | Codex capability |
|---------------|------------------|
| worker | `multi_agent_v1.spawn_agent` with `agent_type: "worker"` |
| reviewer | `multi_agent_v1.spawn_agent` with `agent_type: "explorer"` |
| planner | `multi_agent_v1.spawn_agent` with `agent_type: "default"` |

## Workflow Notes

- Prefer workers for disjoint write scopes.
- Prefer reviewers for read-only validation.
- Prefer planners only when decomposition is needed before implementation.
- Spawn parallel workers when their write scopes do not overlap.
- Keep each worker's ownership explicit and narrow.
- Never ask a worker to verify its own changes.
- If the task is small enough for direct execution, do not delegate.

## Lifecycle

- Spawn workers for implementation.
- Spawn reviewers for validation.
- Wait only when their result is on the critical path.
- Close finished agents after integrating their output.

## Fallback

If delegation is not available, execute directly in the main thread and tell the user that the workflow is downgraded.
