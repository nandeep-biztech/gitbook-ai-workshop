---
description: Think Before You Touch Code
---

# PLAN Mode

This is the most **underused and most important mode**.

{% embed url="https://youtu.be/WInPBmCK3l4?si=6l0PmLwCm7VfECtP" %}

#### What PLAN does

* Breaks tasks into steps
* Identifies impacted files
* Surfaces risks and assumptions
* Creates a change strategy without editing files

#### When to use PLAN

Use PLAN when:

* Feature scope is unclear
* Multiple files/modules are involved
* You are touching legacy or fragile code
* You want alignment before execution

#### Examples

* “Plan changes needed to add OAuth login.”
* “Plan refactor to move from REST to GraphQL.”
* “Plan approach to add caching without breaking behavior”

#### Why does this save time

Developers usually:

* Jump straight into coding
* Discover problems mid-way
* Rewrite work

PLAN eliminates this churn.

> **Rule**
>
> If a task touches **more than 2 files**, start with PLAN.

### Real Life Example

In one of my projects, I need to **implement end-to-end TOTP/MFA**. For this, multiple tickets have been created in Jira. I have utilized proper prompting and the MCP with plan mode in Cursor to develop an effective plan for executing the steps one by one.

<details>

<summary>Row Prompt</summary>

https://proofed.atlassian.net/browse/PP-1556&#x20;

https://proofed.atlassian.net/browse/PP-1555&#x20;

https://proofed.atlassian.net/browse/PP-1554&#x20;

https://proofed.atlassian.net/browse/PP-1553&#x20;

https://proofed.atlassian.net/browse/PP-1552&#x20;

https://proofed.atlassian.net/browse/PP-1551&#x20;

https://proofed.atlassian.net/browse/PP-1550

&#x20;https://proofed.atlassian.net/browse/PP-1549&#x20;

https://proofed.atlassian.net/browse/PP-1548&#x20;

https://proofed.atlassian.net/browse/PP-1547&#x20;

https://proofed.atlassian.net/browse/PP-1546&#x20;

https://proofed.atlassian.net/browse/PP-1545&#x20;

https://proofed.atlassian.net/browse/PP-1544&#x20;

I have a list of Jira task links, and I want to create a prompt for Cursor since I have connected MCP for Jira in Cursor. I need a clear plan to address all these tickets properly. This should include checking requirements using MCP, reviewing the codebase, and developing an implementation plan. Additionally, I want to check the descriptions and comments in the Jira tickets. Please provide a detailed plan on a ticket-by-ticket basis, as I will implement each ticket individually.

</details>

<details>

<summary>Initial version</summary>

You are an expert software engineer and technical planner. You have access to the following Jira tickets:

https://proofed.atlassian.net/browse/PP-1556 https://proofed.atlassian.net/browse/PP-1555 https://proofed.atlassian.net/browse/PP-1554 https://proofed.atlassian.net/browse/PP-1553 https://proofed.atlassian.net/browse/PP-1552 https://proofed.atlassian.net/browse/PP-1551 https://proofed.atlassian.net/browse/PP-1550 https://proofed.atlassian.net/browse/PP-1549 https://proofed.atlassian.net/browse/PP-1548 https://proofed.atlassian.net/browse/PP-1547 https://proofed.atlassian.net/browse/PP-1546 https://proofed.atlassian.net/browse/PP-1545 https://proofed.atlassian.net/browse/PP-1544

For each ticket:

1. Read the **title, description, acceptance criteria, comments, attachments, and any linked tickets**.
2. Identify the **technical requirements**.
3. Identify possible **edge cases, dependencies, and integration points** in the codebase.
4. If any requirement is unclear, list **specific questions to clarify with product/PO**.
5. Create a **step-by-step implementation plan**, including:
   * Summary of the task
   * Code areas/modules affected
   * DB/Migrations (if any)
   * API/endpoint changes (if any)
   * UI changes (if applicable)
   * Testing plan (unit, integration, e2e)
   * Estimated effort (S/M/L)
   * Risks and unknowns

Output the result as a **ticket-wise plan** in this exact format:

***

#### PP-XXXX —

**Summary**

