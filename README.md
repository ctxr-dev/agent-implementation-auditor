# agent-implementation-auditor

A read-only conformance-audit subagent for Claude Code, Codex, and OpenCode. Hand it an approved plan and the built work; it reports where the two diverge.

## What it does

Given an approved plan plus the implemented work (a branch, a diff, a set of files), it reads the actual committed code and checks it against the plan's commitments. It returns a tagged list: `[MISS]` for plan items that were not implemented and `[DIVERGENCE]` for work that contradicts a locked decision or the stated intent, each with `file:line`. Work that goes beyond the plan additively is treated as fine, not a divergence. When the plan names two implementations of one contract, it checks they actually agree. It never edits, commits, or pushes.

Its instructions are tool-agnostic: it uses whatever capabilities the environment exposes (the built-in file, search, and shell tools, plus any servers the project provides) and treats them as best-effort. If a capability is missing, unhealthy, or errors, it notes that and falls back rather than halting, so it works across very different setups and never stalls a fan-out because one tool is down. It stays read-only by rule, even where write-capable tools are present.

## When to use it

At merge-prep, before declaring a non-trivial piece of work done. Fold its findings in and fix-or-accept each before the human merge gate.

## Install

```bash
npx @ctxr/kit@latest install @ctxr/agent-implementation-auditor
```

`@ctxr/kit` installs the bundle and mirrors it into the host's agent directory; the agent then appears as `agent-implementation-auditor`.

## Releasing

Releases are PR-gated and the publish step is a manual dispatch; the CI workflows (`ci`, `release`, `tag-on-main`, `publish`) carry the details.

## License

MIT
