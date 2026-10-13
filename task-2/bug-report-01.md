### Bug ID
BUG-01

### Title
**Transfer Funds Allows Transfers Exceeding the Available Balance**

### Environment
- Application: ParaBank public demo banking application
- URL: https://parabank.parasoft.com/parabank/index.htm
- Browser: [Enter browser and version]
- Test account: Newly registered demo user

### Preconditions
1. The user is registered and logged in.
2. The source account has a balance of $400.
3. The destination account has a balance of $100.

### Steps to Reproduce
1. Log in to ParaBank.
2. Open **Accounts Overview** and verify that the source account balance is $400.
3. Verify that the destination account balance is $100.
4. Navigate to **Transfer Funds**.
5. Select the source account with a $400 balance.
6. Select the destination account with a $100 balance.
7. Enter $1,000 as the transfer amount.
8. Submit the transfer.
9. Observe the confirmation message and return to **Accounts Overview**.
10. Check the balances of both accounts.

### Expected Result
The application should reject the transfer because the requested amount ($1,000) exceeds the source account's available balance ($400). Neither account balance should change.

**Expectation basis:** Standard banking balance-integrity rules: an account must not be allowed to transfer more money than its available balance unless an explicitly supported overdraft facility permits it.

### Actual Result
The application displayed a transfer confirmation and processed the transfer. The source account balance changed from $400 to -$600, while the destination account balance increased from $100 to $1,100.

### Severity
**High** — The application allows a transfer exceeding the available balance and results in a negative source balance, potentially violating financial balance-integrity rules.

### Priority
**High (proposed)** — This should be investigated promptly because it affects a core financial transaction and could permit unauthorized overdrafts if the same behavior exists in a real banking system.

### Evidence
Attached screenshots showing:
1. Both account balances before the transfer.
2. The Transfer Funds form with $1,000 entered and the selected source and destination accounts.
3. The transfer confirmation message.
4. Both account balances after the transfer, showing -$600 and $1,100.
