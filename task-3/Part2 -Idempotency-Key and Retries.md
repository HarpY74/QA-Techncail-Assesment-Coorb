### 3.2 — Idempotency-Key and Retries

The `Idempotency-Key` prevents the same payment from being processed more than once when a client retries a request.

I would test it by:
- Sending a valid payment request and verifying that it is processed only once.
- Resending the same request with the same key and verifying that no duplicate payment or debit occurs.
- Reusing the same key with a different request body and verifying that the API rejects the conflict according to its contract.
- Testing concurrent duplicate requests to ensure only one payment is created.

If a network timeout occurs, I would not assume the payment failed, because it may have been processed while the response was lost. I would check the payment status and retry with the same idempotency key if needed.

After a `503 Service Unavailable` response, I would follow the defined retry policy, use the same key for the same intended payment, and verify the final payment status to avoid duplicate processing.

The system should return the original payment result for a valid retry, or clearly indicate that the original request is still being processed.
