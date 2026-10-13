# 02 – Risk Assessment

## Objective

Identify the main risks associated with the Local Fund Transfer feature, evaluate their potential impact, and recommend testing or mitigation activities.

Severity is assessed qualitatively. Likelihood has not been rated because production data and failure-rate information were not provided.

## Risk Register

| ID | Risk | Potential Impact | Severity | Recommended Mitigation |
|---|---|---|---|---|
| R1 | Duplicate transfer after repeated submission or network interruption. | Duplicate debit or credit, financial loss, reconciliation issues, and customer complaints. | Critical | Test idempotency, repeated requests, delayed responses, and reconnection behavior. Verify balances and transaction records. |
| R2 | Invalid or expired OTP accepted, or OTP verification bypassed. | Unauthorized transfers, fraud, and security or compliance concerns. | Critical | Test valid, invalid, and expired OTPs and retry exhaustion. Verify no unauthorized execution. |
| R3 | Incorrect enforcement of daily or per-transaction limits. | Transfers exceed policy limits or legitimate transfers are rejected. | Critical | Test values below, at, and above each limit, accumulated transfers, and daily reset behavior. |
| R4 | Incorrect fee calculation or unclear fee disclosure. | Unexpected debit, financial discrepancies, and customer complaints. | High | Verify the displayed fee, assumed EGP 10 charge, total debit, and transaction records. |
| R5 | Insufficient-funds validation ignores the transaction fee. | The total debit exceeds the available balance or a valid transfer is rejected. | High | Verify available balance against the transfer amount plus the applicable fee. |
| R6 | Transfer fails after the source account is debited. | Funds remain unavailable, transaction status is uncertain, and reconciliation is required. | Critical | Simulate processing failures where possible. Verify recovery/reversal, traceability, final status, and balance consistency. |
| R7 | Invalid or ineligible source account or beneficiary is accepted. | Downstream processing failure or transfer against an ineligible account. | High | Test inactive or ineligible source accounts and invalid, unavailable, or ineligible beneficiaries. |
| R8 | Incorrect or ambiguous transaction status or error message. | Customers may repeat successful transfers or misunderstand failures. | High | Validate messages and status transitions for success, limit failures, OTP failures, and system errors. |
| R9 | Settlement timing or beneficiary receipt is misrepresented. | Customer confusion, complaints, and incorrect expectations about fund availability. | High | Confirm settlement rules and validate displayed statuses against backend processing. |
| R10 | SMS delivery fails or is delayed after success. | The customer misses the notification and may retry the transfer unnecessarily. | Medium | Simulate SMS failure and verify that transfer status remains successful and in-app confirmation is available. |
| R11 | Account balances, transaction records, and confirmation details are inconsistent. | Reconciliation problems, reduced auditability, and difficulty investigating incidents. | Critical | Validate source balance, fee, transfer amount, reference number, backend status, and audit records where accessible. |

## Risk Prioritization

Testing should prioritize:

1. Financial integrity and duplicate prevention.
2. OTP authentication and transaction security.
3. Daily and per-transaction limits.
4. Recovery from partial failures.
5. Balance, fee, and transaction-record consistency.

Critical risks should be addressed before release. High-severity risks require resolution or an explicitly documented risk-acceptance decision from authorized stakeholders.

Risk ratings should be revisited when likelihood data and formal risk-acceptance criteria become available.
