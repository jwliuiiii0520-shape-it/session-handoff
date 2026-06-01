# session-handoff

<p align="center">
  <a href="./README.md">中文</a> · English
</p>

A lightweight Codex skill for handing off context between sessions: summarize
the current work before the context gets too long, then align on understanding
before resuming in a fresh session.

## Why It Exists

Long-running collaboration with Codex creates a tradeoff:

- Stay in the old session: the context keeps growing, early constraints become
  easier to miss, and decision quality may decline.
- Start a fresh session: the context is clean, but the agent no longer knows
  what has already been done and may repeat previous work.

`session-handoff` adds a concise, reviewable handoff document between sessions.
It does not copy the full conversation, require a database, or depend on an
external service. The relevant context stays in a Markdown file inside the
current project.

## Quick Start

In the old session:

```text
$session-handoff sho-summary
```

Codex checks the current project state and creates a new handoff file:

```text
.codex/handoff/2026-06-01-1530-session-handoff-skill-design.md
```

In the new session:

```text
$session-handoff sho-read
```

Codex reads the latest handoff, verifies the actual project state, and then:

1. Explains its understanding of the completed work.
2. Lists the recommended next actions.
3. Calls out remaining risks or ambiguities.
4. Waits for your confirmation.

It will not modify files or resume execution until you explicitly approve.

## What Gets Preserved

The handoff document includes:

- What the session discussed and advanced
- The user's end goal
- Completed work and current state
- Prioritized next actions
- Confirmed decisions and constraints
- Relevant file paths
- Completed and pending verification
- Remaining blockers

A simplified example:

```markdown
# Session Handoff

Updated: 2026-06-01 15:30
Project root: /path/to/project
Session title: fix-login-redirect

## Session Summary
- Located the repeated redirect after the login callback.
- Fixed the route guard logic for expired sessions.
- Added regression coverage, but have not run the complete test suite.

## Current State
- The login callback logic has been updated.
- Focused tests pass.
- The complete test suite has not been run.

## Next Actions
1. Run the complete test suite.
2. Verify redirects for signed-out users visiting restricted pages.

## Confirmed Decisions
- Keep the existing routing structure. Do not add a state management dependency.

## Blockers
None.
```

## Two Operations

### `sho-summary`

Summarize the current session and create a new handoff document.

- Git projects: save to `.codex/handoff/` under the repository root
- Non-Git projects: save to `.codex/handoff/` under the current working directory
- Create a new file every time instead of overwriting previous records

### `sho-read`

Read a user-specified handoff document, or the latest one by filename timestamp.

The purpose of `sho-read` is to restore context and align with the user, not to
start working immediately. It verifies the filesystem and Git state first, then
waits for explicit approval.

## Installation

### Option 1: Manual Installation

Create the skill directory:

```bash
mkdir -p ~/.codex/skills/session-handoff
```

Save [`SKILL.md`](./SKILL.md) to:

```text
~/.codex/skills/session-handoff/SKILL.md
```

### Option 2: Install with Git

```bash
git clone https://github.com/jwliuiiii0520-shape-it/session-handoff.git
mkdir -p ~/.codex/skills/session-handoff
cp session-handoff/SKILL.md ~/.codex/skills/session-handoff/SKILL.md
```

## Usage Examples

Create a handoff:

```text
$session-handoff sho-summary
```

Restore context:

```text
$session-handoff sho-read
```

Natural language also works:

```text
sho-summary
Summarize the current session for a handoff.

sho-read
Read the handoff, explain your understanding, and wait for confirmation.
```

## Design Principles

- **Short but useful**: preserve what the next session needs to act correctly,
  not a transcript of the conversation.
- **Project-local**: keep handoffs with the project without depending on a
  specific computer path or third-party service.
- **Historical**: create a new handoff file every time for traceability.
- **Align before acting**: a new session never starts working immediately after
  reading a handoff.
- **Verify reality**: treat the handoff as a working summary, not ground truth.
  Check the current files and Git state before resuming.

## Good Fits

- Development tasks that span multiple Codex sessions
- Long conversations that need proactive context compression
- Research, writing, or documentation work with staged handoffs
- Projects that benefit from a readable, reviewable progress record

## What It Is Not

`session-handoff` is not a long-term memory database or a RAG system that
automatically searches every historical conversation. It is intentionally
simple: one handoff, one Markdown file, and one clear confirmation point.

## License

[MIT](./LICENSE) © 2026 pika wanwan
