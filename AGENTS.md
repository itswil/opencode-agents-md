# Agent Instructions (Global)

This file holds conventions for projects built from the `starter` template. The template uses React 19, Vite, TypeScript, and TailwindCSS v4. These rules apply globally. You can drop per-project `AGENTS.md` files.

## Using the Template

The [starter](https://github.com/itswil/starter) repo is a base template for new projects. Install it into your current directory:

```bash
npx giget gh:itswil/starter .
```

## Commands

```bash
pnpm dev        # Start dev server
pnpm build      # Build for production (tsc -b && vite build)
pnpm preview    # Preview production build
pnpm fmt        # Format code (oxfmt)
pnpm fmt:check  # Check formatting (oxfmt --check)
pnpm lint       # Lint (oxlint)
pnpm lint:fix   # Lint + autofix (oxlint --fix)
pnpm test       # Run tests (vitest)
```

## Git Hooks (Husky)

Husky is configured in `.husky/`. The `pre-commit` hook runs `pnpm fmt` and `pnpm lint:fix`. Never check in unformatted or lint-failing code. Run `pnpm fmt` and `pnpm lint` yourself before every commit — never rely on the hook alone.

Work on a feature branch per task and open a PR when done.

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/):

- `feat(component): add new feature`
- `fix: resolve bug`
- `style: update styling`
- `perf: improve performance`
- `docs: update documentation`
- `refactor: restructure code`
- `test: add/update tests`
- `chore: maintenance tasks`

Scope in `()` is optional. Add `!` before `:` for breaking changes (e.g., `feat!: breaking change`).

## Stack

- React 19 + TypeScript
- Vite (with `@vitejs/plugin-react` and `@tailwindcss/vite`)
- TailwindCSS v4 (via `@import "tailwindcss"` in `index.css`, no config file)
- oxlint + oxfmt for linting/formatting
- Vitest with `vitest-browser-react` + Playwright (Chromium) for browser tests
- Husky for git hooks
- pnpm as package manager

## Project Conventions

- The entry point is `src/main.tsx`. It creates the root and wraps `<App />` in `<StrictMode>`.
- React components are default-exported function components (see `src/App.tsx`).
- Use Tailwind utility classes via `className` in JSX. Do not use custom CSS.
- Colocate tests next to source as `*.test.tsx`. Use `vitest-browser-react`'s `render` and `expect.element(...)` assertions, e.g.:
  ```tsx
  import { render } from "vitest-browser-react";
  import { expect, test } from "vitest";
  import App from "./App.jsx";

  test("renders the title", async () => {
    const { getByText } = await render(<App />);
    await expect.element(getByText("Starter")).toBeVisible();
  });
  ```
- JSX/ESM imports resolve both `.tsx` and `.jsx` extensions.
- Styling lives in `src/index.css`. Keep Tailwind the source of truth for design decisions.
- New code goes in `src/components/` (UI), `src/hooks/` (reusable logic), `src/lib/` (utilities), and `src/types/` (shared types). Use kebab-case filenames; component names are PascalCase.
- Use `import.meta.env.VITE_*` for environment variables. Never commit `.env*` files or hardcode secrets.

## Technical Rules

- Keep TypeScript clean under `tsc -b`. No `any`, no `// @ts-ignore` — fix types properly.
- State/data fetching uses plain React hooks and the browser fetch API by default. Don't add libraries (React Query, Zustand, axios, etc.) unless the user asks.
- Prefer small, focused components over large ones. Extract reusable UI into `src/components/`.

## Asking Before Acting

- Ask before installing new dependencies (`pnpm add`).
- Ask before changing build/tooling configs: `tsconfig.json`, `.oxlintrc.json`, `.oxfmtrc.json`, `vite.config.ts`.
- Ask before creating a PR or pushing to a remote — never do it unprompted.

## Allowed Packages

These may be installed without asking. Anything else requires explicit approval:

- **Routing**: `@tanstack/react-router`
- **Data fetching**: `@tanstack/react-query`
- **Forms/validation**: `@tanstack/react-form`, `zod`
- **Client state**: `@tanstack/react-store`
- **Icons**: `lucide-react`
- **Styling**: `tailwindcss`, `clsx`, `tailwind-merge`
- **Utilities**: `date-fns`, `nanoid`
- **Dev/quality**: `@playwright/test`
- **Server**: `hono`, `@hono/node-server`
- **Database/ORM**: `drizzle-orm`, `drizzle-kit`
- **Authentication**: `better-auth`, `bcryptjs`

## Linting & Formatting

- Configs live in `.oxlintrc.json` and `.oxfmtrc.json`. Both ignore `dist/**`, `*.min.js`, `node_modules`, and `.husky`.
- Formatting is handled by oxfmt. TODO characters, quotes, and trailing commas follow oxfmt defaults. Run `pnpm fmt` rather than hand-editing style.
