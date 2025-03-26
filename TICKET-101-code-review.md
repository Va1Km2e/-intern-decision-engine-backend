# TICKET-101 Validation Report

## Overview
TICKET-101 aimed to implement the MVP scope of the loan decision engine. The decision engine is designed to assess loan applications based on a personal code, requested loan amount, and loan period, returning either an approval decision with the maximum loan amount or a rejection with an alternative loan offer.

## What Was Done Well
- The code is well-organized, following the separation of concerns principle. The controller only manages requests and passes business logic to the service layer, making it easier to maintain.

- Custom exception handlers are used to manage errors in one place, keeping the controller clean and simple.

- The application compiles and runs without issues, and the main features work as expected.

- The main API endpoint correctly handles different inputs, returning the right responses and error codes.

- The project structure (e.g., config, exceptions, service) is clear, making it easy for developers to find and understand the logic.

- DTOs help separate internal processing from external data, keeping things organized.

- Unit and integration tests check both normal and edge cases, ensuring the system works reliably.

- Lombok is used to reduce repetitive code by automatically creating getters, setters, and constructors, making the code cleaner.










## Areas for Improvement

- Rename `endpoint` to `controller` for consistency with Spring.
- Move DTOs (`DecisionRequest.java`, `DecisionResponse.java`) to a `dto` package and add `DTO` to their names.
- Use singular `exception` instead of `exceptions` for package naming.
- Rename `Decision.java` to `DecisionDTO.java` and move it to `dto`.
- Rename `DecisionEngine.java` to `DecisionEngineService.java` to clarify its role.

### 2. Missing Input Validation
- Add `@Valid` in the controller to validate input and prevent errors.

### 3. Better Error Handling
- Replace `try/catch` in the controller with a global exception handler for cleaner code.

### 4. Inconsistent Data Types
- Standardize `loanAmount` and `loanPeriod` types (avoid mixing `Long`, `int`, and `Integer`).

### 5. Unnecessary `null` Values
- Remove unused fields in API responses (e.g., no `errorMessage: null` in success responses).

### 6. Conflicting Constants
- Ensure `MAXIMUM_LOAN_PERIOD` aligns with business rules (60 vs. 48 months).

### 7. Violation of the Single Responsibility Principle (SRP)
- The `calculateApprovedLoan` method performs multiple tasks: input validation, credit modifier determination, and loan decision-making.
- **Suggested Fix:** Split validation into a separate method, extract credit modifier logic into a helper, and separate loan decision logic.

## Most Important Shortcoming
**Lack of Credit Score Calculation in the Loan Approval Logic**
- The lack of credit score calculation was the biggest issue because it allowed loan approvals without assessing customer risk, while other issues, though not ideal, did not break core functionality; other issues were left unchanged as I was specifically asked to fix only the most critical problem.
- **FIX:** The credit score calculation was correctly implemented, ensuring that loans are only approved when the credit score is above the defined threshold of 0.1, adhering to business rules and reducing financial risk.
## Conclusion
The initial implementation of TICKET-101 covered the essential functionality but had significant flaws in its loan decision logic and code structure. After implementing the necessary fixes, the decision engine now uses the correct loan approval formula