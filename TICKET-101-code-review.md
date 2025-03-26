# TICKET-101 Validation Report

## Overview
TICKET-101 aimed to implement the MVP scope of the loan decision engine. The decision engine is designed to assess loan applications based on a personal code, requested loan amount, and loan period, returning either an approval decision with the maximum loan amount or a rejection with an alternative loan offer.

## What Was Done Well
- **Good Documentation:** The code includes clear and well-structured comments explaining its functionality.
- **Encapsulation of Business Logic:** The decision engine follows a structured approach by encapsulating validation and decision-making logic within the service class.
- **Exception Handling:** The implementation correctly throws and handles exceptions for invalid inputs (invalid personal codes, loan amounts, and loan periods).
- **Use of Constants:** Business constraints (e.g., loan limits and periods) are stored in `DecisionEngineConstants`, improving maintainability.

## Areas for Improvement
### 1. Violation of the Single Responsibility Principle (SRP)
- The `calculateApprovedLoan` method performs multiple tasks: input validation, credit modifier determination, and loan decision-making.
- **Suggested Fix:** Split validation into a separate method (already done), extract credit modifier logic into a helper, and separate loan decision logic.

- The decision engine does not correctly approve or reject loans based on the given scoring rules.
- **Fix Implemented:** A new function `isLoanApproved(loanAmount, loanPeriod)` was introduced to apply the correct formula.

## Most Important Shortcoming
**Lack of Credit Score Calculation in the Loan Approval Logic**
- The most critical issue was the absence of credit score calculation, which led to loan decisions being made without considering the customer's creditworthiness. This could result in approving loans for high-risk individuals
- **FIX:** The credit score calculation was correctly implemented, ensuring that loans are only approved when the credit score is above the defined threshold of 0.1, adhering to business rules and reducing financial risk.
## Conclusion
The initial implementation of TICKET-101 covered the essential functionality but had significant flaws in its loan decision logic and code structure. After implementing the necessary fixes, the decision engine now uses the correct loan approval formula