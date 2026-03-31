---
name: acpx-session
description: Manage ACPX sessions for delegating tasks from an orchestrator agent to coding agents. Use for agent-to-coding-agent work delegation with named sessions, parallel workstreams, and status tracking.
version: "1.0.0"
---

## Install

Copy to `agents/<coder>/skills/acpx-session/SKILL.md`. Requires ACPX CLI installed and at least one coding agent configured.

# ACPX Session Skill

## Session Creation

Create a new session for a coding agent:

```
acpx <agent> sessions new
```

Named sessions for tracking:

```
acpx <agent> sessions new --name <name>
```

## Prompt Submission

Send a task to the agent:

```
acpx <agent> "task description"
```

Submit from a file:

```
acpx <agent> --file <path>
```

Pipe via stdin:

```
echo "task" | acpx <agent>
```

## Parallel Workstreams

Use named sessions to run concurrent work without collisions:

```
acpx <agent> -s frontend "implement login page"
acpx <agent> -s backend "add auth endpoint"
```

Each named session maintains independent context and state.

## Exec Mode

For one-shot, stateless tasks that don't need session persistence:

```
acpx <agent> exec "one-shot task"
```

No session is created or retained after completion.

## Status and Cancel

Check active sessions:

```
acpx <agent> status
```

Cancel a running session:

```
acpx <agent> cancel
```

Cancel a named session:

```
acpx <agent> cancel -s <name>
```

## Integration with sessions_send

Combine ACPX with the handoff protocol:

1. Receive a `HANDOFF` message via `sessions_send`
2. Spawn an ACPX session for the target coding agent
3. Post `ACK` with the session name and ETA
4. On completion, post `DONE` with evidence back via `sessions_send`
5. If blocked, post `BLOCKED` with explicit unblock requirements

This bridges the inter-agent handoff protocol with ACPX session management.

## Supported Agents

| Agent    | Identifier |
|----------|------------|
| Codex    | codex      |
| Claude   | claude     |
| Gemini   | gemini     |
| OpenCode | opencode   |
| Pi       | pi         |
| Copilot  | copilot    |
| Cursor   | cursor     |
| Droid    | droid      |
| Kimi     | kimi       |
| Kiro     | kiro       |
| Qwen     | qwen       |
| Trae     | trae       |
