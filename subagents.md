# Subagents

[`subagents/senior-code-reviewer.md`](./subagents/senior-code-reviewer.md) is the
source for our code review agent. It uses the parent session's model.

[Claude Code loads Markdown agent files](https://code.claude.com/docs/en/sub-agents).
[Codex can import Claude Code subagents](https://learn.chatgpt.com/docs/import).

## Claude Code

From this repo's root:

```sh
mkdir -p "$HOME/.claude/agents"
ln -sfn "$PWD/subagents/senior-code-reviewer.md" "$HOME/.claude/agents/senior-code-reviewer.md"
```

This replaces any file at the target path. Back it up first if needed.
The link keeps Claude Code in step with edits here.

## Codex

First create the Claude Code link above.
You do not need to run Claude Code.

1. Start a local Codex CLI session with `codex --no-daemon`.
2. Type `/import`.
3. Choose Claude Code, then select the subagent items you want to import.
4. Review the imported `senior-code-reviewer` configuration.

`/import` is unavailable during a running task, in a remote session, or when
connected to a local app-server daemon.

After editing the Markdown source here, import it again to update Codex's copy.

The desktop app also offers **Settings > Import**, with automatic updates
to keep imported work in sync with the source.

## Use it

Start a fresh session in either tool, then ask:

```text
Use the senior-code-reviewer subagent to review my changes.
```
