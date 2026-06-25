# Identity & Access — summary of changes and rationale

The prototype (on localhost) is the target direction. This document lists the
changes needed to evolve the current production build, and **why** each one
matters. Visual details (typography, paddings, row heights) are secondary.

## User detail page

### 1. A cleaner summary card built on the Attribute Field component
**Change:** rebuild the summary as one Card using the **Attribute Field**
component — label + value with the relevant action inline (Edit on name and
email, Copy on the ID). Group related fields on the same row (User ID next to
Email; Identity provider together with Created / Updated / Last sign-in) and move
Status up next to the name.
**Why:** the production card spreads a handful of fields over a lot of vertical
space, with lonely rows (e.g. "Last sign-in —" on its own line) and no clear
actions. One consistent attribute component makes the card compact and scannable
and puts actions exactly where the user expects them, instead of having to hunt.

### 2. Surface roles and groups directly
**Change:** show the user's roles and groups right on the page.
**Why:** so you can see at a glance what the user belongs to and where their
access comes from — no extra navigation needed.

### 3. Lead with an "Access" tab — what the user can actually do
**Change:** add an **Access** view (tabs become **Access | Roles | Groups**) that
resolves all roles and groups into one picture — grouped by service, with
Allow/Deny and an access level (View / Edit / Admin). It's the first thing on the
page.
**Why:** when you open a user, the first thing you want to know is what exactly
they have access to. The production page only shows tables of *attached*
policies, so you still have to go too deep — open each policy and read its
statements — to understand what the user can and can't do. The Access view
answers that up front.

## Users list

### 4. Show role and group names, not counts
**Change:** replace the "1 / 1" count columns with the actual group and role
names as tags, collapsing extras into a "+N" chip when they don't fit.
**Why:** a count tells you *how many*, not *which*. Names let you understand a
user's access straight from the list, without opening each one.

## Roles & Groups tabs

### 5. Show the access a role grants, not its internals
**Change:** in the Roles tab, replace technical columns ("Statements: 1",
"Type: Managed / Default") with a readable access summary (which services, at
what level). Keep "Attached via" (Direct / via &lt;group&gt;).
**Why:** "1 statement" or "Managed/Default" doesn't tell an admin what the role
actually grants. The point of the row is the access it gives.

### 6. One consistent vocabulary
**Change:** pick one model and name it consistently (a Role with its policy).
**Why:** the current page mixes "Policy name", "Type", "Managed/Default" and
roles — two rows both named "Admin" with different types are impossible to tell
apart.

## Functional gaps

### 7. Roles and groups must open
**Why:** they aren't clickable today, so you can't drill into a role or group to
see or change it.

### 8. Create role / create group must work
**Change:** build both the simple way shown in the prototype — a single name
field with an auto-generated identifier, and an **AWS IAM-style policy editor**
(service → actions grouped by access level → resources → Allow/Deny, with a
Visual / JSON toggle).
**Why:** both flows are currently non-functional; and the editor should match IAM
because the audience is DevOps, who already know that model.

## Across the board

### 9. Single source of truth
**Change:** derive the Access view and the Roles / Groups tabs from the same
data.
**Why:** the effective access shown should never contradict the list of what's
attached to the user.

---

*Lower priority: table styling — one text style, consistent paddings and row
heights, and chips that don't change row height.*
