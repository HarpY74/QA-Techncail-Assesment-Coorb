# AI Usage Disclosure

## 1. Purpose

AI assistance was used to support selected activities during the QA Technical Assessment. The purpose was to improve the organization, clarity, and completeness of the testing documentation while keeping the assessment requirements and QA reasoning as the primary basis for the work.

## 2. AI-Assisted Activities

### Task 1 – Local Fund Transfer

AI assistance was used for the following activities:

- Reviewing and structuring requirement clarification questions.
- Drafting working assumptions for ambiguous requirements.
- Identifying potential risks and their business impacts.
- Organizing test scenarios into a structured test case format.
- Reviewing test coverage, deliberate omissions, automation candidates, and release readiness criteria.
- Improving the clarity and consistency of written documentation.

## 3. Representative Prompt Summaries

The following summaries describe the types of prompts used during the work. They are summaries, not necessarily verbatim copies of the original prompts.

| Activity | Prompt Summary |
|---|---|
| Requirement analysis | Help analyze the Local Fund Transfer requirements and identify clarification questions and assumptions. |
| Risk assessment | Identify important functional, financial, security, and operational risks and explain their potential impact. |
| Test case design | Create a maximum of 12 test cases with preconditions, test data, expected results, and execution status. |
| Coverage review | Identify deliberate omissions, recommend automation versus manual testing, and define release safety criteria. |
| Documentation | Organize the assessment artifacts and improve the clarity and consistency of the Markdown and spreadsheet documentation. |

## 4. Suggestions Retained

AI suggestions were used as a starting point for organizing the analysis and documentation, including:

- Structuring assumptions and clarification questions.
- Considering duplicate transaction prevention and recovery after connectivity or processing failures.
- Covering transaction limits, OTP validation, fees, balance checks, and notification failures.
- Separating automated regression candidates from scenarios better suited to manual or exploratory testing.
- Using a clear test case format and explicit execution statuses.

## 5. Suggestions Modified or Not Treated as Confirmed Requirements

Some AI-generated suggestions were treated as proposals rather than confirmed requirements.

- **Bank-specific behavior:** Suggested behavior was not treated as official bank policy unless supported by the assessment requirements or explicitly documented as an assumption.
- **Unconfirmed settlement rules:** Settlement timing and recovery behavior were identified as requiring confirmation where the requirements did not specify the exact rules.
- **Fee and limit interpretation:** The assumed fee calculation and treatment of fees against transaction limits require confirmation from the Business Analyst/Product Owner.
- **Test execution results:** Test cases were marked `Not Run` because test execution results must not be fabricated.
- **Scope and coverage:** Suggested scenarios were prioritized and organized according to the assessment scope and the stated test case limit.

## 6. Validation and Responsibility

AI-generated content was used as assistance, not as a substitute for QA judgment. Requirements, assumptions, test data, expected results, and recommendations should be reviewed against the original assessment instructions.

The final submission remains the responsibility of the candidate. No test execution, external system validation, or business approval is claimed unless supported by actual evidence.

## 7. Future Tasks

If AI assistance is used for additional tasks, this document will be updated to reflect the relevant activities, representative prompts, suggestions retained or rejected, and the rationale for those decisions.
