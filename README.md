# AI-DLC Supply Chain

How we run **AWS AI-DLC** inside **Kiro** as a *product-independent software factory*. This repo is orientation material so anyone joining a project can get up to speed fast on *how we build*, independent of *what* we're building.

> **Product-agnostic by design.** Nothing here is tied to a specific product. The factory is the same whatever we build on top of it; a product enters only as an "intent".

## TL;DR
- **AI-DLC** is an open methodology from AWS: **5 phases, 33 stages, 14 agents**, with human approval gates. It's the *how we build*.
- We run it inside **Kiro IDE**. A piece of work enters as an intent: `/aidlc <describe the work>`.
- **Start light** (`/aidlc express`) and **dial up** toward `enterprise` as the work gets more serious.

## The factory at a glance

[![The AI-DLC factory — a product-independent pipeline with the AWS supplier layer](diagrams/02-factory.svg)](diagrams/02-factory.svg)

*Click the diagram to open it full size.* Only the two orange ends are product-specific. Everything between is the reusable factory. Three cross-cutting layers run underneath: **Control dials** (right-size each run), **Governance** (gates, guards, audit), and **Memory** (team knowledge + a learning loop). An external **supplier** layer (MCP servers such as the Agent Toolkit for AWS) gives the agents real, governed capability.

## More diagrams

| Diagram | What it shows |
|---|---|
| [01-scenario-grid.svg](diagrams/01-scenario-grid.svg) | How each **scope** lights up the pipeline (start light, dial up) |
| [03-agents-and-stages.svg](diagrams/03-agents-and-stages.svg) | The 14 agents and all 33 stages, in detail |

## Set it up

See **[docs/setup-kiro-windows.txt](docs/setup-kiro-windows.txt)** — a Windows setup guide plus the full functional hierarchy (Appendix A) and scope scenarios (Appendix B).

**Prerequisites:** Kiro IDE >= 1.1.70, Claude Opus 4.8 (paid Kiro plan).

## Suggested reading order
1. This README — the concept
2. `diagrams/02-factory.svg` — the big picture
3. `diagrams/01-scenario-grid.svg` — how to right-size a run
4. `diagrams/03-agents-and-stages.svg` — the detail
5. `docs/setup-kiro-windows.txt` — install + reference

---
*Sources: AWS AI-DLC docs (awslabs/aidlc-workflows) and the Agent Toolkit for AWS product page. Content summarized for compliance with licensing restrictions.*
