# Stock-Audit-Automation-With-Odoo-Data

## 📦 Odoo Inventory Forensic Audit Notebook

## Overview

This notebook reconstructs inventory truth from exported Odoo data.

It is designed for retail businesses using Odoo ERP that need:

- Stock reconciliation  
- Pilferage detection  
- Internal transfer leakage analysis  
- After-hours adjustment detection  
- Serial-level anomaly tracking  

Instead of relying on Odoo’s UI alone, this tool rebuilds stock logic externally using exported movement logs.

The result is a prioritized investigation list instead of manually reviewing thousands of rows.

---

## The Problem It Solves

ERP systems provide visibility.

They do not automatically provide forensic clarity.

Common issues addressed:

- Stock counts not matching expected levels  
- Adjustments happening outside business hours  
- Internal transfers not returning  
- Serial numbers disappearing  
- Ghost SKUs inflating inventory  
- High-value shrinkage buried inside large datasets  

This notebook reduces review scope from tens of thousands of rows to a small, risk-ranked subset.

---

## Required Input Files

The notebook uses **two exported Excel files from Odoo**.

---

### 1️⃣ Products List

**Required Columns:**

| Column Name | Description |
|-------------|-------------|
| Name | Product name |
| Sales Price | Selling price |
| Cost | Cost per unit |
| Quantity On Hand | Current stock in Odoo |
| Forecasted Quantity | Odoo forecast |

---

### 2️⃣ Product Moves

**Required Columns:**

| Column Name | Description |
|-------------|-------------|
| Date | Movement timestamp |
| Reference | Movement reference (e.g. WH/IN/00001) |
| Product | Product name |
| Lot/Serial Number | Serial number (if applicable) |
| From | Source location |
| To | Destination location |
| Quantity | Quantity moved |
| Status | Must be "Done" |

Only movements where `Status = Done` are used.

---

## How It Works

### 1️⃣ Reconstructs Stock Ledger

Movement logic:

- If `To = SHOP/Stock` → quantity is added  
- If `From = SHOP/Stock` → quantity is subtracted  

This rebuilds theoretical stock independently of Odoo’s displayed quantity.

---

### 2️⃣ Calculates Variance


Flags:

- Missing items  
- Overstated inventory  
- High financial exposure  

---

### 3️⃣ Detects After-Hours Adjustments

Flags movements:

- Outside defined business hours  
- Especially negative movements from the retail location  

---

### 4️⃣ Detects Internal Transfer Leakage

Identifies products that:

- Frequently leave SHOP  
- Rarely return  
- Create one-way transfer imbalances  

---

### 5️⃣ Serial-Level Tracking

Detects:

- Serials leaving but not returning  
- Serials moved unusually often  
- Serial movement irregularities  

---

## Outputs Generated

The notebook produces:

- `Full_Stock_Audit.xlsx`  
- `Missing_Items.xlsx`  
- `Variance_Issues.xlsx`  
- `Transfer_Imbalance_Report.xlsx`  
- `After_Hours_Movements.xlsx`  
- `Serial_Anomalies.xlsx`  

Each report prioritizes financial exposure.

---

## Setup Instructions (Google Colab)

1. Upload exports to Google Drive  
2. Update file paths in the notebook  
3. Mount Google Drive  
4. Run cells sequentially  

No API integration required.

---

## Business Impact

This tool:

- Reduces audit scope drastically  
- Identifies high-risk SKUs first  
- Quantifies shrinkage value  
- Enables faster physical verification  
- Improves inventory control discipline  

It transforms reactive stock-taking into structured risk management.

---

## Assumptions

- Movement records are complete  
- Location names are consistent  
- Shop retail location is defined correctly  
- Costs are accurate  

If these assumptions are incorrect, results will reflect data integrity issues.

---

## Limitations

- Does not connect directly to the Odoo database  
- Requires clean exports  
- Cannot detect theft without recorded movements  
- Depends on consistent location naming  

---

## Intended Audience

- Retail operators using Odoo  
- SME owners  
- Inventory controllers  
- ERP consultants  
- Operations managers  

---

## Future Enhancements

Planned improvements:

- Risk scoring model  
- Dashboard visualization  
- Employee-level anomaly detection  
- Automated scheduled reporting  
- Category-based shrinkage thresholds  

---

## Author : Stephen Nderitu Waweru

Built as part of an operational ERP implementation focused on inventory control and shrinkage reduction in retail electronics.
