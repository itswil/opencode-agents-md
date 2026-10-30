# opencode config

Global [OpenCode](https://opencode.ai) configuration. Cloned to the XDG config directory:

```bash
git clone https://github.com/itswil/opencode-agents-md ~/.config/opencode
```

`~/.config/opencode` is where OpenCode reads its global config from. After cloning, OpenCode picks up:

- `opencode.jsonc` — global config (currently defaults + schema)
- `AGENTS.md` — agent instructions applied to every project
- `package.json` — pins `@opencode-ai/plugin`

Machine-local files (`cli.json`, `service.json`, lockfiles) are gitignored and not shared between machines.
