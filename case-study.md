# Case Study — Customer Support AI

## 1. Overview

**Role:** AI Prompt Engineer  
**Domain:** E-commerce / Customer Support  
**Use Case:** Product Exchange Eligibility Evaluation

This case study documents the iterative development of a policy-grounded AI prompt for handling product exchange inquiries.

## 2. Problem

A customer may report a product malfunction without enough information to determine whether the issue qualifies for an exchange.

The prompt therefore needs to prevent unsupported assumptions while still producing a useful customer-facing response.

## 3. Approach

The prompt was developed through:

1. Baseline prompt design
2. Test-case evaluation
3. Failure analysis
4. Decision-logic refinement
5. Adversarial testing
6. Final prompt evaluation

## 4. V1 Findings

V1 established the basic role, context, task, and output format.

Testing revealed:
- Insufficient operational definition of evidence
- Ambiguous decision hierarchy
- Generic next-step behavior

## 5. V2 Findings

V2 introduced strict decision ordering and stronger grounding.

Adversarial testing revealed:
- The risk of treating “no accident” as evidence of a manufacturing defect
- The risk of allowing unsupported warranty concepts into the decision

## 6. V3 Improvements

V3 explicitly enforced:

- `Not misuse ≠ Manufacturing defect`
- Missing required information → `Need More Information`
- Confirmed policy failure → `Not Eligible`
- All policy requirements satisfied → `Eligible`
- Unsupported policy concepts must not affect the decision

## 7. Final Evaluation

Six adversarial cases were evaluated:

| Case | Expected | Result |
|---|---|---|
| Confirmed accident | Not Eligible | Pass |
| Insufficient defect evidence | Need More Information | Pass |
| Missing purchase date | Need More Information | Pass |
| >30 days | Not Eligible | Pass |
| Missing proof | Need More Information | Pass |
| Warranty trap | Need More Information | Pass |

**Result: 6/6 cases passed.**

## 8. Key Prompt Engineering Insight

The central failure mode in this project was an invalid inference:

`No evidence of misuse → manufacturing defect`

The final prompt prevents this by treating these as separate conditions. This illustrates why robust prompting requires explicit decision logic rather than relying only on natural-language policy descriptions.

## 9. Skills Demonstrated

- Prompt Design
- Context Engineering
- Grounding
- Conditional Logic
- Constraints
- Structured Outputs
- Edge-Case Handling
- Adversarial Testing
- Failure Analysis
- Iterative Optimization
