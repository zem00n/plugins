---
name: subagent-adapter
description: 'Translate Cursor-oriented pstack delegation instructions to the Pi coding agent.'
---

# Pi adaptation for pstack

This install runs inside the Pi coding agent. Cursor's `Task`, `AskQuestion`, cloud-agent environments, Cursor model files, and Cursor transcript paths are not available here.

Apply these substitutions whenever a skill or playbook asks for a subagent, `Task`, `subagent_type`, a cloud worker, or a Cursor built-in:

- **Delegate investigative or reasoning work:** use `bg_delegate` when the work is read-only and benefits from a separate context. Give it a bounded question and retrieve the result with `bg_result` after completion.
- **Delegate a shell command, test, build, or watcher:** use `bg_run`. Set `isAgent: false` for ordinary commands and `isAgent: true` only when launching another Pi agent.
- **Produce an attested child-agent report:** use `bg_run_pi_attested` only when the user explicitly asks for attested Pi evidence.
- **No delegation needed:** do the work in the current session. The parent owns the final diff, review, and summary.
- **Context isolation:** for large artifacts, use bounded reads, `readSeek_digest`, `readSeek_grep`, or `session_search`; keep summaries in the main context rather than copying raw payloads.
- **Parallel fan-out:** use `multi_tool_use.parallel` for independent repository reads or ordinary commands. For model-based fan-out, launch separate background tasks with isolated output paths. Never have workers write the same file.
- **Fresh-eyes review:** perform a separate review pass with an explicit rubric, or use a read-only `bg_delegate`. Do not claim independent review from the same reasoning pass.
- **Models:** do not read `~/.cursor/rules/pstack-models.mdc` and do not use Cursor model slugs. Use the current Pi route unless a Pi tool explicitly accepts a provider/model supplied by the user.
- **Interactive questions:** do not name `AskQuestion`. Ask in the normal chat only when the issue is genuinely ambiguous or irreversible. For goal/list workflows, use the goal tools supplied by Pi.
- **Cursor built-ins:** replace Cursor-only skills such as `create-skill`, `deslop`, babysit, `control-ui`, and `control-cli` with the closest local skill, an explicit checklist, or the repository's own commands. Do not invent a command that is not installed.
- **Transcript/session history:** use Pi session tools (`session_search`, `session_list`, `session_read`) or the active conversation. Never assume `~/.cursor/projects/.../agent-transcripts/...` exists.
- **Project-local skills:** use the project's established Pi skill directory. In this pstack repository, keep bundled skills in their current directories; do not generate `.cursor/skills/...` paths.

When a source skill still contains Cursor terminology, follow the Pi substitution above rather than treating the old syntax as executable instructions.
