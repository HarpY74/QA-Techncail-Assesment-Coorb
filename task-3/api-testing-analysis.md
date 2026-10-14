# Task 3 — Part A: FX Rates API Testing Analysis

## 1. Objective

The objective of this task is to test an upstream foreign-exchange (FX) rates API that a banking application could use to display indicative exchange rates before an international transfer.

## 2. API Provider Used

- **Provider:** Open Exchange Rates API by open.er-api.com
- **Endpoint pattern:** `https://open.er-api.com/v6/latest/{BASE_CURRENCY}`
- **Authentication:** No API key required for this public endpoint
- **Currencies used for data-driven testing:** USD, EUR, GBP
- **Backup provider:** Frankfurter was not used because the selected provider was available and returned the expected response structure.

The collection uses Postman variables for the base URL and base currency rather than hard-coding the full endpoint in each request.

## 3. Collection Contents

1. **Get FX Rates - Data Driven** — Retrieves rates for the selected base currency and validates the response.
2. **Validate Captured FX Rate** — Reads a previously captured EGP rate and compares it with the rate in a subsequent response.
3. **Negative - Unsupported Currency** — Checks how the provider responds to an unsupported currency code (`XXX`).

## 4. Data-Driven Execution

The main request was executed using a CSV data file named `currency-data.csv`:

```csv
base_currency
USD
EUR
GBP
```

This approach runs the same request for multiple base currencies without duplicating requests. The environment provides the `base_url` and a default `base_currency`; the iteration data supplies the base currency during Collection Runner execution.

**Observed result:** 3 iterations completed successfully. The main request's assertions passed for USD, EUR, and GBP.

## 5. Assertions and Rationale

The main request includes the following checks:

- **HTTP status is 200:** Confirms that the provider returned the expected HTTP response for a valid currency request.
- **Response contains `result` and `rates`:** Checks that the main response fields are present.
- **API result is `success`:** Confirms application-level success, not only HTTP-level success.
- **Base currency matches the selected currency:** Checks that `base_code` matches the currency supplied by the environment or iteration data.
- **Base currency rate equals 1:** Checks that the selected base currency has a rate of 1 relative to itself.
- **Expected target currencies are present:** Checks for EUR, GBP, and EGP in the `rates` object.
- **Target rates are positive numbers:** Rejects missing, non-numeric, zero, or negative values for the checked currencies.
- **Response time is below 3000 ms:** Applies a simple performance threshold for this assessment. This is an exercise threshold, not a published provider SLA.
- **Expected response schema/shape:** Confirms the response is an object, `result` is `success`, `base_code` is a string, and `rates` is an object rather than an array.

These assertions cover HTTP status, response time, response structure, expected data, and basic value sanity. They do not prove that the provider's rates are financially accurate; that would require an independent trusted reference source and appropriate business tolerances.

## 6. Capturing and Reusing a Response Value

The main request saves these collection variables:

- `captured_egp_rate` — the EGP rate from the response
- `captured_base_currency` — the base currency returned by the provider

The `Validate Captured FX Rate` request reads `captured_egp_rate` and checks that the subsequent response is successful and that its EGP rate is within 1% of the captured value.

**Why this value was selected:** EGP is a relevant target currency for this test scenario, and reusing its rate demonstrates how a value from one API response can be carried into a subsequent validation.

**Important limitation:** The 1% tolerance is an assumption for this exercise, not an official provider guarantee. For a banking application, the acceptable tolerance and rate freshness rules should be defined by product/business requirements. A real transfer should also use the bank's approved pricing and execution rules rather than relying only on an indicative public rate.

## 7. Negative Test: Unsupported Currency

The negative request uses:

`GET https://open.er-api.com/v6/latest/XXX`

**Observed behavior:**

- HTTP status: `200`
- Response body:
  - `result`: `error`
  - `error-type`: `unsupported-code`

The test asserts the observed provider behavior: HTTP 200, `result` equal to `error`, and `error-type` equal to `unsupported-code`.

### Assessment for a banking application

An HTTP 200 response does not necessarily mean the requested operation succeeded. The client must inspect the response body and handle the application-level error. For a banking integration, this behavior should be documented and handled safely; ideally, an API would use an appropriate 4xx status for invalid input, but the test must validate the behavior the selected provider actually returns.

The banking application should not display an invalid rate or allow a transfer to proceed based on this error response.

## 8. Execution Summary

- Main request: **9/9 assertions passed** in the observed run after adding the response-shape check.
- Data-driven run: **3 iterations passed** for USD, EUR, and GBP.
- Captured-rate validation request: **3/3 assertions passed** in the observed run.
- Unsupported-currency negative request: **3/3 assertions passed** in the observed run.

These results describe the runs performed during the assessment. They are not a guarantee that future requests will always pass.

## 9. Files Submitted

- `Task_3_FX_Rates_Collection.json` — exported Postman collection
- `Task_3_FX_Environment.json` — exported Postman environment
- `currency-data.csv` — data-driven test input
- `api-testing-analysis.md` — this analysis and test rationale
