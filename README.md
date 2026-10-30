# 🏝️ opencode config

Global [OpenCode](https://opencode.ai) configuration — powered by the [Cove](https://github.com/mugnavo/cove) stack conventions.

## 📦 Install

Clone into the XDG config directory:

```bash
git clone https://github.com/itswil/opencode-agents-md ~/.config/opencode
```

`~/.config/opencode` is where OpenCode reads its global config from.

## 📂 What's inside

| File | Purpose |
|---|---|
| `AGENTS.md` 🤖 | Agent instructions applied to every project (TanStack Start, Drizzle, Better Auth, …) |
| `opencode.jsonc` ⚙️ | Global config (currently defaults + schema) |
| `package.json` 📎 | Pins `@opencode-ai/plugin` |

## 🔒 Machine-local

These files are gitignored — they stay on your machine and aren't shared between machines:

- `cli.json` 🎨 — UI/theme preferences
- `service.json` 🔑 — local service password
- lockfiles 📌
