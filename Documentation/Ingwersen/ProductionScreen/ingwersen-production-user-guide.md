# Ingwersen Production — User Guide

Page 50118 "Ingwersen Production" is the daily production planning worksheet. It shows all production items (Gen. Prod. Posting Group = `PROD`), their sales order demand, current inventory, and assembly order coverage. From this single screen, production staff can see what needs to be produced, create assembly orders, and print product labels.

The page remembers your filter settings between sessions (`SaveValues = true`).

---

## Filters

The filter bar at the top controls which items are displayed and how related pages behave.

| Filter | Description |
|--------|-------------|
| **Date Filter for Shipment Date** | Sets the production date. Defaults to **today** when the page opens. Supports BC date tokens (e.g. `t` for today, `w` for work date). Internally applies an open-ended filter (`date..`) on the Item's Date Filter, so all demand from this date onward is included. |
| **Item No. Filter** | Filters by item number. Use the lookup button to open the Item List and select one or multiple items. |
| **Item Description Filter** | Free-text search on the item description. Multiple words are split by spaces and combined with AND logic — all words must appear in the description. |
| **Only with Quantity in Sales Orders** | When enabled, hides items that have zero quantity on sales orders for the selected date range. Useful to focus only on items that need production. |
| **Production Location** | Filters inventory calculations to a specific warehouse location. When set, all inventory-related FlowFields (Inventory, Qty. on Sales Order, etc.) are calculated for that location only. |
| **Shortage View Mode** | Controls how the BOM Shortage page (opened via the BOM Shortage action) displays results: **Summary** groups shortages by component, **Per Parent Item** shows detailed breakdown per parent production item. |

---

## Production Items Grid

The main repeater shows one row per production item. All quantity fields respect the Date Filter and Location Filter.