* One-sentence executive summary

**Requirements**

* Clear bullet list from description + comments

**Clarifications / Questions**

* List of specific clarifying questions

**Affected Code Areas**

* Modules/files likely impacted

**Implementation Steps**

1. ...
2. ...
3. ...

**Testing Checklist**

* Unit tests
* Integration tests
* Manual test cases
* Regression impacted areas

**Estimate**

* Effort (S/M/L)
* Risk (Low/Medium/High)

**Dependencies**

* Any other ticket(s), API, feature flags, backend services

***

Plan for all tickets **one by one** in the order above. If something spans multiple tickets, note it and adjust ordering/dependencies as needed.

</details>

<details>

<summary>Final version for Cursor plan</summary>



You are a senior staff engineer responsible for planning and sequencing implementation work.

You have access to the following Jira tickets:

https://proofed.atlassian.net/browse/PP-1556 https://proofed.atlassian.net/browse/PP-1555 https://proofed.atlassian.net/browse/PP-1554 https://proofed.atlassian.net/browse/PP-1553 https://proofed.atlassian.net/browse/PP-1552 https://proofed.atlassian.net/browse/PP-1551 https://proofed.atlassian.net/browse/PP-1550 https://proofed.atlassian.net/browse/PP-1549 https://proofed.atlassian.net/browse/PP-1548 https://proofed.atlassian.net/browse/PP-1547 https://proofed.atlassian.net/browse/PP-1546 https://proofed.atlassian.net/browse/PP-1545 https://proofed.atlassian.net/browse/PP-1544

#### STEP 1 — FULL ANALYSIS

For each ticket:

* Read **title, description, acceptance criteria, comments, attachments, and linked tickets**
* Extract the **actual functional and technical requirements**
* Identify:
  * Prerequisite tickets
  * Shared models/APIs
  * UI ↔ backend dependencies
  * Feature flags or config dependencies

Do NOT assume ticket order equals implementation order.

***

#### STEP 2 — DETERMINE IMPLEMENTATION SEQUENCE

Based on the descriptions and dependencies:

* Decide **which ticket MUST be implemented first**
* Create a **logical implementation flow**, such as:
  * Schema/backend foundations
  * Core APIs
  * Business logic
  * UI wiring
  * Enhancements/edge cases
* Explicitly justify **why one ticket comes before another**

Produce an ordered list like:

1. PP-XXXX — Reason it must be first
2. PP-XXXX — Depends on PP-XXXX
3. PP-XXXX — UI on top of existing APIs …

***

#### STEP 3 — DETAILED PLAN (TICKET-WISE, IN EXECUTION ORDER)

For EACH ticket (in the finalized implementation order), generate the plan in this format:

***

### PP-XXXX —

**Why This Ticket Is Implemented At This Stage**

* Short, concrete justification

**Summary**

* One-line business + technical summary

**Requirements**

* Bullet list derived from description + comments

**Dependencies**

* Tickets, APIs, DB changes, configs, feature flags

**Clarifications Needed (If Any)**

* Specific questions that block implementation

**Affected Code Areas**

* Backend modules/services
* Frontend components/pages
* Shared libs/utils
* DB collections/tables

**Implementation Steps**

1. Backend changes (models, services, APIs)
2. Frontend changes (state, UI, validations)
3. Integration points
4. Error handling & edge cases

**Testing Checklist**

* Unit tests
* Integration tests
* Manual verification steps
* Regression risks

**Estimate**

* Effort: S / M / L
* Risk: Low / Medium / High

***

#### STEP 4 — FINAL OUTPUT REQUIREMENTS

* Tickets must be presented **only in correct execution order**
* Call out any **blocking ambiguities**
* If two tickets can be parallelized, explicitly mention it
* Be concise but technically precise
* No generic fluff or assumptions

</details>

{% file src="../../.gitbook/assets/ticket planning prompt.mp4" %}

{% file src="../../.gitbook/assets/totp_mfa_implementation_sequence_0cd59901.plan.md" %}

\
**One More Example (Implement GraphQL client)**

{% file src="../../.gitbook/assets/graphql_codegen_setup_7763f86e.plan.md" %}
