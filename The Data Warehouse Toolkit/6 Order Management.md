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



Excellent — this is one of the most powerful (and underused) areas of dimensional modeling. A clean **Shipment + Inventory dual-fact design** gives you end-to-end visibility from customer orders all the way down to physical devices moving through warehouses.

I’ll walk you through:

1. **Conceptual architecture (why dual-fact exists)**
    
2. **Shipment Star Schema (event-driven logistics fact)**
    
3. **Inventory Transaction Fact (movement-driven)**
    
4. **Inventory Snapshot Fact (state-driven)**
    
5. **How they reconcile together (critical for trust)**
    
6. **Mobile-device–specific extensions (IMEI / refurb / activation)**
    

All in ASCII, Kimball-correct, and lakehouse-friendly.

---

## 🔹 1. Why Dual-Fact? (Events vs State)

Inventory needs **two truths**:

- **Events** → “What changed?” (Ship, Receive, Transfer, Adjust)
    
- **State** → “What do I have right now?” (On-hand, Available, Reserved)
    

So we model:

- 📦 **Shipment Line Fact** → outbound logistics (customer-facing)
    
- 🔁 **Inventory Transaction Fact** → every stock movement
    
- 📊 **Inventory Snapshot Fact** → daily balance per product × location
    

These three must reconcile mathematically.

---

## 🚚 2. Shipment Line Star Schema (Logistics Event Fact)

**Grain:**

> 1 row per **Order Line × Shipment × Product (or Device)**

Supports partial shipments, split shipments, serialized devices.

```sql
                         +----------------+
                         |   Dim Date     | (ship_date_key, delivery_date_key)
                         +----------------+
                                   |
                                   |
        +-------------+     +-------------------+     +----------------+
        | Dim Carrier |-----|                   |-----| Dim Ship Method|
        +-------------+     |                   |     +----------------+
                            |                   |
+----------------+          |                   |          +------------------+
| Dim Warehouse  |----------|  Fact_Shipment    |----------| Dim Order (DD)   |
+----------------+          |    _Line           |          +------------------+
                            |                   |
+----------------+          |                   |          +------------------+
| Dim Product    |----------|                   |----------| Dim Order Line   |
+----------------+          |                   |          |   (DD)           |
                            |                   |
+----------------+          |                   |          +------------------+
| Dim Device     |----------|                   |----------| Dim Customer     |
| (IMEI/Serial)  |          |                   |          +------------------+
                            |                   |
        +-------------+     |                   |     +----------------+
        | Dim Location|-----|                   |-----| Dim Channel    |
        +-------------+     +-------------------+     +----------------+

```
### Measures (Shipment Fact)

- quantity_shipped
- shipping_cost
- handling_cost
- insured_value
- shipment_weight
- delivery_days (derived)
    

Mobile-specific:

- device_count
    
- high_value_flag
    

---

## 🔁 3. Inventory Transaction Fact (Movement Ledger)

This is the **financial-grade inventory ledger**.

**Grain:**

> 1 row per **Inventory Event × Product (or Device) × Location × Timestamp**

Every movement inserts a row.

Examples:

- Receive PO
    
- Ship customer order
    
- Return received
    
- Transfer DC → Store
    
- Shrink / cycle count adjust
    
- Refurb complete
    

```sql
                         +----------------+
                         |   Dim Date     |
                         +----------------+
                                   |
                                   |
        +----------------+    +---------------------+    +----------------+
        | Dim Location   |----|                     |----| Dim Transaction|
        | (Warehouse)   |    | Fact_Inventory       |    | Type           |
        +----------------+    |   _Transaction       |    +----------------+
                               |                     |
+----------------+             |                     |        +------------------+
| Dim Product    |-------------|                     |--------| Dim Reason       |
+----------------+             |                     |        +------------------+
                               |                     |
+----------------+             |                     |        +------------------+
| Dim Device     |-------------|                     |--------| Dim Condition    |
| (IMEI/Serial)  |             |                     |        +------------------+
                               |                     |
        +----------------+     |                     |     +----------------+
        | Dim Order Line |-----|                     |-----| Dim Shipment    |
        | (optional)    |     +---------------------+     | (optional DD)   |
        +----------------+                                   +----------------+

```
### Measures

Core:

- quantity_delta (+ / -)
    
