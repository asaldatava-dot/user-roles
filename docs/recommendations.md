# Identity & Access — recommended changes

Design recommendations against the current developer implementation.
The prototype in this repo is the reference (etalon) for the target behaviour.

## Tables (apply to every table)

- **One text style for all cells.** Body text: Geist, 14px, regular (400),
  line-height 20px, color `--color-text-primary` (#0F172B). Header text: 12px.
  Today the size and weight vary between tables.
- **No monospace for names.** Role and group names use the regular font, not
  monospace. Keep monospace only for technical identifiers (ARNs, slugs,
  policy action names, resource types) — same convention as AWS IAM.
- **No medium weight for names.** Names render at regular (400), not 500.
- **Consistent padding and row height.** 8px vertical / 14px horizontal padding,
  rows ~36px, header 32px — the same in every table.
- **A chip must never change the row height.** Tags/pills are clamped to the
  20px line box and vertically centered, so a cell with a chip is exactly as
  tall as a cell with plain text. Rows only grow when content wraps to a second
  line.

## Users table

- **Email is its own column** (not stacked under the name).
- **Drop the "Type" column.**
- **Show Groups and Roles as separate columns**, each as tags.
- **Add a "Verified" indicator** — a small green check next to verified emails.
- **Keep Status as a pill** and add a "Last active" column.
- **Groups column is a bit wider** to fit common cases.
- **Group overflow is width-driven.** Show as many group tags as fit; collapse
  the rest into a "+N" chip *only when the tags exceed the column width*. The
  "+N" chip opens a tooltip listing the hidden groups. It is not a fixed count.

## User detail page

- **Use the Card component for the summary** (rounded-8, 1px border, subtle
  shadow) instead of a plain box.
- **Move Status into the page header**; remove the separate "User" tag.
- **Put User ID next to Email** in the same row.
- **Put Identity Provider in the same row** as Created / Updated / Last sign-in.
- **Lay out Roles and Groups in two columns** inside the summary card to keep it
  compact.
- **Attribute labels are 14px**, not 12px.
- **Split "Roles & groups" into two separate tabs.** Tabs are now
  **Access | Roles | Groups**.
- **Roles and Groups tabs use tables**, not cards:
  - Roles: `Role | Source (Direct / via <group>) | Access`.
  - Groups: `Group | Roles | Access`.
  - Access is derived from the same role policies as the Access tab
    (single source of truth).

## Navigation & breadcrumbs

- **Breadcrumbs end on the current page.** List pages read `Settings › Users`,
  `Settings › Roles`, `Settings › Groups` — the current section is the active
  last item, no redundant trailing segment (e.g. drop "All users").
- **Fix the Roles breadcrumb parent** — it should be `Settings › Roles`, not
  `Settings › Users › Roles`.
- **Remove the "Access management" middle segment** from breadcrumbs.
- **A navigation item must not act as a filter.** Remove the "Pending invites"
  nav entry; move status filtering into the table toolbar.
- **No page descriptions** under the title on the Users, Roles and Groups list
  pages.

## Permissions / policy editor

- **Follow AWS IAM exactly** — don't reinvent it (most users are DevOps):
  - Service → actions grouped by access level (List / Read / Write /
    Permissions management / Tagging) → resources (All / specific ARN) → Effect.
  - Support both **Allow and Deny**.
  - Offer a **Visual | JSON** toggle.
- **Friendly rollup in read-only views.** Where we only show the result (cards,
  table summaries), collapse levels into View / Edit / Admin / Custom.

## Create role / create group

- **Single name field with an auto-generated slug** (identifier fixed after
  creation); keep the form minimal.
- **No giant catalog tables** in the create flow.

## General principle

- **Keep v1 minimal.** When in doubt, remove. Prefer the simplest version that
  still works — the reference should show the ideal, lean first version.
