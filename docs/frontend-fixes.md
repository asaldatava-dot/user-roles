# Frontend fixes — backlog (next iteration)

Captured design feedback to apply later. Not yet implemented.

## Users table

### 1. Column widths — stop the user name wrapping to two lines
The user name currently wraps to 2 lines because Email is too narrow. Rebalance:
- **Email**: give it more room (~15% of table width).
- **Status**: narrower.
- **Last active**: narrower.
- Recalculate the remaining widths so the **User name stays on one line**.

### 2. User-name cell text (the link)
Currently 16px blue link. Should be:
- **14px** (not 16px).
- **Default:** primary text color — **not** blue link color.
- **Hover:** switch to link style — correct blue + underline.
(Applies to clickable name cells in tables generally.)

### 3. Bulk action bar position
When a row is selected, the action bar ("N selected · Select all · Clear · Remove")
currently appears in a random spot over the table.
- **Pin it to the bottom of the page** (sticky, centered floating bar).

## Notes / kept as-is
- The `← →` arrows in the table header are correct — fixed first column + horizontal
  scroll for wide tables. Keep.
- The `Auth` column showing `—` is a placeholder; real data is coming. Keep the column.
