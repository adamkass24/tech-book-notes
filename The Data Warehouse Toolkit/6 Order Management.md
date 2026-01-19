Order Management Bus Matrix
Order Transactions
	Fact Normalization
	Dimension Role Playing
	Product Dimension Revisited
	Customer Dimension
	Deal Dimension
	Degenerate Dimension for Order Number
	Junk Dimensions
	Header/Line Pattern to Avoid
	Multiple Currencies
	Transaction Facts at Different Granularity
	Another Header/Line Pattern to Avoid
Invoice Transactions
	Service Level Performance as Facts, Dimensions, or Both
	Profit and Loss Facts
	Audit Dimension
Accumulating Snapshot for Order Fulfillment Pipeline
	Lag Calculations
	Multiple Units of Measure
	Beyond the Rearview Mirror
Summary


### 🚌 Order Management Bus Matrix (Dimensional Modeling)

```

+----------------------+-------------+-------------+-------------+-------------+-------------+-------------+
| Dimension / Process  | Order Entry | Order Line  | Shipment    | Invoice     | Payment     | Returns     |
+----------------------+-------------+-------------+-------------+-------------+-------------+-------------+
| Date                 |     X       |     X       |     X       |     X       |     X       |     X       |
| Time                 |     X       |     X       |             |             |     X       |             |
| Customer             |     X       |     X       |     X       |     X       |     X       |     X       |
| Product              |             |     X       |     X       |     X       |             |     X       |
| Order                |     X       |     X       |     X       |     X       |     X       |     X       |
| Order Line           |             |     X       |     X       |     X       |             |     X       |
| Sales Channel        |     X       |     X       |     X       |     X       |     X       |     X       |
| Sales Rep            |     X       |     X       |             |     X       |             |             |
| Promotion            |             |     X       |             |     X       |             |     X       |
| Currency             |     X       |     X       |     X       |     X       |     X       |     X       |
| Ship Method          |             |             |     X       |             |             |             |
| Warehouse / Location |             |             |     X       |             |             |     X       |
| Payment Method       |             |             |             |             |     X       |             |
| Return Reason        |             |             |             |             |             |     X       |
| Status (Order/Line)  |     X       |     X       |     X       |     X       |     X       |     X       |
+----------------------+-------------+-------------+-------------+-------------+-------------+-------------+
```


### 🧾 Fact Tables (Processes Across the Top)

Typical grain for each:

- **Order Entry Fact** – 1 row per order header (order placed)
- **Order Line Fact** – 1 row per order line item (most important revenue fact)
- **Shipment Fact** – 1 row per shipped package or line
- **Invoice Fact** – 1 row per invoice or invoice line
- **Payment Fact** – 1 row per payment transaction
- **Returns Fact** – 1 row per returned line item

---

### 🔑 Conformed Dimensions (Down the Side)

These are shared across multiple facts:

- **Date / Time** – role-playing (order date, ship date, invoice date, payment date, return date)
- **Customer** – conformed across all revenue lifecycle
- **Product** – shared between sales, shipments, invoices, returns
- **Order / Order Line** – degenerate or mini-dimension often
- **Sales Channel** – web, store, phone, marketplace
- **Currency** – critical for financial reconciliation
- **Status** – order status, line status, shipment status, etc.

🧱 Conformed Dimensions Matrix

```
+----------------------+--------------+------------+---------------+--------------+-----------+--------------+--------------------+
| Dimension            | Order Header | Order Line | Shipment Line | Invoice Line | Payment   | Return Line  | Inventory Snapshot |
+----------------------+--------------+------------+---------------+--------------+-----------+--------------+--------------------+
| Date (role-playing)  |      X       |     X      |      X        |      X       |     X     |      X       |         X          |
| Time                 |      X       |     X      |               |              |     X     |              |                    |
| Customer             |      X       |     X      |      X        |      X       |     X     |      X       |                    |
| Product (Device/SKU) |              |     X      |      X        |      X       |           |      X       |         X          |
| Device (IMEI/Serial) |              |     X*     |      X        |      X       |           |      X       |         X*         |
| Order (DD)           |      X       |     X      |      X        |      X       |     X     |      X       |                    |
| Order Line (DD)      |              |     X      |      X        |      X       |           |      X       |                    |
| Sales Channel        |      X       |     X      |      X        |      X       |     X     |      X       |                    |
| Store / Location     |      X       |     X      |      X        |      X       |     X     |      X       |         X          |
| Warehouse / DC       |              |            |      X        |              |           |      X       |         X          |
| Carrier              |              |            |      X        |              |           |              |                    |
| Ship Method          |              |            |      X        |              |           |              |                    |
| Promotion            |              |     X      |               |      X       |           |      X       |                    |
| Sales Rep            |      X       |     X      |               |      X       |           |              |                    |
| Currency             |      X       |     X      |      X        |      X       |     X     |      X       |         X          |
| Payment Method       |              |            |               |              |     X     |              |                    |
| Return Reason        |              |            |               |              |           |      X       |                    |
| Inventory Status     |              |            |               |              |           |              |         X          |
| Device Condition     |              |     X      |      X        |              |           |      X       |         X          |
| Status (Order/Line)  |      X       |     X      |      X        |      X       |     X     |      X       |                    |
+----------------------+--------------+------------+---------------+--------------+-----------+--------------+--------------------+
```

