## Objectives

1. **Identify whether AI models are being used in a way that promotes automation complacency.**  
   Automation complacency refers to excessive reliance on automated systems, which can reduce human vigilance, critical thinking, and active oversight.

   This can lead to problems such as:
   - **Automation bias**: the tendency to trust or favor suggestions from automated systems, even when they are incorrect.
   - **Skill atrophy**: the gradual loss of expertise caused by reduced hands-on engagement and independent reasoning.

2. **Prevent automation complacency by applying a hybrid engineering approach.**  
   In this workflow, the human leads the process and the AI supports implementation.

   To operationalize this approach, we propose a structured loop called **BPIR**.

## BPIR Framework

BPIR consists of four stages that should be followed when working with AI:

- **B — Brief**: The human defines the task context, requirements, constraints, and definition of done.

- **P — Plan**: The AI model proposes an implementation plan, and the human reviews, refines, and approves it before execution begins.

- **I — Implementation**: The AI model generates code iteratively under human supervision using a lazy-iteration approach. Code is produced in manageable chunks, and after each chunk, the human reviews the current state, decides whether it should be accepted or revised, and then determines whether development should proceed.

  Within the implementation stage, it is useful to include **unit tests** for each chunk of code whenever possible. These tests should cover:
  - normal cases
  - edge or extreme cases
  - invalid inputs and error scenarios

  This stage should also include **bug bashes**, meaning the deliberate introduction of unexpected requirements or disruptive changes to evaluate how the model responds. During this process, it is important to assess whether the model:
  - extends interfaces cleanly
  - incorporates new rules without breaking existing logic
  - avoids duplicate code
  - avoids excessive coupling between modules
  - maintains logic that remains understandable and maintainable

- **R — Review**: The human audits the generated solution for logic, security, maintainability, and alignment with the original brief and approved plan.

  This review should include a structured checklist such as:

  - Are all referenced modules, functions, and dependencies real and compatible in version?
  - Is the code avoiding incorrect uses of floating-point arithmetic in financial calculations or incorrect rounding behavior?
  - Are there security risks such as SQL injection, command injection, insufficient input validation, exposure of sensitive information, or endpoints without proper authentication?
  - Is the code respecting the rules, requirements, and constraints established in the brief?
  - Has the implementation silently removed constraints, ignored instructions, or introduced unjustified assumptions?
  - Can the code be broken in an isolated environment using unusual inputs, malformed data, or incorrect information?

  After this review, failures should be documented, along with:
  - what failed
  - why it failed
  - how it was corrected

## Contents

This folder contains my brief and review protocol template, which will be used throughout the course.
