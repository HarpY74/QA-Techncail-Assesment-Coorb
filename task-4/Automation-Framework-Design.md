# Task 4 --- Automation Framework Design

## 1. Objective and Scope

The objective is to design a maintainable UI automation approach for a
Bank Admin Console used to configure operational settings such as
onboarding rules, KYC thresholds, transfer limits, fees, notifications,
channels, and audit settings.

The main workflow is to update at least 12 configuration fields across
at least 4 tabs, save the changes, reload the page, and verify that the
saved values persist.

This deliverable focuses on framework design and test strategy. It does
not claim that the application has been inspected or that executable
locators have been validated.

### In scope

-   A scalable framework structure and separation of responsibilities.
-   Stable locator and control-handling strategies.
-   Handling tab changes, scrolling, and DOM re-rendering.
-   Test data management and safe cleanup in a shared environment.
-   Persistence validation after saving and reloading.
-   Growth, execution, reporting, and CI/CD considerations.

### Out of scope

-   A complete, runnable implementation of the framework.
-   Claims about actual DOM attributes, field labels, or tab names that
    have not been confirmed.
-   Changes to production configuration or unrestricted database resets.

## 2. Assumptions and Constraints

The assessment refers to `Bank_Admin_Console_Demo.html`, but the HTML
file was not available for inspection. Christine confirmed that the
HTML-file reference in Task 4 should be disregarded and that the rest of
the assessment should be completed.

Accordingly: - The design uses logical examples of configuration tabs
and fields, not verified application labels. - Locator examples are
strategies to apply after inspecting the actual application; they are
not asserted to exist in the current DOM. - The final test data must be
aligned with the real UI labels and available controls if the
application becomes available. - Tests will run only in an authorized
test environment with approved test data.

## 3. Recommended Technology Stack

  ------------------------------------------------------------------------
  Area                    Selection                Reason
  ----------------------- ------------------------ -----------------------
  Language                Java                     Strong fit for a
                                                   structured,
                                                   maintainable automation
                                                   framework.

  Browser automation      Selenium WebDriver       Mature browser
                                                   automation ecosystem
                                                   and suitable for
                                                   UI-driven configuration
                                                   workflows.

  Test runner             TestNG                   Supports
                                                   setup/teardown,
                                                   assertions, grouping,
                                                   and controlled parallel
                                                   execution.

  Build and dependencies  Maven                    Reproducible dependency
                                                   management and standard
                                                   CI integration.

  Design pattern          Page Object Model (POM)  Keeps test intent
                          with reusable components separate from UI
                                                   implementation details.

  Synchronization         Selenium Explicit Waits  Waits for meaningful UI
                                                   conditions instead of
                                                   relying on fixed
                                                   delays.

  Test data               JSON plus                Separates configuration
                          properties/environment   values from test logic.
                          configuration            

  CI/CD                   Jenkins                  Supports scheduled or
                                                   change-triggered
                                                   execution and result
                                                   publishing.

  Reporting               TestNG reports;          Provides failure
                          optionally Allure        details and
                                                   trend-friendly
                                                   reporting without
                                                   requiring a custom
                                                   reporting framework.
  ------------------------------------------------------------------------

Selenium with Java is selected because it is a mature and maintainable
option for this use case and aligns with the intended framework design.
The design can be revisited if the application's technology or team
standards make another tool a better fit.

## 4. Proposed Project Structure

``` text
bank-admin-automation/
├── pom.xml
├── testng.xml
├── README.md
├── src/
│   ├── main/
│   │   └── java/com/bank/automation/
│   │       ├── driver/
│   │       │   └── DriverFactory.java
│   │       ├── pages/
│   │       │   └── AdminConsolePage.java
│   │       ├── components/
│   │       │   ├── ConfigurationTab.java
│   │       │   ├── ConfigField.java
│   │       │   └── FieldHandler.java
│   │       └── utils/
│   │           ├── WaitUtils.java
│   │           └── TestDataReader.java
│   └── test/
│       ├── java/com/bank/automation/tests/
│       │   └── ConfigurationPersistenceTest.java
│       └── resources/
│           ├── config/
│           │   └── config.properties
│           └── testdata/
│               └── bank-console-regression.json
```

### Responsibilities

-   **DriverFactory:** creates and closes the WebDriver instance and
    applies browser settings.
-   **AdminConsolePage:** represents the console page and owns
    high-level actions such as opening tabs, saving, and reloading.
-   **ConfigurationTab:** encapsulates tab-specific navigation and
    access to its fields.
-   **ConfigField / FieldHandler:** represents a setting and delegates
    interaction to a handler appropriate for its control type.
-   **WaitUtils:** provides reusable explicit-wait helpers for
    conditions that are used consistently across the framework.
-   **TestDataReader:** loads and validates external test data.
-   **ConfigurationPersistenceTest:** describes the business-level test
    flow without embedding low-level locators throughout the test.

