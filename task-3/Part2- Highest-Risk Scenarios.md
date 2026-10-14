## 3.1 — Highest-Risk Scenarios

I would focus on the following high-risk scenarios:

1. **Amount and currency validation:** Test missing, null, zero, negative, non-numeric, and excessively large amounts. Validate decimal precision, supported currencies, and currency compatibility with the source account and FX quote.

2. **Transfer limits:** Test amounts below, exactly at, and above the per-transaction limit. Also test daily limits based on previous successful transactions. Requests exceeding limits should return the expected error, such as `422`, without moving any money.

3. **Insufficient funds:** Test when the available balance is less than, equal to, and greater than the transfer amount plus applicable fees. Insufficient-funds requests should be rejected without an incorrect debit or beneficiary credit.

4. **Duplicate requests and concurrent transfers:** Submit the same payment multiple times, including concurrent requests, to verify that the same intended payment is not processed twice.

5. **Account and authorization validation:** Test inactive or frozen source accounts, invalid beneficiaries, missing or invalid authorization, and attempts to access accounts owned by another customer.

6. **FX quote validation:** Test expired, invalid, or currency-mismatched FX quotes. Verify that the payment uses the correct approved rate or is rejected according to the API contract.

7. **Failure handling and consistency:** Simulate processing failures and service unavailability. Verify that the system does not leave an unintended debit, duplicate payment, or inconsistent account balance.

For every rejected payment, I would verify not only the expected HTTP status and error response, but also that no unintended financial movement occurred. I would validate the payment status, account balances, and ledger entries.
