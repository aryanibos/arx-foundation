# Ask-Gates

The gate mechanics of this skill. A gate is a mandatory stop: the agent asks, the user decides, the decision is recorded. No gate is ever skipped, and no decision ever passes silently.

## The gate pattern

Every gate follows the same five steps:

1. **Stop.** The agent stops work. It does not proceed, scaffold, or write code until the gate resolves.
2. **Recommend.** The agent presents curated recommendations suited to *this* project, each grounded in a constraint from the user's source document (see the source-doc rule below), with the one-line trade-off for each.
3. **Challenge.** The agent presents one risk or one serious alternative and asks the user to weigh it. A recommendation without a challenge is not finished.
4. **Ask.** The agent asks the question and waits. Every question offers the recommendations **plus a "choose your own" option**. The user may accept a recommendation, pick their own, or change direction.
5. **Record.** The decision goes into the decision log (see `decision-log.md`): choice, reason, rejected alternatives, date.

A gate the user waves through without thinking has failed its purpose. If that happens, the agent should notice and say so — then re-present the trade-off once, briefly, before moving on.

## The source-doc rule

Recommendations are grounded in a source document the user designates. The document may be a BA doc, a PRD, or a TSD. The agent does not know in advance which one it will be, and must never assume.

1. **The user tells the agent first** which document to read. Ask, if it was not named: "Which document should I read as the source — the BA doc, the PRD, or the TSD — and where is it?"
2. **Read it before recommending anything.** Extract the constraints that matter for structure: scale and volume, team size and roles, timeline, must-have features, integrations, compliance, and non-functional requirements.
3. **Ground every recommendation in it.** Each recommendation names the constraint it follows from.
4. **If the document is silent on a constraint the decision needs, say so and ask.** Do not guess the missing constraint and do not fill it with a generic default.

## Stack selection gate (foundation)

Before any scaffolding, the agent stops and asks which stacks to use — one decision at a time, frontend first, then backend, then database and the other load-bearing choices. For each, it presents recommendations from the tables below (adapted to the source doc) plus a "choose your own" option. It does not proceed until the user answers.

The tables are starting points, not prescriptions. The agent's recommendations must follow from the source doc's constraints; if the tables do not fit the project, the agent says so and recommends from outside them, with the reason.

### Frontend

| Option | Fits when | Trade-off |
|---|---|---|
| React + Vite | Dashboards and apps, team knows React | Largest ecosystem; no SSR/SEO out of the box |
| Next.js (React) | Public pages needing SEO/SSR, or full-stack React | SEO and SSR built in; heavier setup and opinions |
| SolidJS | Small, fast SPAs; reactivity-first | Tiny and fast; smaller ecosystem |
| Vue + Vite / Nuxt | Teams that prefer Vue; Nuxt when SSR/SEO matters | Gentle learning curve; smaller hiring pool than React |
| Svelte / SvelteKit | Small teams wanting less boilerplate | Very productive; smaller ecosystem |

### Backend

| Option | Fits when | Trade-off |
|---|---|---|
| Hono (Bun/Node) | Lightweight APIs, edge-friendly | Fast and small; fewer batteries than full frameworks |
| Elysia (Bun) | Type-safe APIs on Bun | End-to-end types; Bun-only |
| FastAPI (Python) | Python teams, data/ML-adjacent APIs | Excellent docs and validation; Python runtime |
| NestJS (Node) | Large teams wanting structure and DI | Opinionated and scalable; heavyweight for small APIs |
| Express (Node) | Maximum familiarity, minimal opinions | Ubiquitous; you build the structure yourself |
| Go (stdlib / Gin / Fiber) | High-throughput services, small binaries | Fast and simple to deploy; more code for the same API |

### Database and storage

| Option | Fits when | Trade-off |
|---|---|---|
| PostgreSQL | The default for relational data | Rock solid; needs hosting/ops |
| MySQL | Teams and hosting already on MySQL | Familiar; weaker advanced types than Postgres |
| SQLite | Single-node apps, local-first, prototypes | Zero ops; no concurrent multi-writer scaling |
| Redis | Cache, queues, rate limits, sessions | Fast; another service to run |
| S3-compatible storage | Files, uploads, assets | Durable and cheap; needs signed-URL care |

## Structural gates (throughout)

Every structural decision goes through the same five-step pattern: database, architecture style, module boundaries, auth model, deployment target, folder structure, and anything else that is hard to reverse. A decision made without a gate is not a decision; it is an assumption, tagged `[ASSUMED Q-NN]` and surfaced at the next phase gate.

Architecture style deserves the hardest grill: lay out the realistic options against *this* project's NFRs and team size, give a recommendation with the trade-off, and get explicit buy-in. Do not accept the first style the user names.

## Phase gates (end of each phase)

At the end of every lifecycle phase, the agent stops and asks: **"Proceed or revise?"** It summarizes what was produced, what is still open, and the one biggest risk it sees. The user confirms, asks for revision, or changes direction. The phase is not done until the user says so.

## What a gate never does

- Never presents a recommendation without its trade-off and its challenge.
- Never lets the user answer with silence. A missing answer is a blocker, not an invitation to pick a default.
- Never records a decision the user did not make. If the user said "you decide", the agent records that the user delegated the choice, the recommendation it acted on, and the reason — and the user can still overturn it at the next phase gate.
