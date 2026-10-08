# Excel Insurance Analytics: One Dataset, Three Cleansing Approaches

Cleansing and analysing a messy synthetic insurance dataset three ways in Excel (**formulas**, **Power Query** and **Power Pivot**), then building an insurance KPI dashboard on the cleaned data.

> **Status:** work in progress. See the [roadmap](#roadmap) for what's done.

---

## Why this project

Real insurance data is messy: inconsistent labels, text-stored dates, duplicate policies, orphan claims and broken business rules. Excel offers several ways to fix it, and each has trade-offs in effort, repeatability, scalability and auditability. This project runs the **same dataset through all three approaches** and compares them, so the choice of tool becomes a decision rather than a habit.

## The dataset

Fully **synthetic** (no real customer data), modelled on an Indian general and life insurer. Amounts are in INR and the extract date is 08-Oct-2026.

| Table | Rows (raw) | Grain | Key |
|---|---|---|---|
| `Policies_Raw` | 2,070 | One policy sold | PolicyNo |
| `Claims_Raw` | 715 | One claim filed | ClaimNo (FK: PolicyNo) |
| `Agents_Raw` | 30 | One sales agent | AgentCode |

Products: Motor, Health, Home, Life, Travel. Channels: Agent, Online, Bancassurance, Broker.

### Data-quality problems injected

- Text: wrong case, leading, trailing and double spaces, non-breaking spaces (CHAR 160), titles in names, "Last, First" order
- Dates: mixed real and text dates in several formats, plus impossible dates (end before start, claim before policy start, invalid DOB)
- Amounts: currency symbols and text numbers (`₹21,60,000`, `Rs. 500000`, `5,25,800/-`), negative, zero and missing premiums
- Categories: many spellings for Product, Channel, City, State, Gender, PaymentMode and Status
- Referential integrity: orphan agent codes, claims with no matching policy, malformed PolicyNo
- Duplicates in both Policies and Claims, including near-duplicates with altered keys
- Business-rule breaks: approved amount greater than claimed, amounts on rejected claims, "Active" policies already expired

## Approach

| | Phase 1: Formulas | Phase 2: Power Query | Phase 3: Power Pivot |
|---|---|---|---|
| Where cleansing happens | Helper columns in the sheet | Applied steps in the Query Editor (M) | Data Model with DAX |
| Key tools | `TRIM`, `CLEAN`, `SUBSTITUTE`, `PROPER`, `DATEVALUE`, `INDEX/MATCH`, `COUNTIFS`, `IFERROR` | Transform steps, Merge, Remove Duplicates, Replace Values, parameters | Relationships, star schema, measures, calendar table |
| Refreshable | Manual | One click | One click |
| Auditability | Visible per cell | Visible per step | Visible in model and measures |
| Verdict | *(to be added)* | *(to be added)* | *(to be added)* |

## Target data model

Star schema built for the dashboard:

- **Facts:** `FactPolicy`, `FactClaim`
- **Dimensions:** `DimCustomer`, `DimAgent`, `DimProduct`, `DimChannel`, `DimDate`

## Dashboard KPIs

- Gross written premium
- Policy count and sum insured
- Claim frequency
- Claims paid, approved and rejected
- Loss ratio
- Claim settlement ratio
- Average claim turnaround time
- Premium by product, channel, region and agent

*(Dashboard screenshot to be added.)*

## Repository structure

```
excel-insurance-analytics/
├── README.md
├── data/                 # raw synthetic dataset
├── phase1-formulas/      # workbook + formula documentation
├── phase2-power-query/   # workbook + M code
├── phase3-power-pivot/   # workbook + data model + DAX measures
├── dashboard/            # final dashboard workbook + screenshots
└── docs/                 # data-quality log, notes, approach comparison
```

## Roadmap

- [x] Synthetic dataset created
- [ ] Phase 0: Data profiling and data-quality log
- [ ] Phase 1: Formula-based cleansing
- [ ] Phase 2: Power Query cleansing
- [ ] Phase 3: Power Pivot model and DAX measures
- [ ] Phase 4: Dashboard (PivotTables, slicers, charts)
- [ ] Phase 5: Approach comparison and lessons learned

## How to use

1. Download `data/Insurance_Raw_Data.xlsx`.
2. Open the workbook for the phase you want in desktop Excel (Microsoft 365 or Excel 2016 or later; Power Pivot needs Windows).
3. For Phase 2, update the file-path parameter to where you saved the raw data, then **Data → Refresh All**.

## Skills demonstrated

Data profiling · data cleansing · Excel formulas · Power Query (M) · Power Pivot and DAX · star-schema modelling · PivotTables and slicers · dashboard design · insurance domain KPIs

## Author

**Nivedita**, building a career in data analytics. [GitHub](https://github.com/nivedita-pai)

---

*All data in this repository is synthetic and generated for learning and portfolio purposes.*
