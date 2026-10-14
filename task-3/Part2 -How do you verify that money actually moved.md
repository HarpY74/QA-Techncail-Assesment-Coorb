### Q3.3 — How do you verify that money actually moved beyond the API returning `201 Created`?

I would not rely on the HTTP `201 Created` response alone, because it only confirms that the payment request was accepted or created, not necessarily that the transfer was completed.

- **Verify payment status:** Use the returned `paymentId` or `reference` to retrieve the payment details and confirm whether its status is `PENDING` or `COMPLETED`.
- **Verify account balances:** For a completed transfer, confirm that the source account was debited and the beneficiary account was credited with the correct amount and currency.
- **Verify ledger entries:** Check the transaction ledger or accounting records to confirm that the debit and credit entries were recorded correctly and balance according to the system's accounting rules.
- **Validate financial details:** Verify the amount, currency, fees, FX quote/rate when applicable, and value date.
- **Check for duplicate processing:** Confirm that the transfer resulted in only one financial effect, even if the client retried the request.
- **Verify failure handling:** For failed or rejected payments, confirm that no unintended debit or credit occurred and that any reserved funds were released according to the business rules.
- **Handle asynchronous processing:** If payment processing is asynchronous, poll the payment status and verify the ledger and balances within an agreed timeout, accounting for eventual consistency.

I would use controlled test accounts and correlate the API response, payment records, account balances, and ledger entries using the payment ID or reference. This provides evidence that the money movement was correct, complete, and not duplicated.
