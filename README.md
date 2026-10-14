# QA Technical Assessment

## Overview

This repository contains my solutions for the QA Technical Assessment, demonstrating a structured and risk-based approach to software quality assurance.

The assessment covers requirement analysis, test design, exploratory testing, API validation, and automation strategy, with an emphasis on identifying risks, validating expected behavior, and producing clear, maintainable documentation.

## Repository Structure

```text
QA-Technical-Assesment-Coorb/
├── README.md
├── ai-usage.md
├── Task 1/
├── Task 2/
├── Task 3/
└── Task 4/
```

## Task Summary

| Task | Focus Area | Status |
|---|---|---|
| Task 1 | Requirement Analysis and Test Case Design | Completed |
| Task 2 | Exploratory Testing and Defect Reporting | Completed |
| Task 3 | API Testing | Completed |
| Task 4 | Automation Design and Scalability | Completed |

## Task 1 — Requirement Analysis and Test Case Design

Analyzed the banking transfer requirements, identified clarification points, documented assumptions, and designed test scenarios based on business and financial risks.

The coverage considers transaction limits, balance validation, OTP behavior, duplicate transactions, negative scenarios, and failure handling.

## Task 2 — Exploratory Testing and Defect Reporting

Focused on exploring application behavior, identifying potential defects, and documenting findings with clear reproduction steps, expected versus actual results, and impact analysis.

## Task 3 — API Testing

Implemented API tests using Postman against the public ExchangeRate-API endpoint:

`https://open.er-api.com/v6/latest/USD`

The solution includes:

- Response status, structure, and response-time validation.
- Currency-rate checks and data-driven execution using CSV input.
- Environment variables and reusable request configuration.
- Capturing a response value and validating it in a subsequent request.
- Negative testing for an unsupported currency code.

The assertions reflect the provider's observed behavior rather than assuming a specific HTTP status for every error. The public API was used for assessment purposes and is not assumed to meet production banking requirements.

## Task 4 — Automation Design and Scalability

Addressed automation design considerations, including maintainability, scalability, and reporting. The proposed approach considers future enhancements, parallel execution, CI/CD integration, and effective test result reporting.

## Assumptions and Limitations

- Assumptions are documented where requirements do not specify expected behavior.
- Test expectations are based on the assessment requirements and available evidence.
- Response-time thresholds used in testing are assessment criteria, not official service-level agreements.
- Reported execution results reflect the tests performed.

## AI Usage Disclosure

AI assistance was used for selected analysis, test design, and documentation activities. The scope of this assistance and the review approach are documented in [`ai-usage.md`](ai-usage.md).

## Conclusion

This assessment demonstrates a structured QA approach that combines risk analysis, functional and negative testing, API validation, and automation planning. The deliverables emphasize clear test coverage, evidence-based decisions, and maintainable documentation.
