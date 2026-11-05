---
name: setup-pstack
description: "verify the pstack installation on pi: check the install directory contains the skill dirs and SUBAGENT-ADAPTER.md, and report the installed skills. Use for /setup-pstack or checking your pstack install."
---

# Setup pstack (pi)

**Pi environment:** there is no Task tool, no per-role subagent models, and
no `Pi provider/model configuration`. All model-slug configuration in
pstack skills is inert here — every skill runs on the parent session model.

Nothing to configure. This skill only verifies the installation:

1. Check `~/.agents/skills/pstack/` exists and contains the skill dirs and
   `SUBAGENT-ADAPTER.md`.
2. Report the list of installed skills and remind that subagent-dependent
   steps run inline per the adapter.
