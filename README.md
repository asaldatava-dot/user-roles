# user-roles — Wallarm Identity & Access prototype

Interactive, clickable prototype of the Wallarm console **Identity & Access (RBAC)** experience.
Static single-file build (`index.html`) — deployable on Vercel with zero configuration.

## What's inside

A self-contained HTML/CSS/JS prototype with a **version switcher** at the top comparing two
access-management models:

- **A · Direct roles** — roles are assigned to people directly; groups are optional. A user can
  exist with just a role. (AWS IAM / GitHub / most SaaS model.)
- **B · Group-driven** — the group is the unit of access; roles attach to groups, never to
  people. Adding a user to a group auto-grants its roles. No group = no access.
  (Okta / Entra / Google Workspace model.)

Both versions are **allow-only / output-first** (no Deny, no precedence, no effect column) and
include the new-user **Invite** flow plus **Revoke invite** / **Deactivate** (no hard delete of users).

Screens: Users list · User → Access · User → Membership/Groups · Groups · Group detail (B) ·
Roles · Role detail. Navigate via the sidebar, clickable rows, tabs, or the floating **QuickBall**
(bottom-right).

## Design system

Visually matches [`@wallarm-org/design-system`](https://github.com/wallarm/design-system) using its
real tokens: brand `--color-w-orange-600` (`#FF441C`), the slate scale, `green/red/blue/purple-600`
for success/danger/link/AI, `--radius-8`, and **Geist** + Geist Mono. Tokens are hardcoded; the live
React components are not imported (that would require the private package).

## Run locally

Any static server works, e.g.:

```bash
python3 -m http.server 4178
# open http://localhost:4178
```

## Deploy on Vercel

Import this repo in Vercel. No framework, no build step — Vercel serves `index.html` as a static
site automatically. (Build & Output settings can be left empty / "Other".)