- unit_cost
    
- extended_cost
    

Optional:

- before_qty (audit)
    
- after_qty (audit)
    

This fact is the **source of truth** for:

- Shrink
    
- Loss
    
- Transfers
    
- Reconciliation with finance
    

---

## 📊 4. Inventory Snapshot Fact (Daily Balance)

This is the **operational & planning workhorse**.

**Grain:**

> 1 row per **Product (or Device) × Location × Day**

Derived nightly from transactions.

```sql
                         +----------------+
                         |   Dim Date     |
                         +----------------+
                                   |
                                   |
        +----------------+    +----------------------+
        | Dim Location   |----|                      |
        | (Warehouse)   |    | Fact_Inventory        |
        +----------------+    |   _Snapshot           |
                               |                      |
+----------------+             |                      |
| Dim Product    |-------------|                      |
+----------------+             |                      |
                               |                      |
+----------------+             |                      |
| Dim Device     |-------------|                      |
| (optional)     |             |                      |
                               |                      |
        +----------------+     |                      |
        | Dim Status     |-----|                      |
        +----------------+     +----------------------+

```
### Measures (Balances)

- on_hand_qty
    
- available_qty
    
- reserved_qty
    
- allocated_qty
    
- in_transit_qty
    
- damaged_qty
    

Mobile-specific:

- activated_qty
    
- refurb_qty
    
- locked_qty (fraud / compliance hold)
    

This drives:

- ATP (Available-to-Promise)
    
- Stock-outs
    
- Aging
    
- Planning
    

---

## 🔗 5. How the Three Facts Reconcile (This is the magic)

This is where elite modeling shows.

### 🔹 Shipment Fact → Inventory Transaction Fact

Every shipment line should generate:

```
Shipment Event
   ↓
Inventory Transaction:
  quantity_delta = -quantity_shipped
  transaction_type = 'SHIP_CUSTOMER'
  order_line_key populated
  shipment_id populated

```

---

### 🔹 Inventory Transactions → Inventory Snapshot

Nightly (or streaming):

```
Yesterday Snapshot
+ Sum(quantity_delta today)
= Today Snapshot
```

Mathematically:

```
on_hand_qty(today) =
  on_hand_qty(yesterday)
+ Σ(receipts)
- Σ(shipments)
+ Σ(returns)
± Σ(adjustments)

```

---

### 🔹 Reconciliation Use Cases

You can now answer:

- “Why is on-hand wrong?” → trace via transactions
- “Which shipment caused shrink?” → join shipment ↔ inventory tx
- “Which IMEI moved where?” → device dimension across all facts

This is **warehouse-grade, audit-safe modeling**.

---

## 📱 6. Mobile Device–Specific Enhancements (High-Value Domain)

This is where your model becomes truly differentiated.

---

### 🔹 Device Dimension (Serial / IMEI)

Strongly recommended.

Attributes:

- imei
- serial_number
- model
- capacity
- color
- generation
- activation_status
- warranty_status

Used in:

- Shipment Fact
- Inventory Transaction Fact
- Inventory Snapshot (optional but powerful)
- Returns
    

---

### 🔹 Device Condition Dimension

Tracks lifecycle:

- New
    
- Open Box
    
- Refurb Grade A / B / C
    
- Damaged
    
- Scrapped
    

Critical for:

- Returns
    
- Refurb
    
- Resale
    

---

### 🔹 Activation Link (Optional Fact)

You can later add:

**Fact_Device_Activation**  
Grain: 1 row per device activation event

This connects:

- Sales
    
- Shipment
    
- Customer
    
- Carrier
    

---

## 🧠 How This Fits Your Lakehouse / Streaming Interests

This architecture maps _perfectly_ to what you were experimenting with:

- **Bronze**
    
    - Raw shipment events
        
    - Raw inventory movement events
        
- **Silver**
    
    - Fact_Shipment_Line
        
    - Fact_Inventory_Transaction
        
- **Gold**
    
    - Fact_Inventory_Snapshot
        
    - Reconciled shipment + inventory marts
        

And with Delta:

- Streaming append to Inventory Transactions
    
- Incremental snapshot builds
    
- SCD2 Device / Product dims
    

---

## 🔮 Next (Highly Recommended)

When you’re ready, the next two pieces complete the picture:

