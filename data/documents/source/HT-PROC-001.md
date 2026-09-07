# High-Dollar Claim Audit & Escalation Workflow

**Document ID:** HT-PROC-001  
**Owner:** Claims Operations  
**Effective Date:** October 1, 2025  
**Version:** v1.0 (APPROVED)  
**Confidentiality:** Internal  

## 1. Purpose & Scope
This standard operating procedure defines the mandatory escalation paths for medical claims exceeding baseline reimbursement limits or triggering high-risk compliance flags.

## 2. Threshold Escalation Triggers
A claim must be routed to Senior Audit Review if any of the following triggers are met:
- **Financial Threshold:** Total billed amount exceeds **$25,000.00** on a single claim event.
- **Unbundling Signal:** Multiple secondary procedure codes billed without appropriate CCI modifiers.
- **Experimental Flag:** Procedure code listed under HT-POL-001 Section 2 without pre-approval documentation.

## 3. Escalation Workflow Steps
1. **Automated Hold:** Claims Engine places `HOLD_HIGH_DOLLAR` status on the transaction.
2. **Initial Auditor Review (T+1 Business Day):** Examiner checks `claims_ledger.csv` against `HT-GUIDE-002` allowed fee matrix.
3. **Medical Director Review (T+3 Business Days):** Required for clinical necessity disputes or high-dollar facility stays exceeding 5 continuous days.
4. **Final Decision Logging:** Result recorded as `APPROVED`, `ADJUSTED`, or `DENIED` in audit ledger.
