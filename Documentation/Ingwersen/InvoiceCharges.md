# Combine Lines Add Charge - Report 50101

**Source:** `src/Sales/OrderHandling/CombineLinesAddCharge.Report.al`

This report does **two things** in sequence:

## 1. Combines multiple sales orders into one per customer

If a customer has **several sales orders** scheduled for the **same shipment date**, the report merges them together. It picks the **first order** for each customer and moves all the lines from the other orders into that first order. This avoids shipping multiple small orders separately to the same customer on the same day.

- Only orders **not** marked as "Separate Order" are included (so special orders are left alone).

## 2. Adds a handling charge for small orders

After combining, it looks at customers who are flagged with **"Handling Charge" = true**. For those customers, it checks whether their **total order amount is below a minimum value** you specify. If it is, the report automatically adds a **charge item** (like a surcharge/fee line) to the order.

Think of it as: *"If a customer's order is too small, add a small-order fee."*

## What you fill in when you run it

- **Shipment Date** - Which date's orders to process (must be today or later)
- **Limit Amount** - The minimum order value; orders below this get a fee added
- **Item No. for Charge** - Which item number represents the handling fee (e.g., a "Small Order Surcharge" item)

## In short

> "Take all of today's orders, merge multiple orders per customer into one, and if the combined order is still too small, slap on a handling fee."
