# pstack in Antigravity

pstack skills use Cursor names. In Antigravity, read this file once per session, then map each Cursor name in a skill with the tables below. `<pstack>` means the folder that holds `plugin.json`, `skills/`, `agents/`, and `docs/` (typically `~/.gemini/config/plugins/pstack` or `<workspace>/.agents/plugins/pstack`). If a Cursor field has no row, drop it and say so. Never pass a field or value the tool schema rejects.

## Tools

| Cursor | Antigravity |
| --- | --- |
| `Task` | `invoke_subagent` |
| `AskQuestion` | `ask_question` (for multi-choice clarification; ask simple yes/no questions in plain text) |
| Open a todolist, or copy steps into the todolist tool | `update_steps` (and `update_focus` for live status inside multi-step phases) |
| Run another pstack skill (`/name` or "the **name** skill") | `read_file` on `<pstack>/skills/<name>/SKILL.md` and follow it. When telling the user to invoke it, write `/<name>`. |
| `Read`, `Edit`, `Write` | `read_file`, `edit_file`, `write_file` |
| `Grep`, `Glob`, `Shell` / `Bash` | `run_command` (e.g. `rg`, `fd`/`find`, shell commands) |
| `WebSearch` | `search_web` |
| Browser / UI verification (`control-ui`) | `invoke_subagent` with `TypeName: "browser"` (pass `Model: "flash"` by default) |

## Subagent fields

| Cursor | Antigravity |
| --- | --- |
| `subagent_type: generalPurpose` | `TypeName: "self"` (for full read/write/shell capabilities) |
| `poteto-agent`, `comment-sicko` | `TypeName: "poteto-agent"`, `TypeName: "comment-sicko"` (bundled in `<pstack>/agents/`) |
| `readonly: true` | `TypeName: "research"` (read-only sandbox with no network access). If shell read-only commands like `git log`/`git diff` are needed, use `TypeName: "self"` and explicitly state "do not edit files" in `Prompt`, then check `git status` afterward. |
| `readonly: false` | `TypeName: "poteto-agent"` or `TypeName: "self"` |
| `environment: "cloud"` | `Workspace: "share"` (shared repo working copy, like a git worktree) or `Workspace: "branch"` (isolated clone). Name the branch or commit to check out in `Prompt`. |
| `environment: "local"` | `Workspace: "inherit"` (or omit; it is the default) |
| `cloud_base_branch: <b>` | Tell the subagent in `Prompt` to check out `<b>` first. |
| `run_in_background: true` | Omit. `invoke_subagent` runs in the background by default and notifies you when the subagent replies. Do not poll. |
| `resume` | Call `send_message` with the existing subagent's `conversationId`. |
| `file_attachments` | List absolute file paths in `Prompt` so the subagent reads them with `read_file`. |

## Models

Antigravity's `invoke_subagent` `Model` parameter accepts only `"inherit"`, `"pro"`, `"flash"`, and `"flash_lite"` (never raw vendor slugs).

| Cursor | Antigravity |
| --- | --- |
| `claude-` (e.g. `claude-opus-5-5-xhigh-fast`, `claude-fable-5-2-high`) | `Model: "pro"` for high-rigor/code/judgment roles, or `Model: "inherit"` to match the parent session model. Never pass raw `claude-*` slugs. |
| `gemini-` (e.g. `gemini-4-neon-xhigh`, `gemini-4-neon-high`) | `Model: "pro"` or `Model: "inherit"` (use `Model: "flash"` for fast/browser tasks). Never pass raw `gemini-*` slugs. |
| `grok-`, `gpt-`, or any other vendor slug | `Model: "inherit"` (or `Model: "pro"` for heavy reasoning), and note in the summary that Antigravity ran on its native model tier instead of cross-vendor routing. |
| `inherit-parent` or `auto` | `Model: "inherit"` (or omit `Model`). |
| `pstack-models.mdc` (`~/.cursor/rules/pstack-models.mdc`) | `~/.gemini/config/rules/pstack-models.md` (or `<pstack>/rules/pstack-models.md`). If absent, default to `Model: "inherit"` for general work, `"pro"` for high-rigor coding/review, and `"flash"` for fast mechanical/browser tasks. |

