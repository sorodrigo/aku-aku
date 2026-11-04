# Subagents

[`subagents/senior-code-reviewer.md`](./subagents/senior-code-reviewer.md) is the
source for our code review agent. It uses the parent session's model.

[Claude Code loads Markdown agent files](https://code.claude.com/docs/en/sub-agents).
[OpenCode loads Markdown agent files](https://opencode.ai/v2/docs/agents) too.
[Codex can import Claude Code subagents](https://learn.chatgpt.com/docs/import).

## Claude Code

From this repo's root:

```sh
mkdir -p "$HOME/.claude/agents"
ln -sfn "$PWD/subagents/senior-code-reviewer.md" "$HOME/.claude/agents/senior-code-reviewer.md"
```

This replaces any file at the target path. Back it up first if needed.
The link keeps Claude Code in step with edits here.

## OpenCode

From this repo's root:

```sh
mkdir -p "$HOME/.config/opencode/agents"
ln -sfn "$PWD/subagents/senior-code-reviewer.md" "$HOME/.config/opencode/agents/senior-code-reviewer.md"
```

The file sets `mode: subagent`, which OpenCode needs to list it as a subagent. It
sets no model, so it uses the parent session's model, as `inherit` does in Claude
Code.

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

Start a fresh session in any of these tools, then ask:

```text
Use the senior-code-reviewer subagent to review my changes.
```
