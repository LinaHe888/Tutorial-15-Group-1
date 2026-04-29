# Part B Presentation Outline

Target length: 5 minutes maximum.

Every team member must speak for at least 40 seconds.

## 0:00-0:30 - Target and Scope

Speaker:

- target program
- target asset
- why the testing was authorized
- finding type: confirmed non-zero-day or zero-day candidate

Required visual:

- scope evidence screenshot

## 0:30-1:20 - System and Attack Surface

Speaker:

- what AI/agentic feature was tested
- where attacker-controlled content enters
- what trusted data or tool access the agent has
- trust boundary diagram

Required visual:

- simple data-flow diagram

## 1:20-2:40 - Reproduction

Speaker:

- test account setup
- synthetic canary
- steps to reproduce
- actual observed result
- reproducibility rate

Required visual:

- transcript or screen recording
- evidence of output/action

## 2:40-3:40 - Impact and Severity

Speaker:

- concrete impact
- affected boundary
- why this maps to the selected severity
- what the finding does not claim

Required visual:

- severity mapping table

## 3:40-4:30 - Novelty and Zero-Day Reasoning

Speaker:

- searches performed
- known related reports
- why this exact chain appears new
- limitations of the novelty claim

Required visual:

- novelty evidence list

## 4:30-5:00 - Mitigation

Speaker:

- root cause
- direct fixes
- defense-in-depth controls
- why the fix addresses the demonstrated chain

Required visual:

- before/after trust boundary or mitigation bullets

## Q&A Prep

Prepare short answers for:

- Why is this in scope?
- Why is it not just a jailbreak?
- What makes the impact security-relevant?
- What prevents this from being a duplicate?
- What is the minimum privilege needed by the attacker?
- Could this affect real users?
- What exact mitigation would you implement first?