Notes (important for mobile devices):

- **Product dimension** = SKU / model / capacity / color / generation
- **Device dimension** (optional but powerful) = IMEI / serial-level tracking
    - This becomes critical for:
        - Shipments
        - Returns
        - Refurb / condition tracking
        - Inventory on hand

You can model **Product** and **Device** as:

- Product = standard dimension
- Device = **outrigger / mini-dimension** or separate serial-grain dimension

## 📌 Fact Table Definitions (Grain + Measures)

This is the “full Kimball” part.

---

### 1️⃣ Fact: Order Header Fact

**Grain:** 1 row per order (per order creation event)

**Measures:**

- order_gross_amount
    
- order_net_amount
    
- order_tax_amount
    
- order_discount_amount
    
- order_line_count
    

---

### 2️⃣ Fact: Order Line Fact (CORE REVENUE FACT)

**Grain:** 1 row per order line item (SKU per order)

**Measures:**

- quantity_ordered
    
- unit_list_price
    
- unit_net_price
    
- extended_gross_amount
    
- extended_net_amount
    
- discount_amount
    
- tax_amount
    

Mobile-specific:

- subsidy_amount (if carrier subsidized)
    
- activation_fee
    

---

### 3️⃣ Fact: Shipment Line Fact

**Grain:** 1 row per shipped order line per shipment (partial shipments allowed)

**Measures:**

- quantity_shipped
    
- shipping_cost
    
- handling_cost
    
- shipment_weight
    
- delivery_days (derived)
    

Mobile-specific:

- device_count
    
- insured_value
    

Key insight: this fact links **sales → logistics → inventory movement**

---

### 4️⃣ Fact: Invoice Line Fact

**Grain:** 1 row per invoice line (billing event)

**Measures:**

- invoiced_quantity
    
- invoice_gross_amount
    
- invoice_net_amount
    
- tax_amount
    
- balance_due
    

Used for:

- Revenue recognition
    
- AR reconciliation
    

---

### 5️⃣ Fact: Payment Fact

**Grain:** 1 row per payment transaction

**Measures:**

- payment_amount
    
- applied_amount
    
- unapplied_amount
    
- refund_amount
    

---

### 6️⃣ Fact: Return Line Fact

**Grain:** 1 row per returned order line (or per device if serialized)

**Measures:**

- quantity_returned
    
- return_amount
    
- restocking_fee
    
- write_off_amount
    

Mobile-specific:

- refurb_cost
    
- resale_value
    

---

### 7️⃣ Fact: Inventory Snapshot Fact (VERY IMPORTANT)

This is where deep inventory modeling starts.

**Grain:**

> 1 row per **Product (or Device)** × **Warehouse/Location** × **Day**

**Measures:**

- on_hand_qty
    
- available_qty
    
- reserved_qty
    
- damaged_qty
    
- in_transit_qty
    
- allocated_qty
    

Mobile-specific:

- activated_qty
    
- refurb_qty
    
- locked_qty (fraud / compliance hold)
    

This fact is used for:

- Inventory aging
    
- Stock-outs
    
- Supply planning
    
- Shrink / loss detection
    

---

## 🔄 Inventory Movement (Optional but powerful)

If you want full fidelity, add:

### 8️⃣ Fact: Inventory Transaction Fact

**Grain:** 1 row per inventory movement event

Examples:

- Receive PO
    
- Ship order
    
- Return receive
    
- Transfer between DCs
    
- Adjust shrink
    

**Measures:**

- quantity_delta (+ / -)
    
- transaction_cost
    
- unit_cost
    

This pairs beautifully with snapshots.

---

## 📣 Reminder for Next Steps (as requested)

Here’s what I recommend we do next — and I’ll remember to bring these up if you continue this thread:

### 🔹 1. Logical Star Schema (Order Line core)

We’ll design:

- Fact_Order_Line
    
- Dim_Product
    
- Dim_Device (serial-level)
    
- Dim_Customer
    
- Dim_Date (role-playing)
    
- Degenerate dims (order_id, order_line_id)
    

Including:

- Surrogate keys
    
- SCD2 strategy (very relevant to your interests)
    

---

### 🔹 2. Lifecycle Flow Diagram

We’ll map:

`Order Created      ↓ Order Line Booked      ↓ Inventory Allocated      ↓ Shipment Created      ↓ Device Shipped      ↓ Invoice Generated      ↓ Payment Applied      ↓ Return / Refurb (optional)      ↓ Inventory Re-entered or Written Off`

With:

- Which fact fires at each step
    
- Which dimensions are reused
    
- How conformed keys propagate