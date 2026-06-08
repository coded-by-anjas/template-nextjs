# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

Package manager is **pnpm**.

- `pnpm dev` — start dev server (http://localhost:3000)
- `pnpm build` — production build
- `pnpm start` — serve the production build
- `pnpm lint` — ESLint 9 (flat config, `eslint.config.mjs`)
- `pnpm typecheck` — `tsc --noEmit`
- `pnpm format` — Prettier write across the repo; `pnpm format:check` to verify only

There is no test runner configured.

## Pre-commit

The Husky `pre-commit` hook runs `pnpm lint-staged` (eslint --fix + prettier on staged files only). A full build is intentionally NOT in the hook — run `pnpm build` / `pnpm typecheck` manually or in CI.

## ESLint

ESLint 9 flat config in `eslint.config.mjs` — spreads `eslint-config-next/core-web-vitals` + `eslint-config-next/typescript`, then `eslint-config-prettier/flat`. Note `eslint-config-next@16` requires `eslint >=9` — do not downgrade to v8 (it crashes with a circular-JSON error against the legacy `.eslintrc` format).

## Stack & conventions

- **Next.js 16 (App Router)** + **React 19** + **TypeScript strict**
- **Tailwind CSS v4** — configured via `@import 'tailwindcss'` and `@theme` in `src/app/globals.css`; there is no `tailwind.config` file. Design tokens are CSS variables (neutral base color, light/dark via `.dark` variant).
- **shadcn/ui** — "new-york" style, RSC enabled, Lucide icons. Components install to `@/components/ui`.
- Import alias `@/*` maps to `src/*`.
- Use `cn()` from [src/lib/utils.ts](src/lib/utils.ts) (clsx + tailwind-merge) for conditional classes.

## Prettier (enforced style)

No semicolons, single quotes, trailing commas (`all`), print width 100, 2-space tabs, arrow parens avoided (`x =>` not `(x) =>`).

## Layout

`src/app/` holds routes (`layout.tsx`, `page.tsx`, `globals.css`). `src/lib/` holds utilities. `src/components/` (and `@/hooks`) are aliased but not yet created — add them as features grow.