| Column | Description |
|--------|-------------|
| **No.** | The item number. Read-only. |
| **Description** | The item description. Read-only. Click the **assist-edit** button (the `...` icon) to open the BOM Component list for this item — a quick way to view ingredients. |
| **Inventory** | Current stock on hand. Read-only, greyed out. |
| **Qty. on Sales Order** | Total quantity on open sales orders for the filtered date range. Read-only. |
| **Qty. on Assembly Order** | Quantity already committed to assembly orders (i.e. production already planned). Read-only. |
| **Labels Printed** | Shows how many labels have been printed (derived from a stored count). Displayed in red. Read-only. |
| **Remaining** | Calculated: `Qty. on Sales Order - (Inventory + Qty. on Assembly Order)`, minimum 0. This is the quantity that still needs to be produced. Displayed in **bold**. |
| **DONE** | Editable input. Enter a quantity and press Enter or Tab to create an Assembly Order. See [Creating Assembly Orders](#creating-assembly-orders-done-column) below. |
| **LABELS** | Editable input. Enter a quantity and press Enter or Tab to print labels. Click the assist-edit button to view the Labels Printed log for this item. See [Printing Labels from the Item Row](#printing-labels-from-the-item-row) below. |

---

## Creating Assembly Orders (DONE Column)

The DONE column is the primary way to record production. When you enter a quantity:

1. A new **Assembly Order** is created for the current item
2. The order quantity is set to the number you entered
3. The **Posting Date** and **Due Date** are both set to the date shown in the Date Filter
4. BC automatically generates **Assembly Lines** from the item's Assembly BOM (bill of materials)
5. All assembly line due dates are updated to match the header
6. The DONE field resets to **0** and the page refreshes
7. The **Qty. on Assembly Order** column updates to reflect the new order, and **Remaining** decreases accordingly

### Example

If an item has Qty. on Sales Order = 50, Inventory = 10, and Qty. on Assembly Order = 0:
- Remaining shows **40**
- You enter **40** in DONE
- An Assembly Order for 40 is created
- Remaining drops to **0**

---

## Printing Labels from the Item Row

The LABELS column in the item grid prints labels at the **item level** (not per sales line). When you enter a quantity:

1. The system checks the item's **Manufacturer Code** for the letter `T`
   - If `T` is found: the **Item Label Small Transparent** report runs
   - Otherwise: the standard **Item Label** report runs
2. A log entry is written to the [Labels Printed Log](#labels-printed-log) with `From Item = true`
3. The LABELS field resets to **0**

---

## Sales Lines Subpage (Bottom Section)

Below the item grid, a subpage shows all **Sales Order Lines** for the currently selected item, filtered by the Date Filter. This gives you visibility into which customers need this product and how much.

### Subpage Columns

| Column | Description |
|--------|-------------|
| **Route/Stop** | The delivery route code for this order. Read-only. |
| **Order No.** | The sales order document number. Read-only. |
| **Sell-to Customer** | The customer number. Read-only. |
| **Customer Name** | The customer name (looked up from the Customer table). Read-only. |
| **Description** | The sales line description. Read-only. |
| **Net Weight** | Net weight of the item on this line. Read-only. |
| **Unit of Measure** | The unit of measure. Read-only. |
| **Qty.** | The outstanding (not yet shipped) quantity. Read-only. |
| **Shipment Date** | The planned shipment date. Read-only. |
| **Printed** | Shows the date when labels were printed for this sales line. When labels are printed, this is set to today's date. When blank, no labels have been printed yet. |
| **LABELS** | Editable input. Enter a quantity to print labels for this specific sales line. See below. |

Lines with a shipment date **later than the work date** are displayed greyed out (subordinate style) to visually distinguish future orders from today's orders.

### Printing Labels from a Sales Line

When you enter a quantity in the sales line LABELS column:

1. The system checks the item's **Manufacturer Code** for the letter `T`
   - If `T` found: runs the **Item Label Small Transparent** report (includes customer number and name)
   - Otherwise: runs the standard **Item Label** report (includes customer number and name)
2. The **Printed** column is set to **today's date**, marking this line as "labels printed"
3. A log entry is written to the [Labels Printed Log](#labels-printed-log) with `From Item = false`
4. The LABELS field resets to **0**

**To clear the "printed" status**: Enter **0** in the LABELS column. This removes the date from the Printed column.

### Difference: Item Labels vs Sales Line Labels

| | Item Row LABELS | Sales Line LABELS |
|---|---|---|
| **Scope** | Prints for the item in general | Prints for a specific customer's order line |
| **Customer info** | Not included on the label | Customer number and name printed on the label |
| **Printed indicator** | No indicator on the page | Sets the "Printed" date on the sales line |
| **Log entry** | `From Item = true` | `From Item = false` |

---

## Actions

### Promoted Actions (Action Bar)

| Action | Shortcut | Description |
|--------|----------|-------------|
| **Date Back** | `Ctrl+PgUp` | Moves the Date Filter back by one day. |
| **Date Forward** | `Ctrl+PgDn` | Moves the Date Filter forward by one day. |
| **BOM Shortage** | `Ctrl+Shift+B` | Opens the BOM Component Shortage page. This calculates which raw materials and components have insufficient inventory to produce the remaining quantities across all visible items. The display mode is controlled by the Shortage View Mode filter. |
| **Create Movement** | — | Creates an **Internal Movement** document to physically move components from storage bins to the production/assembly bin. Shows a confirmation dialog first. If any components cannot be fully sourced from available bins, the movement is opened and shortage warnings are displayed. |
| **Assembly Orders** | `Ctrl+K` | Opens assembly orders for the current item on the current date. If exactly **one** order exists, it opens the Assembly Order card directly. If multiple exist, it opens the Assembly Orders list. |

### Navigation Actions

| Action | Shortcut | Description |
|--------|----------|-------------|
| **Item - edit** | `Ctrl+F4` | Opens the Item Card for the selected item. |
| **Item Ledger Entries** | `Ctrl+F7` | Opens the item ledger entries for the selected item, sorted by posting date (newest first). |
| **Ingredients** | `F8` | Opens the BOM Component list showing all ingredients/components for the selected item. |
| **Internal Movements** | — | Opens the Internal Movement List page (all movements, not filtered). |

---

## Labels Printed Log

Every label print — whether from the item row or a sales line — is logged in the **Labels Printed** table (50108). Each entry records:

| Field | Description |
|-------|-------------|
| **Entry No.** | Auto-incrementing sequence number. |
| **Prod. Date** | The production/shipment date at the time of printing. |
| **Item No.** | The item for which labels were printed. |
| **Qty. Printed** | The number of labels printed. |
| **From Item** | `true` = printed from the item row (header LABELS column). `false` = printed from a sales line. |

To view the log: click the **assist-edit** button on the LABELS field in the item grid row. This opens the Labels Printed page filtered to the current item and date.

---

## Typical Daily Workflow

1. **Open the page** — it defaults to today's date and shows all production items
2. **Review the Remaining column** — items with values > 0 need production
3. **Check BOM Shortage** (`Ctrl+Shift+B`) — verify you have enough raw materials
4. **Create Movement** — move needed components to the production bin
5. **Enter quantities in DONE** — as items are produced, enter the quantity to create Assembly Orders
6. **Print labels** — use the LABELS column (either at item level or per sales line) to print product labels
7. **Navigate dates** — use `Ctrl+PgUp` / `Ctrl+PgDn` to check previous or upcoming days

---

## Keyboard Shortcuts Reference

| Shortcut | Action |
|----------|--------|
| `Ctrl+PgUp` | Date back one day |
| `Ctrl+PgDn` | Date forward one day |
| `Ctrl+Shift+B` | Open BOM Shortage |
| `Ctrl+K` | Open Assembly Orders for current item |
| `Ctrl+F4` | Open Item Card |
| `Ctrl+F7` | Open Item Ledger Entries |
| `F8` | Open BOM Components (Ingredients) |

---

## How Shortage Quantities are Calculated

Both the **BOM Shortage** page and the **Create Movement** action start from the same production quantity, but they determine shortages differently.

### Step 1: Production Quantity (shared by both)

For each production item, the system calculates the **Remaining** quantity — how much still needs to be produced:

```
Remaining = Qty. on Sales Order - (Inventory + Qty. on Assembly Order)
```

- **Qty. on Sales Order**: total demand from open sales orders (from the date filter onward)
- **Inventory**: current stock on hand
- **Qty. on Assembly Order**: quantity already committed to assembly orders (production already planned)

Only items where Remaining > 0 are processed further. This formula is identical in both code paths.

### Step 2: BOM Explosion (shared by both)

For each item with Remaining > 0, the system "explodes" the Assembly BOM — it walks through all components and sub-components recursively:

- For each component: `Required Qty = Remaining x Qty. per (accumulated through BOM levels)`
- If a component itself has an Assembly BOM, the explosion continues into sub-components
- Circular references are detected and prevented

### Step 3a: BOM Shortage — Component Inventory Check

The BOM Shortage page checks component shortages against **Item Inventory** (the total stock of each component at the filtered location):

1. **Aggregate demand**: All required quantities for the same component across all parent items are summed. For example, if Parent A needs 10 and Parent B needs 15 of Component X, the total demand is 25.
2. **Check inventory once**: The component's inventory is fetched once (respecting the Production Location filter).
3. **Calculate shortage**: `Shortage = Total Required - Inventory`. If inventory is 12 and total demand is 25, the shortage is 13.
4. **Distribute proportionally**: The shortage is distributed across parent items proportionally to their demand. Parent A gets `10/25 x 13 = 5.2` shortage, Parent B gets `15/25 x 13 = 7.8` shortage.
5. **No shortage = no entry**: If a component has enough inventory to cover all parents, it does not appear in the list at all.

The **Summary** view groups by component and shows the total shortage. The **Per Parent Item** view shows the proportional distribution.

### Step 3b: Create Movement — Bin Content Check

The Create Movement action checks component availability against **Bin Content** — the actual physical stock in warehouse bins:

1. **Aggregate demand**: Same as BOM Shortage — all required quantities for the same component are summed across parents.
2. **Check bin content**: For each component, the system looks at all bins in the production location (excluding the destination bin) and calculates available quantity: `Available = Quantity (Base) - Pick Quantity (Base) - Negative Adjmt. Qty. (Base)`.
3. **Create movement lines**: Movement lines are created from each bin that has available stock, until the required quantity is fulfilled.
4. **Shortage = unfulfilled demand**: If all bins together cannot provide enough, the remaining unfulfilled quantity is reported as a shortage.

### Why the Numbers Can Differ

| | BOM Shortage | Create Movement |
|---|---|---|
| **Checks against** | Item Inventory (total stock at location) | Bin Content (stock in specific bins, excluding destination bin) |
| **Available means** | Total inventory at the filtered location | Physically pickable quantity across bins |
| **Can differ because** | Inventory includes stock in the destination bin and reserved stock | Only counts stock in bins other than the production bin, minus picks already in progress |

In practice, the BOM Shortage page may show **fewer** shortages than Create Movement if component inventory exists in the destination bin (which is excluded from movement sourcing). Conversely, BOM Shortage may show **more** shortages if bin-level availability differs from total inventory.

---

## Technical Notes

The page repurposes several standard BC fields for custom purposes:

- **Item."Maximum Inventory"** — stores a label print count that is displayed (rounded up) in the "Labels Printed" column
- **SalesLine."Promised Delivery Date"** — used as a "labels printed" flag; set to today's date when labels are printed for a sales line
- **SalesLine."Qty. to Invoice"** — temporarily used during label printing to pass the label count to the report
