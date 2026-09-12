# Section 1: Structured Data Architecture & AI Consumption
**Author:** Paul Clark | **Team:** Team 4 — HealthTech: Medical Claims & Payer Auditing

---

## Business Purpose and Grain

**claims_ledger.csv** (500 rows, 23 cols)
Each row represents one claim billing event submitted by a provider for services rendered to a member on a specific date of service. PK: `claim_id`

**patient_accounts.csv** (275 rows, 6 cols)
Each row represents a member and their current plan, insurance type, region, enrollment date, and eligibility status. This is a snapshot of current members. PK: `member_id`

**procedure_catalog.csv** (19 rows, 5 cols)
Each row represents a procedure or diagnosis code. This dataset serves as the reference catalog of valid procedure and diagnosis codes used to validate and categorize claims. PK: `code`

---

## Dataset Attributes

| Dataset | Type |
|---|---|
| claims_ledger.csv | Event / Transaction |
| patient_accounts.csv | Reference / Master |
| procedure_catalog.csv | Reference / Master |

---

## Important Keys and Relationships

**claims_ledger.csv**
- PK: `claim_id` — uniquely identifies each billing event; anchor for all audit and fraud analysis
- FK: `member_id` → `patient_accounts.member_id` (many-to-one) — without this join, the AI cannot verify the member was covered at time of service
- FK: `diagnosis_code` → `procedure_catalog.code` (many-to-one) — validates the diagnosis code exists in the approved catalog; essential for detecting invalid code use
- FK: `procedure_code` → `procedure_catalog.code` (many-to-one) — validates the procedure; links the claim to whether prior authorization is required (`review_flag`)

**patient_accounts.csv**
- PK: `member_id` — referenced by claims_ledger; one member may have many claims over time

**procedure_catalog.csv**
- PK: `code` — referenced by both `diagnosis_code` and `procedure_code` in claims_ledger; gives codes their business meaning (description, category, review_flag)

---

## Primary Access/Storage Pattern and Ingestion Approach

**claims_ledger.csv**
- Storage: Columnar/analytical (Parquet) — optimized for AI scanning across fields like `billed_amount`, `allowed_amount`, `decision_status`, `is_fraud_flag`
- Ingestion: Batch — claims submitted by providers on a recurring cycle; daily or weekly load

**patient_accounts.csv**
- Storage: Columnar/analytical; may also be mirrored in operational storage for eligibility lookups
- Ingestion: CDC (Change Data Capture) — eligibility status, plan, and enrollment data changes over time and must be captured as it occurs to avoid point-in-time errors

**procedure_catalog.csv**
- Storage: Columnar/analytical — small reference table (19 rows) used by AI to validate and categorize codes
- Ingestion: Batch on a slow schedule — updated infrequently when CPT/ICD-10 standards bodies release updates

---

## How an AI System Might Consume the Dataset

**claims_ledger.csv**
- Feature/input: Fields like `billed_amount`, `allowed_amount`, `days_between_service_and_claim`, `claims_per_provider_monthly`, and `is_fraud_flag` are direct inputs to a fraud detection model
- Historical context: Full claims history enables pattern detection — providers consistently overbilling, members with unusually high claim frequency
- Decision-support input: AI surfaces high-risk claims to a human auditor with supporting context

**patient_accounts.csv**
- Current case context: AI looks up `eligibility_status`, `plan_code`, and `insurance_type` to confirm coverage was active at time of service before any claim decision is made

**procedure_catalog.csv**
- Decision-support input: AI uses `review_flag` to determine if a procedure requires additional scrutiny; uses `category` and `description` to provide plain-language context to the auditor

---

## One Important Assumption the AI Consumer Makes

**claims_ledger.csv**
The AI assumes that `is_fraud_flag`, `decision_status`, and billing fields are accurately and consistently labeled at ingestion. Errors or inconsistent labeling produce unreliable fraud predictions and audit flags.

**patient_accounts.csv**
The AI assumes `eligibility_status` reflects the member's coverage at the exact time a claim was processed (point-in-time correctness). A stale snapshot may cause the AI to approve a claim for an ineligible member — a payer compliance violation.

**procedure_catalog.csv**
The AI assumes the catalog is complete and current. A missing code causes valid claims to be flagged incorrectly; a retired code may allow invalid claims to pass through.

---

*Architecture diagram:* See `architecture/` folder in this repository.
