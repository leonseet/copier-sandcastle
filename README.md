# copier-sandcastle

A [copier](https://copier.readthedocs.io/) template that vendors a [sandcastle](https://github.com/mattpocock/sandcastle)-based implement loop into a repo as `.sandcastle/` — a plan → make → check → fold → finalize pipeline that drives coding agents against the repo's issue tracker (GitLab or GitHub).

## Install

From the root of the consuming repo:

```sh
uvx copier copy --trust gh:leonseet/copier-sandcastle .
```

Commit everything it creates, fill in `.sandcastle/.env`, tune `.sandcastle/implement/config/knobs.mts`, then run:

```sh
.sandcastle/node_modules/.bin/tsx .sandcastle/implement/index.mts [issue ids]
```

## Preconditions

The consuming repo must provide:

- `docs/agents/issue-tracker.md` and `docs/agents/triage-labels.md`
- Tracker labels: `ready-for-agent`, `ready-for-human`, `in-review`
- Slash-skills (see [leonseet/skills](https://github.com/leonseet/skills)):
  - `/code-review`, `/tdd`, `/resolving-merge-conflicts` — from Matt Pocock
  - `/simplify` — built into Claude
  - `/writing-commit`, `/explainer-html` — others
- Node.js 22.x, `pnpm`, `uv`/`uvx`, and Docker on the host

## Updates

```sh
uvx copier update --trust --vcs-ref=HEAD -a .sandcastle/.copier-answers.yml
```

Resolve any conflict markers and commit. If `.sandcastle/package.json` changed, re-run `pnpm -C .sandcastle install`. Seed-once files (`standards/`, `pnpm-lock.yaml`, `docs/agents/sandbox-rules.md`) never receive updates.
