# DSBA 6171: [Domain Name] Data & Knowledge Microcosm

## 1. Team Information & Roles
* **Team Number / Name:** Team 4 - HealthTech
* **Team Lead / Liaison:** Paul Clark
* **Data & Ingestion Lead:** Thejeshri Rajesh
* **Database & Analytics Lead:** Paul Clark
* **Knowledge & Retrieval Lead:** Mohammed Qurneh & Natalia Walker
* **Quality & Governance Lead:** Taylor Ferguson

---

## 2. Business Context & Problem Framing
* **Assigned Domain:** HealthTech: Medical Claims & Payer Auditing
* **Business Function / Process:** Claims audit and payer compliance review for a regional health plan
* **Target Stakeholders:** Claims auditors, compliance/policy team, provider relations, finance/actuarial
* **Primary Decision Question:** For a given claim, was the coverage decision (denied, allowed) consistent with the plan and procedure policy that was in effect on the date of service?
* **Supporting Business Questions:**
  1. Which regions have the highest rate of claims that reference policies with missing or incomplete metadata?
  2. Are there any patterns across any member's claim history that suggest a systemic policy-application error?
  3. Which claims involve a procedure code or plan where the applicable policy is missing, or outdated? 

---

## 3. Microcosm Assets Overview

### Structured Operational Datasets (`data/structured/raw/`)
* `claims_ledger.csv` - Transaction/Event dataset for member claims.  500 rows.  PK: claim_id  FK: member_id, diagnosis_code, procedure_code
* `patient_accounts.csv` - Patient table, reference dataset for members.  275 rows.  PK: member_id
* `procedure_catalog.csv` - Procedure table, reference dataset for procedures.  19 rows.  PK: code

### Knowledge Corpus (`data/documents/source/`)
* Contains **[X]** total documents (**[Y]** PDFs, **[Z]** TXT/MD files).
* Includes structural challenges (tables, hierarchical headers) and version state diversity (e.g., Current vs. Superseded).

---

## 4. Structured Signal to Knowledge Linkage
* **Signal Example 1:** [Structured Field / Event] -> [Policy Document ID]
  * *Why it matters:* [Explanation of business impact]
* **Signal Example 2:** [Structured Field / Event] -> [Policy Document ID]
  * *Why it matters:* [Explanation of business impact]

---

## 5. Controlled Quality Issues & Risk Matrix Summary
| Quality Problem | Affected Layer | AI Impact | Business Consequence | Future Control |
| :--- | :--- | :--- | :--- | :--- |
| Miscoded data entries for the column insurance_type under the table patient_accounts | [text] | [text] | [text] | [text] |
| Outdated documents referencing diagnosis codes | [text] | [text] | [text] | [text] |
| Incorrect column type for numeric columns (e.g. float vs integer) | [text] | [text] | [text] | [text] |

---

## 6. How to Run & Environment Setup
1. Clone this repository to your local machine.
2. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
