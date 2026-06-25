# Identity & Access — change handoff

The prototype (localhost / Vercel) is the reference design. This document lists,
page by page, the changes needed to bring the current production build in line
with it, and **why** each one matters. Each item is meant to be small and
actionable.

---

## Cross-cutting (applies everywhere)

- **One model and vocabulary.** It's a **Role** (with its policy) and a **Group**
  — not "Policy / Policy name / team". Don't show two identical-looking rows
  (e.g. "Admin / Admin") distinguished only by an opaque "Type".
  *Why:* identical-looking rows that mean different things are unreadable.
- **Single source of truth.** The effective-access view and the Roles/Groups
  lists derive from the same data, so they can never contradict each other.
- **Tables — one type ramp.** Body text: 14px / regular / 20px line-height /
  text-primary; header 12px. Consistent padding (8px / 14px) and row height
  (~36px). A chip never changes row height (clamped to the line box).
- **First-column name = link on hover.** Names render as primary text and turn
  into a link (blue + underline) on row hover; the whole row navigates.
- **Tabs = design-library Tabs**, active tab underlined in **brand orange**.
- **Permissions editor = AWS IAM style** (service → actions by access level →
  resources → Allow/Deny, Visual/JSON toggle). Don't reinvent it — the audience
  is DevOps.

---

## 1. Users table

1. **Show role/group names, not counts.** The Groups and Roles columns currently
   show numbers (1, 1). Show the actual names as tags, collapsing extras into a
   "+N" chip when they overflow the column width (the chip opens a tooltip with
   the rest). *Why:* a count says how many, not which — names let you read access
   straight from the list.
2. **Verified indicator on Email.** A green check next to verified emails, with a
   "Verified" tooltip on hover. *Why:* surfaces verification at a glance.
3. **Name = plain text, link on hover** (today it's always blue). *Why:* the row
   is clickable; a permanently blue name is noisy.
4. **Consistent status wording.** Production says "Enabled" — pick one word
   (mockup uses "Active") and use it in the list and the detail page.
5. **Column order:** `User · Email · Auth · Status · Groups · Roles · Last active`
   (Last active is the final column). Auth stays as a placeholder ("—") until
   there's data.
6. **Row removal via select + bulk bar** (checkbox → bottom action bar) rather
   than a per-row control, so the right edge stays clean.

## 2. User detail page

1. **Status in the header next to the name; remove the "Summary" heading.**
   *Why:* status is the first thing to see; a heading above a single card is
   redundant.
2. **Compact summary card (Attribute Field component).** Put related attributes on
   one row — **User ID next to Email**, **Identity provider in the same row as
   Created / Updated / Last sign-in** (today "Last sign-in —" sits alone).
   Actions appear **on hover** (pencil / copy), not three repeated "Edit" links.
   *Why:* denser, scannable; repeated "Edit" and empty rows waste space.
3. **Add an "Access" tab — the key change.** Today there's only Roles | Groups
   (attached policies). Add a tab showing **effective access**: by service,
   Allow/Deny, amount of permissions. *Why:* the first question on a user is
   "what can they actually do?" — today you must open each policy and read its
   statements to find out.
4. **Tabs: Access | Roles | Groups**, active underlined in brand orange.
5. **Roles tab — show access, not internals.** Replace "Type (Default/Managed)"
   and "Statements (1)" with **Source** (Direct / via &lt;group&gt;) and **Access**
   (service + permission count; Deny in red text). *Why:* "1 statement" /
   "Managed" don't say what the role grants.
6. **Mirror roles and groups as chips in the summary card.** *Why:* see what the
   user belongs to before switching tabs.
7. **Verified indicator + tooltip on Email** (same as the table).

## 3. Roles list

1. **Name = link on hover; row opens the role.**
2. **Split usage into two sortable columns — `Users` and `Groups`** (numbers),
   instead of one combined "N users · N groups" cell. Clicking a header sorts
   asc/desc. *Why:* you can sort by reach (find heavily-used or unused roles); a
   combined string can't be sorted.
3. **Minimal columns:** `Role · Users · Groups · Created · Updated` — no "Type" /
   "Statements". *Why:* the list is for navigation, not internal structure.
4. **"Add role" → simplified create** (single name + auto identifier + IAM-style
   editor).

## 4. Role detail page

1. **Header = role name + one "Actions" menu (Duplicate / Delete).** Remove the
   "Managed by Acme Corp" pill, the "Attached to N users · N groups" meta, and the
   "This role can edit rules…" alert. *Why:* the header should be clean; the
   explanatory text and split buttons are noise.
2. **Summary card on the Attribute Field component, no "Summary" heading:** Name
   (editable, pencil on hover), Resource ID (copy), Created At, Updated At.
3. **Remove the "N statements · N allow · N deny" line.** *Why:* it's derivable
   from the table below and just adds clutter.
4. **Permissions table — fill it and make it readable:**
   - **Service** actually populated (today the cell is empty).
   - **Permissions** as the real action chips, not a bare "*" (keep "*" only for a
     true wildcard role like Admin).
   - **Resources** shown friendly ("All resources") instead of raw `*:*:*:**`.
   - **Effect** — Allow green, Deny red.
   - **"⋯" per row** to remove a service (statement).
   *Why:* an admin should see what the role allows and on what, not raw tokens and
   empty cells.
5. **Keep the Table | JSON toggle.** *Why:* visual mode for most, JSON for the
   exact policy document.

## 5. Groups list

1. **Name = link on hover; row opens the group.**
2. **`Members` as its own sortable numeric column** (mirrors Users/Groups on the
   Roles list). *Why:* sort by group size to find large or empty groups.
3. **`Roles` column shows role names as tags** (with "+N"), not a count. *Why:*
   shows what the group grants from the list.
4. **Minimal columns:** `Group · Members · Roles · Created (· Updated)`.
5. **"Add group" → simplified create** (single name + auto identifier).

## 6. Group detail page

1. **Remove the "Summary" heading** — name + card directly.
2. **Summary card on the Attribute Field component:** Name (editable, pencil on
   hover), Resource ID (copy), Created At, Updated At — actions on hover, not a
   permanent "Edit" link.
3. **Remove extra copy:** the "N members · grants 1 role" subtitle, the "Everyone
   in this group automatically gets the roles…" alert, and the "Roles granted" /
   "Members" section headings. *Why:* it's obvious from the structure; the text
   and alert only add noise.
4. **Two tabs: Users | Roles**, active underlined in brand orange. *Why:* members
   and granted roles are distinct, like on the user page.
5. **Users tab — compact member table** (`User · Email · Status · Last active`),
   names link, rows open the user. *Why:* in a group you care who's in it and
   their basic status; the full user grid is redundant here.
6. **Roles tab — show access:** `Role · Access` (services + permission count, Deny
   red), not just the role name. *Why:* the point is what the group grants its
   members.
7. **Header actions = one "Actions" menu** (Delete; Rename/Duplicate if needed).

---

*Reference build: the prototype repo (served locally and on Vercel). Styling
tokens — type, spacing, row height, chips — are lower priority than the
structural points above.*