> Note on multi-model panels (`arena`, `interrogate`): Antigravity does not route to arbitrary external vendor slugs via `invoke_subagent`. When running `arena` or `interrogate`, spawn independent subagents with distinct personas/angles (using `"pro"` / `"inherit"` / `"flash"` as configured) and note that diversity comes from independent contexts and prompting rather than distinct external vendors.

## setup-pstack

When running `/setup-pstack` in Antigravity:

- **Detect**: The valid subagent `Model` values are `inherit`, `pro`, `flash`, and `flash_lite`.
- **Budget mapping**:
  - `large` → code/judgment/review roles use `pro` (or `inherit`), mechanical workers use `flash`
  - `medium` → code/judgment roles use `inherit`, mechanical/review workers use `flash`
  - `small` → routine/worker roles use `flash` or `flash_lite`, code roles use `inherit`
- **Write**: Write `~/.gemini/config/rules/pstack-models.md` with plain Markdown (no `.mdc` frontmatter) containing the `# defaults: 3` line, `# budget:` line, and one line per role mapped to `inherit`, `pro`, `flash`, or `flash_lite`.

## Paths

| Cursor | Antigravity |
| --- | --- |
| `.cursor/skills/`, `~/.cursor/skills/` | `.agents/skills/`, `~/.gemini/config/skills/` |
| `~/.cursor/plugins/`, `.cursor-plugin/plugin.json` | `~/.gemini/config/plugins/`, `.agents/plugins/`, `plugin.json` |
| `.cursor/rules/*.mdc`, always-applied rules | `AGENTS.md`, `GEMINI.md`, `.agents/rules/*.md`, `~/.gemini/config/rules/*.md`, or plugin `rules/AGENTS.md` |
| `https://github.com/cursor/plugins/blob/main/pstack/<path>` | `<pstack>/<path>` when that file exists locally. Read the local copy and never fetch it. |
| `agent-transcripts` (`~/.cursor/projects/<slug>/agent-transcripts/`) | `~/.gemini/antigravity/brain/<conversation-id>/.system_generated/logs/transcript.jsonl` (and `transcript_full.jsonl` when `truncated_fields` is present). |

## Cursor-only features

Antigravity has no Cursor Automations (`api2.cursor.sh/automations`), Bugbot, Cursor Cloud Agents, or Cursor built-in PR tool. When a step requires one, write one line in your reply: `Skipped in Antigravity: <step>. <reason>.`, then proceed to the next step without blocking.

| Cursor | Antigravity |
| --- | --- |
| Cursor Automations: routines, `update_state`, `api2.cursor.sh/automations` webhooks, `SendToUser` | Skip with the line. For recurring or timed work inside Antigravity, use the `schedule` tool or recommend `/schedule`. |
| `/loop` | Use `/goal` for long autonomous runs or `/schedule` for recurring checks. |
| Bugbot | Triage existing Bugbot PR comments as ordinary GitHub review threads via `gh`. Skip steps that trigger or configure Bugbot. |
| Cursor Cloud Agents | Map `environment: "cloud"` to `Workspace: "share"` or `"branch"` on `invoke_subagent`. Skip steps that query a Cursor cloud dashboard. |
| Built-in PR tool | Use `gh` CLI commands via `run_command` (e.g., `gh pr create`, `gh pr view`). Not a skip. |
| `cursor-team-kit` (`/deslop`, `control-cli`, `control-ui`) | For `control-ui`, use the `browser` subagent; for `control-cli`, drive via `run_command`; for `/deslop`, use pstack's own `/unslop` and `/no-comments` plus a manual diff pass. |
