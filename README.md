# copier-sandcastle

A [copier](https://copier.readthedocs.io/) template that vendors my
[sandcastle](https://github.com/mattpocock/sandcastle)-based implement loop
into a repo as `.sandcastle/`: a plan → make → check → fold → finalize pipeline
that drives coding agents against the repo's issue tracker (GitLab or GitHub).

Personal tool that happens to be public — docs are minimal and assume my setup.

## Install

One command, into the root of the consuming repo:

```sh
uvx copier copy --trust gh:leonseet/copier-sandcastle .
```

`--trust` is required: the template's tasks scaffold `.sandcastle/.env` from
`.env.example` and run `pnpm -C .sandcastle install`. Commit everything it
creates (including `.sandcastle/.copier-answers.yml` — updates need it).

Then fill in `.sandcastle/.env` (tracker token; optionally explainer hosting)
and tune `.sandcastle/implement/config/knobs.mts` (verify command, sandbox
setup, worktree copies). Run the loop from the repo root:

```sh
.sandcastle/node_modules/.bin/tsx .sandcastle/implement/index.mts [issue ids]
```

## Preconditions

Not shipped; the loop expects the consuming repo to provide:

- `docs/agents/issue-tracker.md` — the tracker contract prompts `cat` at
  runtime (which CLI, conventions). `docs/agents/triage-labels.md` — the label
  vocabulary. (`docs/agents/sandbox-rules.md` IS shipped, seed-once.)
- Triage labels existing on the tracker: `ready-for-agent`, `ready-for-human`,
  `in-review`.
- The slash-skills the prompts invoke, installed in the consuming repo:
  `/code-review`, `/tdd`, `/writing-commit`, `/simplify`, `/explainer-html`,
  `/resolving-merge-conflicts`.
- `pnpm` and `uv`/`uvx` on the host; Docker for sandboxes.

## Updates

One-way: consumers pull, tuning stays local thanks to copier's 3-way merge.

```sh
uvx copier update --trust --vcs-ref=HEAD
```

Conflicts appear as inline markers; resolve and commit. If the update changed
`.sandcastle/package.json`, re-run `pnpm -C .sandcastle install`. Seed-once
files (`standards/`, `pnpm-lock.yaml`, `docs/agents/sandbox-rules.md`) never
receive template changes after first copy.

Upstreaming is manual: edit this template repo directly.
