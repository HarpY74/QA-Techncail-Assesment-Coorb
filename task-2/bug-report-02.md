### Bug ID
BUG-02

### Title
Bill Pay Accepts and Records a Zero-Amount Payment

### Environment
- Application: ParaBank public demo banking application
- URL: https://parabank.parasoft.com/parabank/index.htm
- Browser: [Enter browser and version]
- Test account: Registered demo user

### Preconditions
1. The user is registered and logged in.
2. A source account is available.
3. The Bill Pay form is accessible.

### Steps to Reproduce
1. Log in to ParaBank.
2. Navigate to **Bill Pay**.
3. Enter the required payee information using test data.
4. Select the source account.
5. Enter `0` in the payment amount field.
6. Submit the payment.
7. Observe the result message.
8. Open the account transaction history and verify whether the zero-amount payment was recorded.

### Expected Result
The application should reject a zero-amount payment and display a validation message requiring an amount greater than zero.

**Expectation Basis:** Standard payment validation practice. A payment should represent a positive monetary amount unless zero-value transactions are explicitly supported by the business requirements.

### Actual Result
The application accepted the zero-amount payment, and the transaction was recorded in the transaction history.

### Severity
Medium (proposed) — The application records a payment with no monetary value, potentially affecting the accuracy of payment records and reporting.

### Priority
Medium (proposed) — Input validation should be corrected to prevent invalid payment records. Final priority depends on the application's business rules and downstream impact.

### Evidence
Attach:
1. Screenshot of the Bill Pay form showing the amount `0`.
2. Screenshot of the success or confirmation message.
3. Screenshot of the transaction history showing the recorded zero-amount payment.
