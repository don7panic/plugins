# pstack for Antigravity

This is an Antigravity-adapted build of [pstack](https://github.com/cursor/plugins/tree/main/pstack) (v0.15.48): all 59 skills (22 `/poteto-mode` playbooks, 35 workflow skills, 24 engineering principles), both custom subagents (`poteto-agent` and `comment-sicko`), the full user guide, and an Antigravity harness adapter at [docs/harness/antigravity.md](./docs/harness/antigravity.md).

## Install

Copy or symlink this folder into Antigravity's global plugins directory (or a workspace's `.agents/plugins/` directory):

```bash
# Global (all workspaces)
ln -sfn /Users/oasis/workspace/my-plugins/antigravity/pstack ~/.gemini/config/plugins/pstack

# Or copy instead of symlinking
cp -R /Users/oasis/workspace/my-plugins/antigravity/pstack ~/.gemini/config/plugins/pstack
```

Antigravity discovers `plugin.json`, `rules/AGENTS.md`, `agents/*.md`, and all `skills/*/SKILL.md` automatically on the next turn.

## Usage

- Use `/poteto-mode <task>` as the main entry point for rigorous engineering tasks (bug fixes, features, refactoring, perf, shipping, autonomous runs, etc.).
- Run `/setup-pstack` to configure Antigravity subagent model tiers (`inherit`, `pro`, `flash`, `flash_lite`) per role.
- Invoke any bundled skill directly, e.g. `/how`, `/why`, `/architect`, `/tdd`, `/blast-radius`, `/interrogate`, `/arena`, `/swarm`, `/unslop`, `/no-comments`, `/poteto-help`.
- New to pstack? Read [docs/guide/README.md](./docs/guide/README.md).

## What Was Adapted for Antigravity

- **Manifest ([plugin.json](./plugin.json))**: Uses Antigravity's native schema (`name`, `displayName`, `version`, `description`, `author`, `homepage`, `repository`, `logo`, `suggestedPrompts`).
- **Always-on Adapter Rule ([rules/AGENTS.md](./rules/AGENTS.md)) & Harness Guide ([docs/harness/antigravity.md](./docs/harness/antigravity.md))**: Maps Cursor tool names (`Task`, `AskQuestion`, todolist), subagent fields, model slugs, config/transcript paths, and Cursor-only features to Antigravity equivalents.
- **Skill Headers**: Every `skills/*/SKILL.md` that references Cursor-specific terms points to `docs/harness/antigravity.md` so skills work even if invoked standalone.
- **Subagents ([agents/poteto-agent.md](./agents/poteto-agent.md), [agents/comment-sicko.md](./agents/comment-sicko.md))**: Converted to Antigravity subagent frontmatter (`subagent: true`, `mainAgent: false`, explicit `tools`) and updated fallback plugin search paths to `~/.gemini/config/plugins/pstack/` and `.agents/plugins/pstack/`.
- **Excluded Cursor-only packs**: `automations/benny/` and `.cursor-plugin/` are omitted; links to omitted files point to upstream GitHub `main`.

## License

MIT — see [LICENSE](./LICENSE).
