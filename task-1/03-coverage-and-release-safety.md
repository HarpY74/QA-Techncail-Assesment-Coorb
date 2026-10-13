# 03 – Test Coverage and Release Safety

## A. Deliberate Omissions

The assessment limits the test design to a maximum of 12 test cases. The following areas are not covered exhaustively in the initial suite.

| Omitted or Limited Coverage | Reason | Recommended Follow-up |
|---|---|---|
| Exhaustive amount boundaries and numeric formats. | Limited test case count. | Add exact-limit, just-above-limit, decimal precision, and invalid-format scenarios. |
| Every OTP boundary and recovery scenario. | Initial coverage focuses on expiry and three failed attempts. | Test exact expiry boundaries, OTP resend, rate limiting, and recovery behavior. |
| Alternative fee configurations. | Initial design uses a provisional flat EGP 10 fee. | Expand coverage when the official fee schedule is available. |
| Every connectivity interruption point. | Initial coverage focuses on duplicate prevention and status checks after reconnection. | Test interruption before submission, during processing, after debit, and before confirmation. |
| Full settlement-calendar coverage. | Settlement rules and cut-off times are unspecified. | Test business days, holidays, delayed settlement, and pending status once requirements are confirmed. |
| Performance, load, and stress testing. | Requires a separate non-functional testing scope and agreed targets. | Execute performance tests against approved throughput and response-time requirements. |
| Accessibility and localization. | Outside the initial functional test case limit. | Perform accessibility checks and validate supported languages and currency formatting. |
| Exhaustive SMS delivery scenarios. | Initial coverage focuses on SMS failure after a successful transfer. | Test delayed and duplicate notifications and correct recipient handling. |

These are deliberate scope limitations, not claims that the omitted scenarios have passed testing.

## B. Automation vs. Manual Testing

### Recommended Automation Candidates

- Successful transfer flow, including confirmation details and transaction reference.
- Per-transaction and daily limit validation.
- Invalid amounts and insufficient-funds scenarios.
- OTP validation and retry limits, where test OTPs or supported test hooks are available.
- Fee calculation and account balance validation.
- Duplicate submission and idempotency behavior.
- Backend transaction status and database consistency, where authorized test access is available.
- SMS integration checks, where a reliable test environment or notification stub is available.

**Safety note:** Use controlled test accounts. Do not execute real financial transfers in production without explicit authorization.

### Recommended Manual Testing

- Exploratory testing of the transfer flow and unexpected user behavior.
- Usability and clarity of error messages and confirmation details.
- Real-device OTP and SMS behavior where simulator coverage is insufficient.
- Accessibility and localization checks.
- Investigation of ambiguous transaction outcomes and recovery behavior.
- Visual validation of transaction status after connectivity interruptions.

Automation supports repeatable regression scenarios, while manual testing remains valuable for exploratory behavior and user experience.

## C. Release Safety Checklist

Before recommending release, verify the following:

- [ ] All critical test cases have been executed and passed.
- [ ] Valid transfers execute once with the correct debit, fee, and transaction reference.
- [ ] Daily and per-transaction limits follow confirmed business rules.
- [ ] Invalid, expired, and exhausted OTP attempts do not result in unauthorized execution.
- [ ] Duplicate submissions and retries do not create duplicate transfers.
- [ ] Partial failures and debit/reversal scenarios have verified outcomes and traceable records.
- [ ] Invalid or ineligible source accounts and beneficiaries are rejected.
- [ ] Error messages and transaction statuses accurately reflect backend outcomes.
- [ ] SMS failure does not incorrectly reverse or duplicate a successful transfer.
- [ ] No unresolved critical defects remain.
- [ ] High-severity defects have documented, authorized risk decisions.
- [ ] Regression evidence, known limitations, monitoring, reconciliation, and recovery procedures are documented.
- [ ] Required QA, business, and technical stakeholders have provided sign-off.

## D. Go / No-Go Recommendation

### No-Go Conditions

Do not recommend release if:

- A critical financial-integrity or security scenario fails.
- Duplicate transfer execution is possible.
- Transaction outcomes remain uncertain without a controlled recovery path.
- The system reports success without confirmed execution.
- A critical defect remains unresolved without an approved exception.

### Conditions to Reconsider Release

Reconsider the release after blocking issues are fixed, affected scenarios are retested, regression evidence is reviewed, and the required stakeholders approve any remaining risks.

## Final Note

This assessment is based on the available requirements and explicitly identified working assumptions. Final expected results must be aligned with confirmed bank rules before the test suite is considered release-ready.
