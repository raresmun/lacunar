# Lacunar

> The block vocabulary for AI agents.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Building in Public](https://img.shields.io/badge/Status-🚧%20Building%20in%20Public-orange)]()
[![AG-UI Compatible](https://img.shields.io/badge/AG--UI-Compatible-blueviolet)]()
[![MCP Ready](https://img.shields.io/badge/MCP-Ready-blue)]()

**Rails for the agentic era.**

---

## The Problem

Every AI coding tool today — Cursor, Claude Code, Copilot — generates structurally inconsistent UIs. Not because the models are dumb. Because they're assembling from raw primitives with no semantic guidance.

Look at the current stack:

```
Agent decides what to build     ← LangGraph, CrewAI, AutoGen
          ↓
Agent writes the actual code    ← ??? nobody owns this layer
          ↓
App runs in production          ← Vercel, Supabase, AWS
```

That middle layer — the vocabulary agents use to compose real, production apps — is completely unaddressed. Every agent today either hallucinates Tailwind classes, generates inconsistent components, or produces code that technically works but is structurally a mess.

This cannot be solved by a smarter model. It requires a better abstraction layer.

**Lacunar is that layer.**

---

## What is Lacunar?

Lacunar is an AI-native block library — a vocabulary of large, semantic, production-ready blocks designed for agent consumption.

Instead of assembling interfaces from raw atoms, agents compose them from **blocks** — pre-built, self-describing units sized for *one agent decision*. Each block knows what it is, what it does, and how it combines with others.

```tsx
// Not a component. A block.
<AuthFlow.EmailPassword
  onSuccess={handleLogin}
  providers={["google", "github"]}
/>

// Not a layout. A block.
<Dashboard.Layout
  sidebar="navigation"
  header="user-context"
  intent="admin-panel"
/>

// Not a form. A block.
<CRUD.CreateForm
  entity="product"
  fields={["name", "price", "inventory"]}
  onSubmit={handleCreate}
/>
```

---

## Why "Lacunar"?

From Latin *lacuna* — a gap, a pit, a hollow.

In architecture, a **lacunar** (also called a coffered ceiling) is a ceiling built entirely from a grid of composed, repeating panels. Each panel is structurally complete on its own. Together they form the whole.

Think of the Pantheon. That geometric ceiling grid — every coffer is a self-contained unit, every coffer is identical in purpose but unique in position, and together they create something greater than any single piece.

That's exactly what this framework is. Each block (`<AuthFlow.EmailPassword />`, `<Dashboard.Layout />`, `<CRUD.DataTable />`) is one coffer. The app the agent builds is the ceiling.

---

## Blocks vs. Components

A component is a **technical unit**. A block is a **semantic unit**.

Components are designed for human developers who understand context. Blocks are designed for agents that need to make one clear decision per unit of UI.

The difference is in the manifest:

```json
// shadcn registry — designed for humans
{
  "name": "card",
  "type": "components:ui",
  "files": ["ui/card.tsx"]
}

// Lacunar manifest — designed for agents
{
  "name": "Card.Preview",
  "intent": "display a single entity with summary info and actions",
  "agent_description": "Use when showing one item from a collection with quick action buttons. Combines thumbnail, title, metadata and 1–3 action buttons.",
  "composable_with": ["Dashboard.Layout", "CRUD.DataTable"],
  "not_for": "forms, authentication, data entry",
  "sizing": "one agent decision",
  "props": {
    "variant": "product | user | content | custom",
    "actions": "array of action strings the agent wants available"
  }
}
```

The `intent`, `agent_description`, `composable_with`, and `not_for` fields don't exist in any current component registry. That's the actual innovation. Blocks expose **intent** rather than dozens of boolean props — the agent describes what it needs, the block handles the how.

---

## Why Blocks Reduce Hallucinations

LLMs are excellent at **selection and configuration**. They are poor at **synthesis from primitives**.

When an agent builds UI from raw Tailwind and JSX, it's doing exactly what LLMs are worst at — generating hundreds of interdependent micro-decisions from scratch. Every className, every prop, every nesting decision is a chance to hallucinate something subtly wrong.

```jsx
// What agents do today — hundreds of micro-decisions, each one a hallucination risk
<div className="flex items-center justify-between p-4 border 
  rounded-lg shadow-sm hover:shadow-md transition-shadow 
  duration-200 bg-white dark:bg-zinc-900 ...">
  ...47 more lines of fragile, inconsistent primitives...
</div>

// What agents do with Lacunar — one decision, zero synthesis
<Card.Preview variant="product" actions={["buy", "save"]} />
```

Blocks collapse hundreds of micro-decisions into one semantic selection. The agent stops hallucinating class names and starts making architectural choices — which is what it's actually good at.

There's no peer-reviewed number yet. But the mechanism is clear: **fewer decisions = fewer failure points**.

---

## Built for Agents, Readable by Humans

Lacunar is designed to be consumed by agents — but the output is dramatically easier for humans to review.

When a human reviews AI-generated code today, they're reading hundreds of lines of raw JSX. They have to mentally reconstruct what it does. Bugs hide in prop combinations, in className conflicts, in subtle structural mistakes.

When a human reviews Lacunar-generated code, they read:

```tsx
<AuthFlow.EmailPassword onSuccess={handleLogin} providers={["google", "github"]} />
<Dashboard.Layout sidebar="navigation" intent="admin-panel" />
<CRUD.DataTable entity="product" columns={["name", "price", "stock"]} />
```

Three lines. The intent is self-evident. The structure is auditable in seconds.

**Agents write it. Humans can actually read it.** That's not a side effect — it's a core design principle.

---

## How It Works

No retraining. No fine-tuning. No magic.

LLMs already understand semantics. When Claude or GPT sees `<AuthFlow.EmailPassword />` it instantly knows what that is. The semantic naming does the work.

The agent flow looks like this:

```
MCP context / system prompt:
"Available blocks:
 - AuthFlow.EmailPassword — handles email+password auth with validation
 - AuthFlow.OAuth — handles social login, accepts providers[]
 - CRUD.CreateForm — generates create form for any entity
 ..."

Agent decides:
"I need a login page → use AuthFlow.EmailPassword"
"I need to show products → use CRUD.DataTable with entity='product'"

Agent outputs:
<AuthFlow.EmailPassword onSuccess={handleLogin} />
<CRUD.DataTable entity="product" columns={["name", "price", "stock"]} />
```

The agent never synthesizes anything from scratch. It selects and configures — which is exactly what LLMs are good at.

Blocks are delivered to agents via a **Lacunar MCP server** that exposes `search_blocks`, `get_block_docs`, and `list_blocks_by_category` tools. Any MCP-compatible agent — Claude Code, Cursor, Copilot — can discover and use blocks automatically without special integration.

---

## Protocol Compatibility

Lacunar is built *on top of* the emerging agentic UI protocol layer, not instead of it.

| Protocol | Role | Lacunar's relationship |
|---|---|---|
| **AG-UI** | Agent ↔ frontend communication | Lacunar blocks emit AG-UI events |
| **A2UI** (Google) | Declarative UI spec | Blocks expose A2UI-compatible manifests |
| **MCP Apps** | UI in Model Context Protocol | Lacunar ships as a native MCP server |

AG-UI, A2UI, and MCP Apps are the **pipes**. Lacunar is the **vocabulary** that flows through them.

---

## Stack

Built on the best available foundation:

- **Next.js 15** — App Router
- **TypeScript** — strict mode
- **Tailwind v4** — CSS variables
- **Supabase** — auth, database, storage
- **shadcn/ui** — primitive layer beneath blocks
- **Turborepo** — monorepo (framework + docs + playground)
- **AG-UI + A2UI + MCP** — agent protocol compatibility

---

## Roadmap

- [ ] `BLOCK_SPEC.md` — the open block standard
- [ ] `AuthFlow.*` blocks — email/password, OAuth, magic link
- [ ] `CRUD.*` blocks — CreateForm, DataTable, EditModal, DeleteConfirm
- [ ] `Dashboard.*` blocks — Layout, Sidebar, Header, Stats
- [ ] `Landing.*` blocks — Hero, Features, Pricing, CTA
- [ ] MCP server — `search_blocks`, `get_block_docs`, `list_blocks_by_category`
- [ ] CLI — `npx create-lacunar-app`
- [ ] Block playground
- [ ] Documentation site at [lacunar.dev](https://lacunar.dev)

---

## Why Now?

The global agentic AI market hit $7.6B in 2025 and is projected to reach $196B by 2034. But only 2% of organizations have deployed agentic AI at scale. The bottleneck isn't orchestration anymore — every team has LangGraph or CrewAI. The bottleneck is **reliable output**. Agents that build things consistently. That's the opening.

If you ship first and the DX is good, you own the category. That's how Rails happened. That's how Tailwind happened.

---

## Contributing

This is early. The block spec isn't published yet.

If the idea resonates — **star the repo** and follow the build. PRs and feedback welcome once `BLOCK_SPEC.md` lands.

---

## Built by

**Stefan Muntenas** — Founder, [Hamiltonian Lab](https://hamiltonianlab.com)
AI automation for the agentic era, based in Aarhus, Denmark.

[LinkedIn](https://www.linkedin.com/in/stefanmuntenas/) · [@raresmun](https://x.com/raresmun)

---

## License

MIT — free for everyone.

*If this resonates, star the repo and follow the build.*
