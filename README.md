# Lacunar

> The block vocabulary for AI agents. Rails for the agentic era.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Building in Public](https://img.shields.io/badge/Status-Building%20in%20Public-blue)]()

## What is this?

AI agents are getting better at reasoning. They are not getting better at 
assembling production UIs — because that problem cannot be solved by a 
smarter model. It requires a better abstraction layer.

**Lacunar is that layer.**

A library of large, semantic, production-ready blocks designed for agent 
consumption. Instead of agents hallucinating raw Tailwind and React from 
scratch, they pick blocks, configure them, and wire them together.
```tsx
<AuthFlow.EmailPassword onSuccess={handleLogin} />
<Dashboard.Layout sidebar="navigation" intent="admin-panel" />
<CRUD.CreateForm entity="product" fields={["name", "price"]} />
```

Named after the architectural term for a coffered ceiling — a structure 
built entirely from composed, repeating panels. Each panel is complete on 
its own. Together they form the whole.

## The Problem

Every AI coding tool today — Cursor, Claude Code, Copilot — generates 
structurally inconsistent UIs. Not because models are dumb. Because they're 
assembling from raw primitives with no semantic guidance.
```
Agent decides what to build     ← LangGraph, CrewAI etc.
          ↓
Agent writes the actual code    ← ??? nobody owns this layer
          ↓
App runs in production          ← Vercel, Supabase etc.
```

Lacunar owns the middle layer.

## How it works

Each block is a **semantic unit** — not a component, but a complete piece 
of functionality that maps to a single agent decision.

- **Intent over props** — agents describe what they need, blocks handle the how
- **LLM-readable by design** — machine-readable docs baked in
- **AG-UI + MCP compatible** — works with any agent protocol stack
- **Production-ready** — not demos, real blocks you ship

## Stack

- Next.js 15 + TypeScript (strict)
- Tailwind v4
- Supabase (auth, database, storage)
- shadcn/ui (primitive layer)
- AG-UI / A2UI / MCP Apps compatible
- Turborepo monorepo

## Roadmap

- [ ] `BLOCK_SPEC.md` — the open block standard
- [ ] `AuthFlow.*` blocks
- [ ] `CRUD.*` blocks
- [ ] `Dashboard.*` blocks
- [ ] `Landing.*` blocks
- [ ] MCP server for agent consumption
- [ ] CLI — `npx create-lacunar-app`
- [ ] Block playground
- [ ] Docs site

## Contributing

This is early. If the idea resonates, star the repo and follow the build.
PRs and feedback welcome once the block spec is published.

## Built by

**Stefan Muntenas**  
Founder, [Hamiltonian Lab](https://hamiltonianlab.com) — AI automation for 
the agentic era.  
[LinkedIn](https://www.linkedin.com/in/stefanmuntenas/) · 
[@raresmun](https://x.com/raresmun)

## License

MIT — free for everyone.
