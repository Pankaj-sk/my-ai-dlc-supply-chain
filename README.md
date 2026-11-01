# AI-DLC Supply Chain

How we run **AWS AI-DLC** inside **Kiro** as a *product-independent software factory*. This repo is orientation material so anyone joining a project can get up to speed fast on *how we build*, independent of *what* we're building.

> **Product-agnostic by design.** Nothing here is tied to a specific product. The factory is the same whatever we build on top of it; a product enters only as an "intent".

## TL;DR
- **AI-DLC** is an open methodology from AWS: **5 phases, 33 stages, 14 agents**, with human approval gates. It's the *how we build*.
- We run it inside **Kiro IDE**. A piece of work enters as an intent: `/aidlc <describe the work>`.
- **Start light** (`/aidlc express`) and **dial up** toward `enterprise` as the work gets more serious.

## Anatomy of one run (what you feed it)

The unit of work you hand the factory is an **intent**: a single piece of work in plain language, typed in the Kiro chat with the `aidlc` agent selected:

```
/aidlc <profile> <describe the work>
```

One complete instruction looks like:

```
/aidlc express Add IP-based rate limiting to the public REST API
```

`express` is the *scope*; the rest is the *intent*. AI-DLC proposes a stage plan, asks a few clarifying questions, and stops at an approval gate after each stage — you reply to move it forward.

**The intent changes every run; these dials right-size it:**

| Dial | How to set | Options | Controls |
|---|---|---|---|
| Scope / profile | `/aidlc <profile>` | poc · express · bugfix · refactor · security-patch · infra · classic · mvp · feature · enterprise | how many of the 33 stages fire |
| Depth | `--depth` | minimal · standard · comprehensive | detail per stage |
| Test strategy | `--test-strategy` | minimal · standard · comprehensive | how many tests |
| Review | `--review` | none · advisory · adversarial | how hard the reviewer pushes back |
| Ceremony | `--sensors` `--learnings` `--plan-approval` … | on · off | individual governance machinery |

Don't want to choose? Describe the work and AI-DLC proposes a profile. **Mental model:** the *intent* is the variable (new every run); the *dials* are constants you occasionally adjust — same factory, same 14 agents, same 33 stages.

### You don't always run the whole thing

The finest-grained unit is a **single stage**, which carries its own lead agent, reviewer, and tools:

```
/aidlc-code-generation        ==  /aidlc --stage code-generation --single
```

A **stage-runner** runs that one stage in isolation, archives any existing files first, and **never advances your main workflow**. There are 30 of them (one per runnable stage).

| You want to run… | How | Reruns the full pipeline? |
|---|---|---|
| The whole lifecycle | `/aidlc <desc>` or a scope-runner | only your scope's stages |
| A right-sized subset | a **scope** (`/aidlc --scope poc`) or `/aidlc-compose` | no |
| **One stage** (its agent + reviewer + tools) | `/aidlc-<stage>` | no — workflow untouched |
| One agent | run the **stage it leads** (agents aren't started on their own) | no |
| One tool call | plain Kiro / MCP chat, outside AI-DLC | no |

Inside a live run you can also **jump** (`/aidlc --stage <name>` or `--phase <name>`), and in Construction a change to one Unit redoes that Unit's stage only. The full pipeline runs **only** when you ask for a full-lifecycle scope.

## The factory at a glance

[![The AI-DLC factory — a product-independent pipeline with the AWS supplier layer](diagrams/02-factory.svg)](diagrams/02-factory.svg)

*Click the diagram to open it full size.* Only the two orange ends are product-specific. Everything between is the reusable factory. Three cross-cutting layers run underneath: **Control dials** (right-size each run), **Governance** (gates, guards, audit), and **Memory** (team knowledge + a learning loop). An external **supplier** layer (MCP servers such as the Agent Toolkit for AWS) gives the agents real, governed capability.

## The agents & stages, in detail

The same pipeline, exploded: all **14 agent personas** (11 domain experts + 2 reviewers + 1 composer) and the **33 stages**, colour-coded by the agent that leads each, with its support agents, run condition, and reviewer coverage.

[![The 14 AI-DLC agents and the 33 stages, colour-coded by lead agent](diagrams/03-agents-and-stages.svg)](diagrams/03-agents-and-stages.svg)

*Click the diagram to open it full size.* A single **conductor** orchestrates all delegation — agents never call each other. The **architect** is the busiest (10 stages); **compliance** and **devsecops** only ever support, never lead. Phase 3 (Construction) repeats its per-unit stages for every Unit of work.

## The AWS supplier: Agent Toolkit for AWS

AI-DLC decides *what* to build and *in what order*; the **Agent Toolkit for AWS** makes the "build it on AWS" steps competent, current, and governed. It's a supplier that plugs into the factory — it gives the agents (notably the `aws-platform` agent) real AWS capability, AWS-authored knowledge, and guardrails.

[![Agent Toolkit for AWS — the four parts and how they plug into your agent and AWS](diagrams/04-aws-toolkit.svg)](diagrams/04-aws-toolkit.svg)

*Click the diagram to open it full size.* Four parts: **AWS MCP Server** (one authenticated endpoint to 300+ services, sandboxed scripts, live docs), **Agent Skills** (on-demand task packages), **Agent Plugins** (one-command install for Kiro and others), and **Rule Files** (persistent "work with AWS this way" instructions). Governed by **IAM context keys** (scope agent actions, e.g. read-only in prod) and **CloudWatch / CloudTrail** auditing. Set up with one command: `aws configure agent-toolkit` (AWS CLI 2.35+). See the [product page](https://aws.amazon.com/products/developer-tools/agent-toolkit-for-aws/).

## Set it up

See **[docs/setup-kiro-windows.txt](docs/setup-kiro-windows.txt)** — a Windows setup guide plus the full functional hierarchy (Appendix A) and scope scenarios (Appendix B).

**Prerequisites:** Kiro IDE >= 1.1.70, Claude Opus 4.8 (paid Kiro plan).

## Suggested reading order
1. This README — the concept
2. `diagrams/02-factory.svg` — the big picture
3. `diagrams/03-agents-and-stages.svg` — the detail
4. `diagrams/04-aws-toolkit.svg` — the AWS supplier
5. `docs/setup-kiro-windows.txt` — install + reference

---
*Sources: AWS AI-DLC docs (awslabs/aidlc-workflows) and the Agent Toolkit for AWS product page. Content summarized for compliance with licensing restrictions.*
