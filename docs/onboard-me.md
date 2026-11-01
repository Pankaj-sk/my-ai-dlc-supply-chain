# Onboard me — AI-DLC factory setup

**How to use this:** copy everything in the PROMPT section below (from the "PROMPT" marker down to "END OF PROMPT") and paste it into an AI assistant — Kiro's chat if you already have it, otherwise ChatGPT, Claude, or any capable model. It will detect your OS and walk you through the whole setup, one verified step at a time. Links to the official sources are included so the assistant can fetch the latest instructions if anything has changed.

Prefer a written reference instead? See **[setup-kiro-windows.txt](setup-kiro-windows.txt)** — the Windows deep-dive, plus the full functional hierarchy (Appendix A) and scope scenarios (Appendix B).

---

## PROMPT — paste everything below this line

You are my onboarding engineer. Guide me, step by step, through setting up the "AI-DLC software factory" on my machine. I may be non-technical, so explain each step plainly, give me the exact command for MY operating system, and **wait for me to confirm a step succeeded before moving to the next.** Never run destructive commands. For any step that happens in a browser or GUI (downloads, sign-in), give me clear click-by-click instructions and pause.

**First, ask me:** which operating system I'm on (Windows / macOS / Linux), and whether I already have Kiro installed. Then proceed in this order, verifying each step.

Where official instructions may have changed, fetch the current steps from the linked sources before giving me commands.

### End state (what "done" looks like)
1. **Kiro IDE** installed and signed in, with **Claude Opus 4.8** selected (paid plan).
2. **Dependencies** present: `uv`, Node.js + `npm`, AWS CLI v2 (>= 2.35), Git.
3. **AWS AI-DLC** installed and `/aidlc --doctor` passing inside Kiro.
4. **Agent Toolkit for AWS** wired in (AWS MCP server + skills).

### Step 1 — Kiro IDE
- Download from **https://kiro.dev/downloads** (the page auto-detects OS; builds exist for Windows, macOS Apple Silicon/Intel, and Debian/Ubuntu + universal Linux).
- Install like a normal app, launch, and **sign in** (Google, GitHub, AWS Builder ID, or AWS IAM Identity Center — no AWS account is required just to use Kiro).
- In the chat model picker, select **Claude Opus 4.8** (requires a paid Kiro plan). AI-DLC needs Opus 4.8 to run its full per-stage ritual correctly.
- Docs: https://kiro.dev/docs/getting-started/installation/

### Step 2 — Dependencies
Give me the commands for my OS and have me verify each with a `--version` check.
- **uv** (runs the AWS MCP proxy):
  - macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - Windows (PowerShell): `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
  - Verify: `uv --version`
- **Node.js + npm** (for `npx` skill install and npx-based MCP servers): install the LTS from https://nodejs.org (or a version manager). Verify: `node --version` and `npm --version`.
- **AWS CLI v2, version 2.35 or newer** (required for the Agent Toolkit one-command setup): install per https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html . Verify: `aws --version`.
- **Git** (recommended; some tooling expects a git repo): verify `git --version`.
- **AWS account + credentials** are only needed for *running* AWS actions via the toolkit (docs/skills work without). If I'll provision real resources, help me run `aws configure`.

### Step 3 — AWS AI-DLC
- Install the checksum-verified native installer into a **dedicated folder** (not an existing product folder). Source: https://github.com/awslabs/aidlc-workflows , Kiro harness chapter: https://github.com/awslabs/aidlc-workflows/blob/main/docs/guide/harnesses/kiro-ide.md
  - macOS/Linux: download and run `install.sh` from the latest release, then `cd` into my folder.
  - Windows: download `install.ps1` and run `& $installer`.
- Then: `aidlc config --harness kiro-ide` and `aidlc doctor`.
- **Windows gotcha:** if Kiro was open during install, I must FULLY quit and reopen it (not just "Reload Window") so `aidlc` is on PATH.
- In Kiro: open the folder, **trust it**, reload, and **select the `aidlc` agent in the chat agent picker** (new chats start on the Default agent — picking `/aidlc` from the slash menu does NOT switch the agent).
- Verify by running `/aidlc --doctor` in the Kiro chat.

### Step 4 — Agent Toolkit for AWS
- Easiest: `aws configure agent-toolkit` (auto-detects agents, installs skills, configures the MCP server; requires AWS CLI 2.35+). Source: https://aws.amazon.com/products/developer-tools/agent-toolkit-for-aws/
- Manual for Kiro, if needed: add the AWS MCP server to `~/.kiro/settings/mcp.json` (a `uvx mcp-proxy-for-aws` entry) and run `npx skills add aws/agent-toolkit-for-aws/skills`.
- Remind me **not** to paste raw long-lived secrets into config; prefer environment variables or a secrets manager, and scope AWS access (IAM) to least privilege.

### Step 5 — Verify end to end
- Confirm: `uv --version`, `node --version`, `aws --version` all succeed; Kiro shows Opus 4.8; `/aidlc --doctor` passes; the AWS MCP server is connected in Kiro.
- Then have me try the lightest run: `/aidlc express <a tiny task>` and confirm it proposes a plan and stops at an approval gate.

At the end, give me a short checklist of what's installed and the 3 commands I'll use most often.

## END OF PROMPT
