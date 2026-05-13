# Contributing to fashion-skill

A personal AI stylist skill for Claude Code — wardrobe inventory, outfit recommendations, pre-purchase checks, and in-store mode. Contributions welcome.

## Before you start

- Check [open issues](https://github.com/googlarz/fashion-skill/issues) and [discussions](https://github.com/googlarz/fashion-skill/discussions)
- New modes or major workflow changes should be discussed first

## What to contribute

- **Workflow improvements** — better outfit logic, smarter wardrobe analysis
- **New modes** — e.g. seasonal planning, packing lists, capsule wardrobe builder
- **Evaluation cases** — add to `evals.json` to improve recommendation quality
- **Bug reports** — describe your wardrobe setup and what recommendation went wrong

## Submitting a PR

1. Fork → branch from `main`
2. For new modes, add to `SKILL.md` following the existing mode structure
3. If adding eval cases, follow the schema in `evals.json`
4. Open the PR with an example: input → expected output → actual output before your fix
