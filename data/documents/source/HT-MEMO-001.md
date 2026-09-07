# SYNTHETIC MEMORANDUM

**[SYNTHETIC INTERNAL DOCUMENT FOR DEMONSTRATION PURPOSES ONLY]**

**TO:** HealthTech Claims Operations & Billing Auditors  
**FROM:** Executive Operations / Compliance Office  
**DATE:** February 1, 2026  
**SUBJECT:** Temporary Emergency Room Copay Waiver Directive (US-SE Region)  
**DOCUMENT ID:** HT-MEMO-001  
**VERSION:** v1.0-DRAFT  
**CONFIDENTIALITY:** Confidential  

### Directive Overview
Effective immediately for dates of service between February 1, 2026, and April 30, 2026, member copayments for emergency department visits resulting in immediate inpatient admission are to be waived.

### Operational Instructions for Auditors
- When reviewing claims with CPT 99285 (ER Level 5) followed by inpatient admission within 24 hours:
  - Override standard copay deduction in `claims_ledger.csv`.
  - Apply adjustment code `COPAY_WAIVE_SE2026`.
- Reference this memo in all manual claim adjustment logs.