This structure avoids placing every locator and interaction in one large
page class. Components should be introduced where they reduce
duplication and clarify ownership, not simply to create more classes.

## 5. Test Scenario and Persistence Verification

The primary test flow is:

1.  Load and validate the test data.
2.  Open the admin console in the authorized test environment.
3.  Record the original values of the settings that will be changed,
    where reading those values is permitted.
4.  Navigate through at least 4 tabs and update at least 12 fields using
    the appropriate control handler.
5.  Verify that the controls show the requested values before saving.
6.  Save the configuration and wait for a reliable success condition,
    such as a confirmed success message or another application-defined
    indication.
7.  Reload the page.
8.  Reopen the relevant tabs and read each field again.
9.  Compare every persisted value with its expected value.
10. Record the result, including the tab and field associated with any
    failure.
11. Restore original values when safe and authorized, then verify
    cleanup.

A successful save message alone is not sufficient proof of persistence.
The core assertion is that the expected values remain after a reload and
are read back from the UI.

### Control-specific verification

-   **Text input:** read the current value, typically using the input's
    `value` property/attribute as appropriate for the control.
-   **Checkbox or toggle:** verify the selected/checked state.
-   **Native dropdown:** read the selected option using Selenium's
    `Select` support.
-   **Custom dropdown:** use the application-specific interaction and
    read-back strategy after inspecting its DOM and behavior.

The test should compare normalized values only when normalization is
part of the documented business rule. It should not silently treat
different values as equal.

## 6. Locator Strategy

### Preferred locator order

1.  Stable, purpose-built attributes such as `data-testid` or
    `data-field`, if the application provides them.
2.  Stable semantic attributes such as `name`, where unique and
    meaningful.
3.  A stable field wrapper associated with its visible label or a unique
    descriptive attribute.
4.  A relative locator scoped to the field wrapper to find the input,
    checkbox, or dropdown.

Dynamic identifiers such as IDs or slot values that change during a
session should not be the primary locator strategy.

### Label/wrapper-based approach

The framework should identify the setting through a stable parent
wrapper or label, then locate the child control within that wrapper.
This makes the strategy independent of the child control's generated ID
and supports different control types.

The exact XPath or CSS selector must be selected only after inspecting
the real DOM. For example, a label-to-wrapper relationship is a design
pattern, not a claim that the missing HTML contains a particular
attribute.

### Locator quality rules

-   Each locator should identify one intended control.
-   Avoid absolute XPath expressions tied to the entire page layout.
-   Avoid relying on CSS classes that are generated or frequently
    changed.
-   Avoid using visible text alone if it is duplicated or localized.
-   Keep locator definitions close to the page/component that owns them.
-   Fail with a useful message when a locator matches zero or multiple
    controls.

## 7. Handling Dynamic DOM and Synchronization

The application may replace field elements when switching tabs or
scrolling. A previously stored `WebElement` can then become stale.

The framework should: - Store stable locator definitions rather than
retaining `WebElement` references across tab changes or re-renders. -
Navigate to the required tab and wait for its content to become visible
before locating the field again. - Re-find a field immediately before
interacting with it when the UI may have re-rendered. - Use explicit
waits for meaningful conditions such as visibility, clickability,
selected state, or a save-completion signal. - Use a limited, targeted
retry only when the failure is known to be transient and the action is
safe to repeat. - Capture the field, tab, action, and failure details
when a timeout or stale-element error occurs.

The framework should not use fixed `Thread.sleep` calls as its normal
synchronization strategy. It should also not blindly retry a save or
another state-changing operation, because the first attempt may already
have succeeded.

## 8. Test Data Management

Test values should be maintained outside the test code in a
version-controlled JSON file. Environment-specific settings, such as the
base URL and browser configuration, should be kept separately in
properties or CI environment variables.

Example JSON structure (illustrative only):

``` json
{
  "testName": "configuration-persistence",
  "fields": [
    {
      "tab": "Transfer",
      "name": "Daily Limit",
      "type": "text",
      "expectedValue": "15000"
    },
    {
      "tab": "KYC",
      "name": "Verification Required",
      "type": "checkbox",
      "expectedValue": true
    }
  ]
}
```

The sample demonstrates the data model only; it is not the complete
12-field regression dataset and its names are not verified against the
application. The actual dataset should include at least 12 fields across
at least 4 real tabs once the UI definitions are known.

### Data rules

-   Validate required keys, supported field types, and duplicate field
    identifiers before test execution.
-   Avoid embedding credentials, tokens, customer data, or other secrets
    in JSON or source control.
-   Use approved, non-sensitive test values.
-   Use unique per-run values only where uniqueness is required and the
    application supports it.
-   Keep expected values explicit and easy to review.
-   Make the target environment clear to prevent accidental execution
    against production.

## 9. Shared Environment, Cleanup, and Concurrency

