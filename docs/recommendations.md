# Identity & Access — recommended changes

The prototype (on localhost) shows the intended direction — an approximate
target for how this should work and look. Below are the **conceptual** changes
to bring the current production build closer to it. Visual details (typography,
paddings, row heights) are secondary; the points below are about structure and
meaning.

## 1. Add an "Access" view — what the user can actually do
Today the detail page only lists the policies *attached* to a user. It never
shows the user's **effective access**. Add an **Access** tab that resolves all
roles and groups into one view: grouped by product/service, with Allow/Deny and
an access level (View / Edit / Admin). Tabs become **Access | Roles | Groups**.
This answers the real question — "what can this person do?" — not just "what is
attached to them?".

## 2. Show meaning, not raw counts and internals
- **Users list:** the Groups and Roles columns show only counts ("1", "1").
  Show the actual **names** as tags, collapsing extras into a "+N" chip when they
  don't fit. A bare count hides what the access actually is.
- **Roles tab:** replace technical columns like "Statements: 1" and
  "Type: Managed / Default" with a readable **access summary** (which services,
  at what level). Keep "Attached via" (Direct / via &lt;group&gt;) — that part is good.

## 3. One vocabulary
The detail page mixes "Policy name", "Type: Managed/Default", and roles. Two rows
both called "Admin" with different types is confusing — the user can't tell what
differs. Pick one model and name it consistently (a Role, with its policy), so
identical-looking rows aren't ambiguous.

## 4. Make roles and groups open
Roles and groups — and the "Attached via" group links — are not clickable today.
Every role and group should open its own detail page.

## 5. Implement create role / create group
Both flows are currently non-functional. Build them the simplified way shown in
the prototype: a single name field with an auto-generated identifier, and an
**AWS IAM-style policy editor** (service → actions grouped by access level →
resources → Allow/Deny, with a Visual / JSON toggle). Don't reinvent IAM — most
users are DevOps and already know that model.

## 6. Tighten the summary card
Group related attributes on the same row and avoid lonely rows (e.g. "Last
sign-in —" sitting on its own line). Put **User ID next to Email**, and
**Identity provider on the same row** as Created / Updated / Last sign-in. Move
**Status up next to the name**. The card becomes compact and scannable.

## 7. Consistent status and labels
Align status wording and labels across list and detail (e.g. "Enabled" vs
"Active" — pick one).

## 8. Single source of truth
The Access view and the Roles / Groups tabs should be derived from the same
data, so the effective access can never contradict the list of what's attached.

---

*Lower priority (already detailed elsewhere): table styling — one text style,
consistent paddings and row heights, and chips that don't change row height.*
