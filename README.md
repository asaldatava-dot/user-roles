# user-roles — Wallarm Identity & Access prototype

Interactive, clickable prototype of the Wallarm console **Identity & Access (RBAC)** experience.
Static single-file build (`index.html`) — deployable on Vercel with zero configuration.

## Model

**Role required, group optional.** Every user must have at least one role; that role can come
**directly** or **through a group** (a group carries roles and grants them automatically). Groups
are an optional scaling tool — useful for large tenants, skippable for small ones (1–2 admins).
Effective access is the **union** of direct roles and group roles (allow-only — no Deny, no
precedence, no effect column).

## Invite flow

A single required **Access** field answers one question — "what access?" — with two ways to
answer: pick a **group** (roles included, shown first as the recommended path) or assign a **role**
directly. `Send` stays disabled until at least one is chosen, enforcing the role requirement. A live
preview shows the resulting permissions. New users can be **invited**, and an invite can be
**revoked**; existing users can be **deactivated** (no hard delete of people).

## Screens

Output-first throughout: Users list · User → Access (what they can do + where it comes from) ·
User → Membership (direct roles + groups) · Groups · Group detail · Roles · Role detail
(human-readable capabilities). Navigate via the sidebar, clickable rows, tabs, or the floating
**QuickBall** (bottom-right).

## Design system

Visually matches [`@wallarm-org/design-system`](https://github.com/wallarm/design-system) using its
real tokens: brand `--color-w-orange-600` (`#FF441C`), the slate scale, `green/red/blue/purple-600`
for success/danger/link/AI, `--radius-8`, and **Geist** + Geist Mono. Tokens are hardcoded; the live
React components are not imported (that would require the private package).

## Run locally

```bash
python3 -m http.server 4178
# open http://localhost:4178
```

## Deploy on Vercel

Import this repo in Vercel. Framework preset **Other**, no build command, no output dir — Vercel
serves `index.html` as a static site automatically.
