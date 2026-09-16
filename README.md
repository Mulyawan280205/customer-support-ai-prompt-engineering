# Customer Support AI — Product Exchange Eligibility

> **Prompt Engineering Portfolio Project #1**  
> **Domain:** E-commerce / Customer Support  
> **Role:** AI Prompt Engineer

## Overview

This project demonstrates the design and iterative optimization of an AI Customer Support Assistant that evaluates customer product exchange requests strictly according to a company's official exchange policy.

The project focuses on making the AI's decisions more consistent, grounded, and resistant to ambiguous customer inputs.

## Business Problem

A customer may report a product problem without providing enough information to determine whether the issue qualifies for an exchange.

Example:

> "Saya beli headset kemarin, tapi sekarang sebelah kanan tidak mengeluarkan suara. Bisa saya tukar?"

The AI must distinguish between:
- A confirmed accident or customer misuse
- A sufficiently established manufacturing defect
- Missing or insufficient information
- A request that clearly fails a policy requirement

The key challenge is preventing the AI from treating a product malfunction as proof of a manufacturing defect without sufficient evidence.

## Objective

Design a prompt that enables the AI to:

1. Apply the exchange policy consistently.
2. Determine `Eligible`, `Not Eligible`, or `Need More Information`.
3. Distinguish manufacturing defects from misuse or accidents.
4. Avoid unsupported assumptions.
5. Ask for missing information instead of guessing.
6. Ignore policy concepts not present in the supplied knowledge base.
7. Produce structured customer-support responses.

## Prompt Engineering Techniques

- Role Prompting
- Context Engineering
- Rule Definition
- Constraints
- Decision Logic
- Grounding
- Structured Output
- Edge-Case Handling
- Adversarial Testing
- Failure Analysis
- Prompt Iteration

## Policy

The AI uses the following Company Exchange Policy:

1. Customers can request an exchange within 30 days of purchase.
2. The product must have a manufacturing defect.
3. Damage caused by customer misuse or accidents is not eligible.
4. Customer must provide proof of purchase.
5. If information is insufficient to determine eligibility, the AI must ask for the missing information.
6. The AI must not assume a product defect is a manufacturing defect without sufficient evidence.
7. The AI must explain the eligibility decision based on the relevant policy rule.

## Iterative Development

```text
Prompt V1
   ↓
Baseline Testing
   ↓
Failure Analysis
   ↓
Prompt V2
   ↓
Adversarial Testing
   ↓
Failure Analysis
   ↓
Prompt V3
   ↓
Final Evaluation
```

### V1 — Baseline

The initial prompt defined the AI role, policy context, task, and output format.

**Main weaknesses found:**
- Manufacturing-defect evidence was not operationally clear.
- Decision priority was ambiguous when information was missing.
- Next-step behavior was too generic.

### V2 — Structured Decision Logic

V2 introduced:
- Decision criteria
- Strict evaluation order
- Grounding rules
- Decision-specific response behavior
- Edge-case handling

### V2 Adversarial Findings

Two important failure modes were identified:

**1. Negative evidence was treated too close to positive evidence**

The AI could infer:

```text
No accident
    ↓
Manufacturing defect
```

This is invalid. The project established:

```text
Not misuse ≠ Manufacturing defect
```

**2. Unsupported warranty concepts**

A customer could mention a warranty, but the provided exchange policy contained no warranty rules. The AI therefore must not use external warranty assumptions to determine exchange eligibility.

### V3 — Final Prompt

V3 explicitly separates:
- Absence of misuse from proof of manufacturing defect
- Policy requirements from unsupported concepts
- Clear ineligibility from insufficient evidence

It also uses `Need More Information` as the fallback when required information or evidence is insufficient.

## Final Decision Logic

```text
IF purchase period > 30 days
    → Not Eligible

ELSE IF misuse or accident is confirmed
    → Not Eligible

ELSE IF proof of purchase is missing or cannot be verified
    → Need More Information

ELSE IF evidence is insufficient to establish a manufacturing defect
    → Need More Information

ELSE
    → Eligible
```

## Final Evaluation

| Test | Scenario | Result |
|---|---|---|
| 1 | Confirmed accident | ✅ Not Eligible |
| 2 | No accident, insufficient defect evidence | ✅ Need More Information |
| 3 | Missing purchase date | ✅ Need More Information |
| 4 | Purchase exceeds 30 days | ✅ Not Eligible |
| 5 | Missing proof of purchase | ✅ Need More Information |
| 6 | Unsupported warranty reference | ✅ Need More Information |

**Final result: 6/6 test cases produced decisions consistent with the final decision logic.**

## Key Learnings

### 1. Policy text is not the same as decision logic

Rules often need to be translated into explicit conditional logic when consistent decisions are required.

### 2. Negative evidence does not prove the opposite

The absence of misuse does not automatically establish a manufacturing defect.

### 3. Grounding requires explicit boundaries

The model should not invent timelines, warranty terms, procedures, or other conditions outside the supplied policy.

### 4. Ambiguity should have a defined fallback

When evidence is insufficient, the model should request the missing information instead of forcing a classification.

### 5. Adversarial testing is essential

Testing ambiguous and policy-trap cases exposed weaknesses that straightforward examples did not reveal.

## Repository Structure

```text
customer-support-ai/
├── README.md
├── prompts/
│   └── final_prompt.md
├── tests/
│   └── adversarial_tests.md
└── docs/
    └── case-study.md
```

## Project Status

**Completed — Final Prompt V3**

This project demonstrates an iterative Prompt Engineering workflow from baseline prompt design through failure analysis, adversarial testing, and final evaluation.
