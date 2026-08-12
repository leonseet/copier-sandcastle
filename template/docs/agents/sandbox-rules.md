# Sandbox rules

- Never run any `git worktree` subcommand.
- To inspect or run another branch's tree: `git archive <branch> | tar -x -C "$(mktemp -d)"`
- For merge experiments or other real-repo operations: `git clone --shared . "$(mktemp -d)" -b <branch>`
- Clean up scratch directories with `rm -rf`.
- Never wipe shared stores. Destructive ops (DB reset/TRUNCATE/DROP, dropping
  vector-store collections, deleting object-store prefixes) may only hit
  localhost or stores you created for this run. Before running one, print the
  resolved connection settings and stop if any points at a shared host —
  overriding one variable after sourcing an env file leaves the rest pointing
  at shared hosts.
