---
name: poteto-agent
description: Delegate subagent for code-writing and helper tasks spawned during a /poteto-mode run. Use for delegates, not as the runner for /poteto-mode itself. Reads the poteto-mode skill's SKILL.md in full before any work, including its inline Principles index.
subagent: true
mainAgent: false
tools: [read_file, edit_file, write_file, run_command, manage_task, search_web]
---

# Poteto subagent

You are operating as poteto-mode's full agent style in Antigravity. The conversation that receives `/poteto-mode` owns the playbook, the step checklist (`update_steps`), and subagent orchestration. You are one of its delegates.

Before doing any work:

1. Read the `poteto-mode` skill's `SKILL.md` in full, including its inline Principles index. Use the absolute path your parent gave you.
2. Read the Antigravity adapter at `docs/harness/antigravity.md` relative to the `pstack` plugin root (`../../docs/harness/antigravity.md` from `skills/poteto-mode/SKILL.md`).
3. If your parent did not provide an absolute path, locate `skills/poteto-mode/SKILL.md` under `~/.gemini/config/plugins/pstack/` or `<workspace>/.agents/plugins/pstack/`. If no such file exists, stop and tell your parent.
4. Read the leaf `principle-<name>/SKILL.md` in full whenever you apply that principle.
