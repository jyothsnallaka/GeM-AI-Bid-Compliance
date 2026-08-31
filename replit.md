# GeM AI Bid Compliance

An evidence-first procurement control room for CPCL's SIH26100 prototype, with separate officer and bidder workflows for tendering, bid compliance review, verification, decisions, notifications and auditability.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/gem-ai-bid-compliance/src/App.tsx` — source of truth for the demo web application, routes, seeded procurement records, local persistence, compliance evidence, reports, exports and role guards
- `artifacts/gem-ai-bid-compliance/src/index.css` — shared light/dark theme tokens, typography, grid surface and responsive utilities
- `artifacts/api-server/src/` — preserved Express API starter
- `lib/db/src/` — preserved Drizzle/PostgreSQL schema starter
- `lib/api-spec/openapi.yaml` — preserved API contract starter

## Architecture decisions

- The SIH demo uses fictional records and mock verification-provider evidence; it never calls live government systems.
- The web demo currently persists changes in browser localStorage because the transferred project did not contain the original application backend workflow.
- The officer is the final decision maker; compliance and AI/system recommendations are advisory and every decision creates an audit event and bidder notification.
- The fixed demo date is Tuesday, 1 September 2026, while seeded bid records retain varied fictional dates so report windows can be demonstrated.

## Product

The app provides role-separated Officer and Bidder workspaces. Officers can create, publish and cancel tenders; inspect bidder evidence; verify, approve, reject, request clarification or require manual verification; view reports; and export registers, review queues and audit trails. Bidders can find published tenders, attach document metadata, declare requirements, submit once per tender, track outcomes and receive updates. Light/dark theme selection and responsive layouts are included.

## User preferences

The existing procurement-oriented light visual identity and login surface should remain recognizable when extending the app.

## Gotchas

- Demo file uploads intentionally store metadata only in the browser; they are not persistent document storage.
- Clear browser localStorage keys (`gem-session`, `gem-tenders`, `gem-bids`, `gem-notices`, `gem-audits`, `gem-theme`) when resetting a presentation demo.
- Keep mock provider labels explicit and do not add live government integrations without a separately approved architecture.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
