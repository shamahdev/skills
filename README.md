# Skills

[![skills.sh](https://skills.sh/b/shamahdev/skills)](https://skills.sh/shamahdev/skills)

A curated, mix-and-match set of agent skills I actually use. Most of them are not original — they are copied, adapted, or distilled from other skill repos, notes, and workflows, then kept here so I can install the ones that fit.

Treat this as a personal shelf, not a framework. Pick one skill. Skip the rest. Fork and rewrite freely.

## Install

```bash
npx skills add shamahdev/skills
```

Install one skill:

```bash
npx skills add shamahdev/skills@call-graph
```

## Skills

### call-graph

Use when showing call graphs, execution flows, or architecture traces.

It forces a consistent plain-text output (`indented →` arrows in a `ts` block, Production and Tests as separate sections when they differ) and a thinking pipeline: draw the graph first, then write code that *is* the graph. That pipeline is Effect-shaped (`A` happy path, `E` break points, `R` requirements) even when the codebase is not Effect.

Not original. The output format and Effect graph thinking come from existing notes (`DESIGN_THINKING.md`, `ECALL_GRAPH_IN_YOUR_AGENTS.md`) packaged so agents load them on demand.

## License

MIT. Upstream ideas stay theirs; this repo only collects the versions I run.
