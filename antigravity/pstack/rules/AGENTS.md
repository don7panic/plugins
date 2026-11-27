# pstack Antigravity Adapter Rule

Whenever you run `/poteto-mode` or any skill from the `pstack` plugin:

1. Read `docs/harness/antigravity.md` inside the `pstack` plugin directory once per session before following Cursor-specific tool, subagent, model, or path instructions.
2. Map Cursor tools to Antigravity tools: `Task` → `invoke_subagent`, `AskQuestion` → `ask_question`, todolist → `update_steps`, `generalPurpose` → `self`, `readonly: true` → `research` (or `self` with no-edit instruction if shell inspection is needed), `environment: "cloud"` → `Workspace: "share"` or `"branch"`, `resume` → `send_message`.
3. Never pass raw Cursor model slugs (`claude-opus-*`, `claude-fable-*`, `gemini-4-*`, `grok-*`, `gpt-*`) to `invoke_subagent`. Map them to Antigravity's valid `Model` values: `"inherit"`, `"pro"`, `"flash"`, or `"flash_lite"`.
4. When a playbook step calls for a Cursor-only feature (Bugbot, Cursor Automations, Cursor Cloud Agent dashboard), emit `Skipped in Antigravity: <step>. <reason>.` and continue without stopping.