### 🔹 A. Full ASCII Lifecycle Flow Diagram

(Order → Allocate → Ship → Tx → Snapshot → Return → Refurb → Re-enter)

### 🔹 B. Logical Star Schema for:

- Order Line (Revenue Core)
    
- Shipment Line
    
- Inventory Transaction
    
- Inventory Snapshot
    

With:

- Surrogate keys
    
- Role-playing dates
    
- Degenerate dimensions
    
- SCD2 strategy

# End-to-End Order → Shipment → Inventory Lifecycle & Logical Star Schemas

This document contains:

1. An end-to-end lifecycle flow diagram (business + data events)
    
2. Logical star schemas for the four core facts:
    
    - Fact_Order_Line (revenue core)
        
    - Fact_Shipment_Line
        
    - Fact_Inventory_Transaction
        
    - Fact_Inventory_Snapshot
        

Designed for a mobile-device sales domain with serial/IMEI tracking, Kimball-compliant and lakehouse-ready.

---

## 1. End-to-End Lifecycle Flow (Business + Fact Events)

```
Customer Places Order
        |
        v
+--------------------+
|  Order Created     |
|  Fact_Order_Header |
+--------------------+
        |
        v
+--------------------+
| Order Line Booked  |
|  Fact_Order_Line   |  <-- Revenue intent established
+--------------------+
        |
        | (ATP / Reserve)
        v
+--------------------------+
| Inventory Allocated      |
| Fact_Inventory_Transaction|
|  type = ALLOCATE         |
|  qty_delta = 0 (logical) |
+--------------------------+
        |
        |
        v
+--------------------+
| Shipment Created   |
| Fact_Shipment_Line |
+--------------------+
        |
        | (Physical movement)
        v
+--------------------------+
| Ship Inventory Out       |
| Fact_Inventory_Transaction|
|  type = SHIP_CUSTOMER    |
|  qty_delta = -N          |
+--------------------------+
        |
        |
        v
+--------------------+
| Invoice Generated  |
| Fact_Invoice_Line  |
+--------------------+
        |
        |
        v
+--------------------+
| Payment Applied    |
| Fact_Payment       |
+--------------------+
        |
        |
        v
+--------------------------+
| Daily Inventory Balance  |
| Fact_Inventory_Snapshot  |
|  on_hand / available     |
+--------------------------+
        |
        |
        v
(Optional Return Path)
        |
        v
+--------------------+
| Return Received    |
| Fact_Return_Line   |
+--------------------+
        |
        v
+--------------------------+
| Inventory Inbound        |
| Fact_Inventory_Transaction|
|  type = RETURN_RECEIPT   |
|  qty_delta = +N          |
+--------------------------+
        |
        v
+--------------------------+
| Refurb / Regrade         |
| Fact_Inventory_Transaction|
|  type = CONDITION_CHANGE |
+--------------------------+
        |
        v
+--------------------------+
| Snapshot Updated         |
| Fact_Inventory_Snapshot  |
+--------------------------+
```

Key guarantees:

- Every shipment produces an Inventory Transaction
    
- Every day’s Snapshot = yesterday + Σ(transactions)
    
- Serial/IMEI can be followed through every box
    

---

## 2. Logical Star Schema — Core Revenue (Fact_Order_Line)

Grain: 1 row per Order Line (SKU per order)

```
                         +----------------+
                         |   Dim Date     | (order_date_key)
                         +----------------+
                                   |
                                   |
        +----------------+     +---------------------+     +----------------+
        | Dim Customer   |-----|                     |-----| Dim Channel    |
        +----------------+     |   Fact_Order_Line   |     +----------------+
                                |                     |
+----------------+              |                     |         +------------------+
| Dim Product    |--------------|                     |---------| Dim Promotion    |
+----------------+              |                     |         +------------------+
                                |                     |
+----------------+              |                     |         +------------------+
| Dim Device     |--------------|                     |---------| Dim Sales Rep    |
| (optional)     |              |                     |         +------------------+
                                |                     |
        +----------------+      |                     |     +------------------+
        | Dim Order (DD) |------|                     |-----| Dim Order Line   |
        +----------------+      +---------------------+     |   (DD)           |
                                                            +------------------+
```

Measures:

- quantity_ordered
- unit_price
- extended_net_amount
- discount_amount
- tax_amount
    

---

