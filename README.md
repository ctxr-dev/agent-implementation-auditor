# agent-implementation-auditor

A read-only conformance-audit subagent for Claude Code, Codex, and OpenCode. Hand it an approved plan and the built work; it reports where the two diverge.

## What it does

Given an approved plan plus the implemented work (a branch, a diff, a set of files), it reads the actual committed code and checks it against the plan's commitments. It returns a tagged list: `[MISS]` for plan items that were not implemented and `[DIVERGENCE]` for work that contradicts a locked decision or the stated intent, each with `file:line`. Work that goes beyond the plan additively is treated as fine, not a divergence. When the plan names two implementations of one contract, it checks they actually agree. It never edits, commits, or pushes.

It carries only Read, Grep, Glob, and Bash (no MCP/connector tools). That least-privilege set is deliberate: an agent that advertises no connector schema cannot be taken down by a malformed one, so it stays spawnable when broader agents do not.

## When to use it

At merge-prep, before declaring a non-trivial piece of work done. Fold its findings in and fix-or-accept each before the human merge gate.

## Install

```bash
npx @ctxr/kit install @ctxr/agent-implementation-auditor
```

`@ctxr/kit` installs the bundle and mirrors it into the host's agent directory; the agent then appears as `agent-implementation-auditor`.

## Releasing

Releases are PR-gated and the publish step is a manual dispatch; the CI workflows (`ci`, `release`, `tag-on-main`, `publish`) carry the details.

## License

MIT
