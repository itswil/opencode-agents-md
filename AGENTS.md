# Agent Instructions (Global)

This file holds conventions for projects built from the [Cove](https://github.com/mugnavo/cove) template (formerly TanStarter). The template uses React 19, TanStack Start, TailwindCSS v4, Drizzle ORM, and Better Auth. These rules apply globally. You can drop per-project `AGENTS.md` files.

## Using the Template

The [cove](https://github.com/mugnavo/cove) repo is a base template for new projects. Scaffold a new project with:

```bash
pnpm create cove
```

Prerequisites: Node.js >= 24, pnpm >= 12, and the Vite+ CLI (`vp`).

## Commands

Scripts run through Vite+: `vp <name>` is a built-in command, `vpr <name>` (shorthand for `vp run`) runs a `package.json` script or `vite.config.ts` task.

```bash
vpr dev              # Start dev server (http://localhost:3000)
vpr build            # Build for production (vp build, Nitro output)
vpr preview          # Preview production build
vpr format           # Format code (vp fmt / oxfmt)
vpr format:check     # Check formatting (vp fmt --check)
vpr lint             # Lint + type check (vp lint --type-aware --type-check)
vpr lint:fix         # Lint + type check + autofix
vpr check            # Format, lint, and type check in one pass (vp check)
vpr test             # Unit/integration tests (Vitest, once)
vpr test watch       # Vitest in watch mode
vpr test:e2e         # Build app + Playwright end-to-end tests
vpr db generate      # Generate Drizzle migration
vpr db migrate       # Apply migration
vpr db studio        # Browse database
vpr ui add <name>    # Add shadcn/ui component
vpr auth:generate    # Regenerate Better Auth schema after config changes
vpr env:load         # Validate .env.local against .env.schema (varlock)
vpr deps             # Selectively upgrade dependencies (taze)
```

Run one-off TypeScript/JavaScript scripts with `vpx <file>` — do not add `tsx` or `ts-node`.

## Git Workflow

- The `pre-commit` hook runs formatting and linting. Never check in unformatted or lint-failing code. Run `vpr check` yourself before every commit — never rely on the hook alone.
- Work on a feature branch per task and open a PR when done.
- Use [Conventional Commits](https://www.conventionalcommits.org/):
  - `feat(component): add new feature`
  - `fix: resolve bug`
  - `style: update styling`
  - `perf: improve performance`
  - `docs: update documentation`
  - `refactor: restructure code`
  - `test: add/update tests`
  - `chore: maintenance tasks`
  - Scope in `()` is optional. Add `!` before `:` for breaking changes (e.g., `feat!: breaking change`).

## Stack

- React 19 + TypeScript, with TanStack Start + Router + Query (file-based routing, SSR)
- TailwindCSS v4 + shadcn/ui + Base UI primitives (`class-variance-authority`, `cn`)
- Drizzle ORM + PostgreSQL (`postgres` driver)
- Better Auth for authentication
- Vite+ (`vp`/`vpr`/`vpx` unified toolchain) with Nitro for the production server
- Varlock for env vars (`.env.schema` is the source of truth; `import { ENV } from "varlock/env"`)
- evlog for structured request logging
- Vitest for unit/integration tests, Playwright for e2e
- pnpm as package manager

## Project Conventions

- Source lives in `src/`; import via the `#/*` alias (e.g. `#/lib/db/schema`).
- Use the shadcn CLI (`vpr ui add <component>`) for new UI components & primitives.
- Use `lucide-react` for UI icons with the `Icon` suffix (e.g. `Loader2Icon`); use `@icons-pack/react-simple-icons` for brand icons (e.g. `SiGithub`).
- Colocate tests next to source; keep them behavioral (outcomes), not implementation-focused.
- Keep UI copy user-centered: describe outcomes and next steps without exposing providers, internal states, or implementation details.
- Never commit `.env*` local files or hardcode secrets. Put local values in `.env.local` and validate with `vpr env:load`. In code, read env via `ENV` from `varlock/env` — never `process.env` directly.
- Don't run a standalone production build after every change. Use `vpr lint` as the baseline and run the narrowest relevant tests; `vpr test:e2e` performs its own build.

## Code Style

- Write simple, readable, reusable code. Prefer straightforward solutions for current requirements — introduce abstractions or architectural layers only when there is a concrete need.
- Keep the main control flow easy to follow. Avoid wrapper layers that merely rename or forward calls, and avoid fragmenting logic across unnecessary files.
- Extract helpers only when they meaningfully improve readability, reuse, or testability.
- Prefer functional TypeScript code and `const` arrow functions over `function` declarations.
- Keep types simple and close to where they are used. Prefer inference; avoid type gymnastics unless necessary.
- Add concise comments only for non-obvious intent, unusual edge cases, and important constraints. Explain why, not what.

## Technical Rules

- Keep TypeScript clean under type checking. No `any`, no `// @ts-ignore` — fix types properly.
- For ad-hoc scripting (automation, one-off tasks, file processing), prefer Bun over Python or Perl where possible.
- Data loading/caching/mutations use TanStack Start/Router + Query; server functions and API routes define the server boundary. Don't add other state libraries unless the user asks.
- Use Drizzle ORM's query builder and schema API — avoid writing raw SQL. The Better Auth schema (`src/lib/db/schema/auth.schema.ts`) is generated — don't hand-edit it; change `src/lib/auth/auth.ts` and run `vpr auth:generate`.
- Be robust at system boundaries (user input, auth, external APIs, persistence). Within trusted code, rely on established invariants instead of guarding hypothetical states.
- Handle edge cases in proportion to their likelihood and impact — don't add complexity for contrived, low-impact scenarios.

### Accessibility

- Use semantic HTML and proper ARIA attributes; keep labels associated with inputs. Test keyboard navigation on interactive elements.

### Error Handling

- Handle errors explicitly with `try/catch` (or `.catch`) and show user-facing error states. Don't silently swallow errors — log them.

### Security

- Sanitize and validate all user input with `zod`. Never log secrets or tokens. Use parameterized queries (Drizzle query builder covers this).
- Server functions/routes must re-validate auth via Better Auth middleware — don't trust client-supplied identity.

### Testing

- Write tests for new logic and components; keep them behavioral (outcomes), not implementation-focused.
- `vpr test` runs Vitest unit/local integration tests; `vpr test:e2e` runs Playwright end-to-end tests.
- Never commit `test.skip` or `test.only`.

### Naming

- Use descriptive names over abbreviations (`fetchUserProfile`, not `getU`). Boolean props read as questions (`isLoading`, `hasError`).

### Performance

- Memoize expensive computations; avoid re-creating handlers in hot paths when it matters. Keep the bundle small — lazy-load route components.

### Types

- Prefer explicit return types on exported functions; use `zod`/Varlock inference for data shapes coming from the API or env.
- Use `type` instead of `interface` when declaring types.

## Asking Before Acting

- Ask before installing new dependencies (`pnpm add`).
- Ask before changing build/tooling configs: `tsconfig.json`, `vite.config.ts`, `nitro.config.ts`, `drizzle.config.ts`, `components.json`.
- Ask before creating a PR or pushing to a remote — never do it unprompted.

## Allowed Packages

These may be installed without asking. Anything else requires explicit approval:

- **UI**: `class-variance-authority`, `cn`, `tw-animate-css`
- **Icons**: `lucide-react`, `@icons-pack/react-simple-icons`
- **Data/validation**: `zod`
- **Utilities**: `date-fns`, `nanoid`
- **Dev/quality**: `@playwright/test`, `taze`

The core stack (TanStack Start/Router/Query, Drizzle, Better Auth, Tailwind, shadcn/ui, Varlock, evlog) ships with Cove — prefer its built-in patterns over adding parallel libraries.

## Linting & Formatting

- Formatting is handled by oxfmt, linting by oxlint — both via Vite+ (`vpr format`, `vpr lint`, or `vpr check` for both). Run `vpr check` rather than hand-editing style.
