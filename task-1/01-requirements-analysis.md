# 01 – Requirements Analysis

## Objective

Analyze the Local Fund Transfer requirements, identify ambiguities, and document working assumptions before finalizing the test design.

## Clarification Questions and Assumptions

### Q1. What currency are the limits (20,000 / 10,000) denominated in?

**Assumption:** Both limits are denominated in local currency (EGP). All limit-related test cases use EGP values.

### Q2. Does the daily limit reset at midnight or use a rolling 24-hour window?

**Assumption:** The daily transfer limit resets at midnight local time, based on the calendar day.

### Q3. Is the fee deducted from the transfer amount or charged separately on top?

**Assumption:** The fee is charged separately on top of the transfer amount and deducted from the source account.

### Q4. What is the fee structure: flat fee, percentage, or tiered?

**Assumption:** A flat fee of EGP 10 is charged per transaction, regardless of the transfer amount. This must be confirmed against the official fee schedule.

### Q5. What is the OTP expiry time, maximum retry count, and behavior after retries are exhausted?

**Assumption:** The OTP expires after 5 minutes. After 3 failed attempts, the transaction is cancelled, no debit occurs, and the customer must initiate a new transfer.

### Q6. What happens if the app loses connectivity after OTP submission but before confirmation appears?

**Assumption:** The backend uses idempotency controls to prevent duplicate execution. Retrying the same transaction returns its original result instead of executing it again. After reconnection, the app checks the transaction status before allowing a new transfer.

### Q7. Does AC6 require a generic error message or a distinct message for each failure type?

**Assumption:** Error messages depend on the failure type:

- **Insufficient funds:** A specific insufficient-funds message.
- **Limit exceeded:** A specific message showing the remaining daily allowance, where applicable.
- **OTP failure:** A specific message showing the remaining attempts.
- **System error:** “Please try again” with a reference code.
- **Invalid source account or beneficiary:** A specific validation or eligibility message.

### Q8. What happens if the account is debited but the transfer or confirmation fails?

**Assumption:** An appropriate recovery or reversal process is initiated if a debit occurs but the transfer fails. The transaction remains traceable, and the app must not report success until the backend confirms execution.

### Q9. What is the expected value date or settlement time for the beneficiary?

**Assumption:** Settlement follows the bank's configured processing rules. The app displays the actual transaction status and does not imply that the beneficiary has received the funds before confirmation.

### Q10. Does SMS failure affect an otherwise successful transfer?

**Assumption:** SMS is handled separately from transfer execution. SMS failure does not reverse a successful transfer, and confirmation remains available in the application.

### Additional Clarification: Do fees count toward transfer limits?

**Assumption:** The initial test design calculates limits using the transfer amount only, excluding the fee. This must be confirmed by the Business Analyst/Product Owner.

## Assumption Validation

These assumptions support the initial test design and must not be treated as confirmed bank policies unless validated by the Business Analyst/Product Owner. Confirmed changes must be reflected in the relevant test cases and expected results.