Because the console changes shared configuration and the environment may
not support an on-demand database reset, cleanup must be designed as
part of the test.

Recommended safeguards: 1. Run only against an authorized test
environment and approved settings. 2. Capture original values before
making changes, when permitted. 3. Coordinate execution with other users
and automation suites. 4. Avoid parallel execution of tests that modify
the same global configuration. 5. Restore only values that the test is
authorized to restore and that have not been changed by another owner
during execution. 6. Verify restored values and report cleanup status
separately from the test result. 7. If cleanup fails, preserve
diagnostic details and notify the responsible owner rather than silently
continuing.

A `finally` block can attempt cleanup during normal test teardown, but
it cannot guarantee recovery after a machine, browser, or process
failure. If the application supports configuration versioning, isolated
test tenants, or a safe administrative reset, those capabilities should
be preferred.

## 10. Reusability and Scaling

### Adding a field

For an already supported control type, adding a field should normally
require a new entry in the JSON dataset rather than a new test method.
The framework should resolve the tab, locate the field, use the
appropriate handler, save, and verify persistence through shared logic.

### Adding a new tab

If the new tab follows the existing navigation pattern, it should
require only configuration and possibly a small page/component update.
If it has different navigation or loading behavior, that behavior should
be encapsulated in the relevant component rather than added as
special-case logic throughout the test.

### Adding a new control type

Add a new `FieldHandler` implementation and focused tests for that
handler. Existing handlers and test flows should remain unchanged where
possible.

### Growing from 7 to 20 tabs

-   Keep field definitions data-driven and group them by tab or
    configuration area.
-   Separate a small smoke suite from the full configuration regression
    suite.
-   Use tags/groups to select suites for pull requests, scheduled runs,
    and releases.
-   Measure execution duration and failure rate from actual runs before
    setting performance targets.
-   Parallelize only independent tests or tests with isolated data.
-   Publish reports and diagnostic artifacts consistently in CI.
-   Review flaky tests and synchronization problems rather than masking
    them with broad retries.

No execution-time estimate is asserted because the application,
environment, browser setup, and actual field count could not be
measured.

## 11. CI/CD and Reporting

Jenkins can run the suite on a schedule or as part of an approved
pipeline.

Proposed pipeline stages: 1. Check out the automation repository. 2.
Prepare the required Java version and browser/WebDriver setup. 3. Load
environment-specific configuration and credentials from secure CI
storage. 4. Run the selected Maven/TestNG suite, for example with
`mvn clean test`. 5. Publish the TestNG results and, if configured,
Allure results. 6. Preserve relevant screenshots and logs for failed
tests while avoiding sensitive information. 7. Report test failures and
cleanup failures distinctly.

The pipeline should return a failed status when a required persistence
assertion fails. Reports should include the environment, test name,
tab/field, expected value, observed value, failure phase, and cleanup
status, while masking sensitive values where necessary.

## 12. Risks and Mitigations

  -----------------------------------------------------------------------
  Risk                                Mitigation
  ----------------------------------- -----------------------------------
  Dynamic IDs or DOM re-rendering     Use stable wrapper/label-based
                                      locators and re-find elements after
                                      navigation or re-rendering.

  False positive after save           Reload the page and assert the
                                      persisted value of every tested
                                      field.

  Tests overwrite another user's      Coordinate execution, avoid
  changes                             shared-setting parallelism, and
                                      restore only when safe and
                                      authorized.

  Test data becomes inconsistent with Validate the data file and review
  the UI                              it against the real UI when
                                      available.

  Flaky timing                        Use explicit waits tied to
                                      application state and capture
                                      diagnostics.

  Sensitive values appear in          Use non-sensitive test data and
  artifacts                           redact logs/screenshots where
                                      required.

  Cleanup cannot complete             Report cleanup separately and alert
                                      the environment owner.
  -----------------------------------------------------------------------

## 13. Acceptance Criteria for This Design

The design is considered ready for implementation when: - The project
responsibilities and framework layers are documented. - The actual UI
has been inspected, or the lack of HTML has been explicitly accepted as
a limitation. - The regression dataset contains at least 12 valid fields
across at least 4 actual tabs. - Each supported control type has a
defined set, read, and verification strategy. - The workflow saves,
reloads, and verifies each expected value. - Dynamic DOM handling and
explicit synchronization are covered. - Shared-environment cleanup and
concurrency controls are documented. - CI execution, failure reporting,
and cleanup reporting are defined.

## 14. Conclusion

A Java, Selenium WebDriver, TestNG, and Maven framework using Page
Object Model and reusable field handlers provides a practical foundation
for this configuration-persistence scenario. Data-driven tests, stable
locators, explicit waits, post-reload assertions, safe cleanup, and
controlled CI execution are the key design decisions.

Because the referenced HTML file was not available and the assessment
instructions confirmed it could be disregarded, the field names and
locator details remain illustrative. They must not be presented as
verified application details.
