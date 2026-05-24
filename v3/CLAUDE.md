# V3 Module Development

This directory contains the V3 monorepo packages. Root CLAUDE.md rules apply here.

## Build & Test

```bash
# From v3/@claude-flow/<package>
npm install && npm run build && npm test
```

## Packages

| Package | Path | Purpose |
|---------|------|---------|
| `@claude-flow/cli` | `@claude-flow/cli/` | CLI entry point (26 commands, 140+ subcommands) |
| `@claude-flow/guidance` | `@claude-flow/guidance/` | Governance control plane (compile, enforce, prove, evolve) |
| `@claude-flow/hooks` | `@claude-flow/hooks/` | 17 hooks + 12 background workers |
| `@claude-flow/memory` | `@claude-flow/memory/` | AgentDB + HNSW vector search |
| `@claude-flow/shared` | `@claude-flow/shared/` | Shared types and utilities |
| `@claude-flow/security` | `@claude-flow/security/` | Input validation, path security, CVE remediation |

## Code Quality

- Files under 500 lines
- No hardcoded secrets
- Input validation at system boundaries
- Typed interfaces for all public APIs
- TDD London School (mock-first) preferred
- Event sourcing for state changes

## Performance Targets

| Metric | Target | Status |
|--------|--------|--------|
| HNSW Search | 150x-12,500x faster | Implemented |
| Memory Reduction | 50-75% (Int8 quantization) | Implemented |
| MCP Response | <100ms | Achieved |
| CLI Startup | <500ms | Achieved |
| Flash Attention | 2.49x-7.47x speedup | In progress |


---

## REUSE-FIRST DISCOVERY GATE

Before writing any new code, workflow, automation, agent, integration, dashboard, scraper, generator, prompt system, SaaS component, or repo update, you MUST complete a reuse-first discovery pass and present the result to the user. **Do not start building until the gate has been cleared.**

### When this rule fires

Applies to anything that could plausibly already exist as a maintained open-source or commercial component:
- Custom features, tools, scripts, CLIs
- Workflow / automation designs (n8n, GitHub Actions, cron, Zapier)
- AI agent designs (sub-agents, skills, orchestrators, eval harnesses)
- SaaS / API integration code
- Dashboards, scrapers, generators, parsers, RAG pipelines
- Prompt systems, prompt libraries, prompt-eval scaffolding
- Repo scaffolding, starter templates, dev tooling

Does NOT apply to: trivial one-line edits, bug fixes in existing code, config tweaks, or work explicitly scoped by the user as "from scratch / no dependencies."

### Required search scope

Before writing code, search at minimum (skip a source only if you state out loud why it's not relevant):

- **GitHub** — repo search, topic search, awesome-lists, gists
- **Hugging Face** — models, datasets, spaces (for any ML/AI work)
- **npm** (Node) / **PyPI** (Python) / **crates.io** (Rust) — packages
- **Docker Hub** — pre-built images
- **n8n template library** — for workflow automation
- **MCP server registries** — modelcontextprotocol.io, awesome-mcp lists, Claude Code marketplace
- **Claude / ClawHub skill or agent resources** — if present in this repo or its referenced docs
- **Official vendor docs** — when integrating a specific API/service
- **Awesome-\* lists and starter kits** for the relevant domain

If a search dimension is impossible in the current environment (no web access, sandboxed, MCP server down), say so explicitly. Do not silently skip.

### Required output before any code is written

Produce these five fields, in this order, then STOP. Do not write code until the user has reviewed and approved them.

1. **Interpreted goal** — one sentence: what the user actually wants accomplished. Restate so they can correct any misreads.
2. **Contained build boundary** — what is IN scope and what is explicitly OUT of scope for this task. Prevents scope creep.
3. **Best existing options** — top 2–4 maintained candidates discovered, each with: name, link, last-updated / maintenance signal, license, what it gives you, what it does not.
4. **Build-vs-borrow recommendation** — pick exactly one: (a) use option X as-is, (b) fork / wrap option X, (c) compose multiple options, or (d) build from scratch — and **why**. Building from scratch requires explicit justification (no maintained option, licensing blocker, performance ceiling, etc.).
5. **One concrete next step** — the single smallest action that moves the chosen path forward. Not a plan, not a phased roadmap. One step.

### Trigger

The user can invoke this gate at any time by typing any of: `/reuse-first`, `/repo-scan`, `/dont-reinvent`. The agent should also self-trigger the gate whenever it detects it is about to begin a buildable task that fits the scope above.

### Failure mode to avoid

Do not produce vague reuse summaries ("there are probably libraries for this"). Either name specific maintained projects with links, or explicitly say no maintained option was found and why a build is warranted. The whole point of the gate is to stop reinventing — vague is the same as skipping.
