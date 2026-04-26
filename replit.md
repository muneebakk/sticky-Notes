# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.

## Artifacts

- **sticky-notes** (`artifacts/sticky-notes`): React + Vite sticky notes web app at `/`. No backend.
  - Storage keys: `sticky-notes:v2` (notes, with migration from `sticky-notes:v1`), `sticky-notes:theme`, `sticky-notes:sort`, `sticky-notes:lang`.
  - Features: 6 preset colors + custom color picker, light/dark theme, search (`/`), color/tag filters, sort (recently edited / added / manual), pin to top, export/import JSON, hashtag extraction, keyboard shortcuts (`/` `N` `Esc`, `Ctrl/Cmd+Enter`), copy/duplicate, FAB on mobile.
  - Markdown rendering: bold, italic, headings, lists (ul/ol), inline code, links, hashtags, and **task checkboxes** (`- [ ]` / `- [x]`). Click a note to edit raw markdown; blur or `Esc`/`Cmd+Enter` returns to rendered view. Checkbox clicks toggle the source via `toggleTaskAt()` without entering edit mode. Custom safe renderer in `src/markdown.ts` (escapes HTML before applying transforms; restricts links to `http(s)`/`mailto`).
  - Voice-to-note: mic button in the composer uses Web Speech Recognition API (`webkitSpeechRecognition` fallback). Continuous + interim results stream into the draft, language follows the UI language (`en-US`/`ur-PK`/`hi-IN`/`es-ES`/`fr-FR`). Pulsing red state while listening; tap again to stop.
  - Drag-and-drop reorder: drag any note onto another to reorder. Auto-switches sort to "Manual order" so the new arrangement persists.
  - Reminders / due dates: per-note reminder button opens a `datetime-local` editor. Due states styled as `--future` (neutral), `--soon` (amber, within 24h), `--overdue` (red). Browser `Notification` API fires on overdue when permission granted; checked every 30s.
  - Multi-language UI: `en`, `ur` (RTL), `hi`, `es`, `fr` via `src/i18n.ts` `tr(lang, key, vars)`. Language selector in topbar. Sets `documentElement.lang` and `dir`.
