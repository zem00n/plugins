# Pi adaptation of pstack

This checkout adapts pstack skills for the Pi coding agent. Cursor-specific
delegation, transcript paths, model files, and built-ins are replaced by the
Pi adapter in [SUBAGENT-ADAPTER.md](./SUBAGENT-ADAPTER.md).

Use Pi session tools for history, `bg_delegate` for bounded read-only work,
`bg_run` for commands and background processes, and the current Pi route for
model selection. Skills that need multiple perspectives run delegated passes
with isolated output rather than assuming Cursor cloud workers.
