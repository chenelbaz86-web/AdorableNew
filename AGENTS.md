# Adorable - AI App Builder

Open-source clone of Lovable. Single Next.js 15 app (not a monorepo) using TypeScript, Tailwind CSS v4, and Turbopack.

## Key commands

| Task | Command |
|------|---------|
| Install deps | `npm install` |
| Dev server | `npm run dev` (port 3000, Turbopack) |
| Lint | `npm run lint` |
| Build | `npm run build` |
| DB migrations | `npx drizzle-kit push` |

## Cursor Cloud specific instructions

### Services required

- **PostgreSQL** (local, port 5432): Start with `sudo pg_ctlcluster 16 main start`. Database `adorable` must exist, owned by `postgres:postgres`.
- **Redis** (local, port 6379): Start with `redis-server --daemonize yes`. No auth required.
- **Next.js dev server**: `npm run dev` runs on port 3000 with Turbopack.

### Environment variables

A `.env` file is required at the repo root. See `.env.example` for the template. Local services use:
- `DATABASE_URL=postgresql://postgres:postgres@localhost:5432/adorable`
- `REDIS_URL=redis://localhost:6379`

External API keys needed for full functionality: `ANTHROPIC_API_KEY`, `FREESTYLE_API_KEY`, `NEXT_PUBLIC_STACK_PROJECT_ID`, `NEXT_PUBLIC_STACK_PUBLISHABLE_CLIENT_KEY`, `STACK_SECRET_SERVER_KEY`. Without these, the app still loads the landing page but cannot create apps or authenticate users.

### Gotchas

- `npm run lint` exits with code 1 due to pre-existing lint warnings/errors in the codebase (e.g., `@typescript-eslint/no-explicit-any`, unused vars). This is expected and not caused by setup.
- `next.config.ts` sets `typescript.ignoreBuildErrors: true` and `eslint.ignoreDuringBuilds: true`, so `npm run build` will succeed even with lint/type errors.
- The database schema is managed via Drizzle ORM. Run `npx drizzle-kit push` after schema changes in `src/db/schema.ts`. The `tablesFilter: ["!mastra*"]` in `drizzle.config.ts` excludes Mastra-managed tables.
- systemd is not available in the cloud VM — start services directly (e.g., `redis-server --daemonize yes`) rather than via `systemctl`.