## 3. Logical Star Schema — Shipment Line (Logistics Event)

Grain: 1 row per Order Line × Shipment × Product/Device

```
                         +----------------+
                         |   Dim Date     | (ship_date, delivery_date)
                         +----------------+
                                   |
                                   |
        +----------------+     +---------------------+     +----------------+
        | Dim Carrier    |-----|                     |-----| Dim Ship Method|
        +----------------+     | Fact_Shipment_Line  |     +----------------+
                                |                     |
+----------------+              |                     |         +------------------+
| Dim Warehouse  |--------------|                     |---------| Dim Order (DD)   |
+----------------+              |                     |         +------------------+
                                |                     |
+----------------+              |                     |         +------------------+
| Dim Product    |--------------|                     |---------| Dim Order Line   |
+----------------+              |                     |         |   (DD)           |
                                |                     |         +------------------+
+----------------+              |                     |
| Dim Device     |--------------|                     |---------+------------------+
| (IMEI/Serial)  |              |                     |         | Dim Customer     |
                                |                     |         +------------------+
                                +---------------------+
```

Measures:

- quantity_shipped
- shipping_cost
- handling_cost
- insured_value
    

---

## 4. Logical Star Schema — Inventory Transaction (Movement Ledger)

Grain: 1 row per Inventory Event × Product/Device × Location × Timestamp

```
                         +----------------+
                         |   Dim Date     |
                         +----------------+
                                   |
                                   |
        +----------------+     +---------------------------+     +----------------+
        | Dim Location   |-----|                           |-----| Dim Tx Type    |
        | (Warehouse)   |     | Fact_Inventory_Transaction |     +----------------+
        +----------------+     |                           |
                                |                           |
+----------------+              |                           |        +------------------+
| Dim Product    |--------------|                           |--------| Dim Reason       |
+----------------+              |                           |        +------------------+
                                |                           |
+----------------+              |                           |        +------------------+
| Dim Device     |--------------|                           |--------| Dim Condition    |
| (IMEI/Serial)  |              |                           |        +------------------+
                                |                           |
        +----------------+      |                           |     +------------------+
        | Dim Order Line |------|                           |-----| Dim Shipment    |
        | (optional)    |      +---------------------------+     | (optional DD)   |
        +----------------+                                        +------------------+
```

Measures:

- quantity_delta (+ / -)
    
- unit_cost
    
- extended_cost
    

---

## 5. Logical Star Schema — Inventory Snapshot (Daily Balance)

Grain: 1 row per Product/Device × Location × Day

```
                         +----------------+
                         |   Dim Date     |
                         +----------------+
                                   |
                                   |
        +----------------+     +---------------------------+
        | Dim Location   |-----|                           |
        | (Warehouse)   |     |  Fact_Inventory_Snapshot   |
        +----------------+     |                           |
                                |                           |
+----------------+              |                           |
| Dim Product    |--------------|                           |
+----------------+              |                           |
                                |                           |
+----------------+              |                           |
| Dim Device     |--------------|                           |
| (optional)     |              |                           |
                                |                           |
        +----------------+      |                           |
        | Dim Status     |------|                           |
        +----------------+      +---------------------------+
```

Measures (balances):

- on_hand_qty
    
- available_qty
    
- reserved_qty
    
- allocated_qty
    
- in_transit_qty
    
- damaged_qty
    

Mobile-specific:

- activated_qty
    
- refurb_qty
    
- locked_qty
    

---

## 6. Reconciliation Rules (Critical Design Contract)

1. Every Shipment Line MUST generate an Inventory Transaction (SHIP_CUSTOMER)
    
2. Inventory Snapshot MUST equal:
    
    snapshot(t) = snapshot(t-1) + Σ(quantity_delta on day t)
    
3. Device / IMEI may appear in:
    
    - Order Line
        
    - Shipment Line
        
    - Inventory Transaction
        
    - Inventory Snapshot
        

This guarantees:

- Auditability
    
- Shrink detection
    
- Serial traceability
    
- Financial reconciliation
    

---

If you’d like next, we can extend this with:

- Full physical table schemas (columns + keys)
    
- SCD2 strategy for Product / Device / Customer
    
- Medallion (Bronze–Silver–Gold) implementation plan in Delta + dbt
    
- Streaming ingestion pattern for inventory transactions