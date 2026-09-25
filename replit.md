# Basket Fast

Basket Fast is a court-inspired storefront with a private admin workspace for managing its catalog and manually entered orders.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server
- `pnpm --filter @workspace/basket-fast run dev` — run the storefront
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- Copy `artifacts/api-server/.env.example` to `artifacts/api-server/.env` and set `ADMIN_PASSWORD` and `SESSION_SECRET` before using the admin workspace.
- The current API intentionally uses an in-memory store. It resets when the API restarts; add the owner-provided `DATABASE_URL` only when persistence is requested.

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- Temporary store: in-memory catalog and order records, ready to replace with the existing PostgreSQL contract later
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

_Populate as you build — short repo map plus pointers to the source-of-truth file for DB schema, API contracts, theme files, etc._

## Architecture decisions

- The public catalog is served by the API rather than duplicated in the storefront.
- Admin sessions use an HTTP-only signed cookie and credentials from the API artifact's manual `.env` file.
- Catalog edits and manual orders are deliberately in-memory until the owner provides a database URL.

## Product

- Public storefront with catalog browsing, category filters, product details, cart, and checkout-ready order submission.
- Private admin access with catalog edits, live overview counts, order status updates, and manual order entry.

## User preferences

- Do not connect a database or add database persistence until the owner supplies a database URL.
- Keep admin credentials in a manually managed `.env` file, not hard-coded in source.

## Gotchas

- The in-memory store resets whenever the API process restarts.
- The `.env` file is intentionally ignored and is not created from the example automatically.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
