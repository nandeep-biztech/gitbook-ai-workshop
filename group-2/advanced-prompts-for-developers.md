---
description: From Asking Questions to Orchestrating AI
---

# Advanced Prompts for Developers

### 1. What Makes a Prompt “Advanced.”

You should open this section with this statement:

> Advanced prompts are not questions.\
> They are execution contracts.

#### Characteristics of an Advanced Prompt

Before examples, define the pattern.

An advanced prompt usually includes:

1. **Explicit role** (reviewer, architect, SRE)
2. **Tool invocation instructions** (MCP, repo, browser)
3. **Step-by-step execution plan**
4. **Evaluation dimensions**
5. **Tone and strictness constraints**
6. **Output structure**
7. **Action trigger** (“start now”, “fetch data first”)

If any of these are missing, the prompt is intermediate, not advanced.

***

### 2. Prompt Maturity Levels (Explain This First)

#### Level 1. Basic

* “Fix this bug.”
* “Write an API”

Low signal, high hallucination risk.

#### Level 2. Structured

* Clear context
* Clear goal
* Clear constraints

Good for most day-to-day work.

#### Level 3. Advanced (Agentic)

* Multi-step execution
* Tool usage (repo, MCP, browser)
* Explicit evaluation dimensions
* Decision-grade output

This is what senior engineers should use.

***

### 3. Advanced Prompt. Full PR Review Using MCP

#### Use Case

Strict, end-to-end PR review with GitHub and Jira context.

#### Advanced PR Review Prompt

> You are a senior staff engineer performing a full pull request review.\
> Use the GitHub MCP and Jira MCP integrations.
>
> **Execution steps:**
>
> 1. Fetch pull request `<PR_NUMBER>` from `<OWNER>/<REPO>`.
> 2. Load all changed files and commit history.
> 3. Identify and load any linked Jira tickets.
>
> **Analyze the PR across these dimensions:**
>
> * Code correctness and edge cases
> * Architectural impact and coupling
> * Security risks (auth, data exposure, secrets)
> * Performance implications and scalability
> * Breaking changes (API, schema, contracts)
> * Maintainability and readability
> * Test coverage gaps and quality
>
> **Jira cross-check:**
>
> * Validate whether acceptance criteria are fully satisfied.
> * Identify missing edge cases or partial implementation.
> * Flag scope creep or mismatch with the Jira story.
>
> **Output format:**
>
> 1. What is solid and well done.
> 2. Issues, risks, or bad decisions.
> 3. Concrete, actionable improvements.
> 4. Hard blockers that must be resolved before merge.
>
> Be strict and technical. Avoid politeness or generic feedback.\
> Start by fetching the PR details now.

***

### 4. Advanced Prompt. Production Debugging / Incident Analysis

#### Use Case

Root cause analysis during a real production issue.

#### Advanced Debugging Prompt

> You are a senior backend engineer handling a production incident.
>
> **Context:**
>
> * Service: `<service-name>`
> * Stack: Node.js, PostgreSQL, Redis
> * Symptom: Intermittent 500 errors under high load
>
> **Execution steps:**
>
> 1. Analyze the provided logs and stack traces.
> 2. Identify the most likely root causes, ranked by probability.
> 3. Explain why each cause could lead to this behavior.
> 4. Propose fixes with risk assessment.
> 5. Suggest monitoring or alerts that would have detected this earlier.
>
> **Constraints:**
>
> * No database schema changes allowed today.
> * Fix must be backward compatible.
>
> Output a concise incident-style report.

***

### 5. Advanced Prompt. R\&D / Architecture Evaluation

#### Use Case

Design decision with real constraints.

#### Advanced Architecture R\&D Prompt

> You are a principal engineer evaluating architectural options.
>
> **Problem:**\
> Build a real-time notification system.
>
> **Scale and constraints:**
>
> * \~100k concurrent users
> * Latency target < 200ms
> * Stack: Node.js, Redis, PostgreSQL
> * Horizontal scalability required
> * Budget-sensitive, avoid over-engineering
>
> **Execution steps:**
>
> 1. Propose at least three architectural approaches.
> 2. For each approach, analyze:
>    * Data flow
>    * Failure modes
>    * Scaling bottlenecks
>    * Operational complexity
> 3. Compare WebSockets, SSE, and queue-based delivery.
> 4. Recommend one approach with justification.
>
> **Output format:**
>
> * Option comparison table
> * Risks and trade-offs
> * Final recommendation with reasoning
>
> Do not give generic answers. Assume this will be implemented.

***

### 6. Advanced Prompt. Safe Refactoring of Legacy Code

#### Use Case

High-risk refactor with guardrails.

#### Advanced Refactoring Prompt

> You are refactoring legacy production code.
>
> **Context:**
>
> * High cyclomatic complexity
> * Bug fixes are risky
>
> **Goals:**
>
> * Reduce complexity
> * Improve testability
> * Preserve external behavior
>
> **Constraints:**
>
> * No database schema changes
> * No API contract changes
> * Minimal diff size preferred
>
> **Execution steps:**
>
> 1. Identify code smells and structural issues.
> 2. Propose a refactor plan before making changes.
> 3. Apply refactor incrementally.
> 4. Highlight risks introduced by the refactor.
>
> Output a diff and explanation of decisions.

***

### 7. Advanced Prompt. Test Strategy and Quality Gaps

#### Use Case

Improving quality without bloating tests.

#### Advanced Test Strategy Prompt

> You are designing a test strategy for this module.
>
> **Context:**
>
> * Business-critical logic
> * Recent bugs escaped to production
>
> **Execution steps:**
>
> 1. Identify critical business rules.
> 2. Define unit, integration, and edge-case tests.
> 3. Highlight scenarios currently untested.
> 4. Suggest tests that would have caught recent failures.
>
> **Constraints:**
>
> * Tests must be deterministic
> * Avoid excessive mocking
>
> Output a prioritized test plan, not just test code.

***

### 8. Advanced Prompt. MCP-Based Release Readiness Check

#### Use Case

Cross-system, leadership-level automation.

#### Advanced MCP Release Prompt

> You are acting as a release manager.
>
> Use GitHub, Jira, and CI MCP integrations.
>
> **Execution steps:**
>
> 1. Fetch all PRs merged since the last release tag.
> 2. Map PRs to Jira tickets.
> 3. Identify:
>    * Missing or unlinked tickets
>    * Incomplete acceptance criteria
>    * High-risk changes (auth, payments, data)
> 4. Analyze CI results and flaky tests.
>
> **Output format:**
>
> * Release risk summary
> * Must-fix items
> * Go or No-Go recommendation
>
> Be strict. Assume production impact.

***

### 9. Tool and Mode Mapping (Tie It Back to Cursor)

Make this explicit.

* Ask. Explanation and critique only
* Plan. Advanced prompts without code changes
* Agent. Execute after plan approval
* MCP. External, live, large-scale context
* Browser. Validate external facts and APIs

Advanced prompts almost always start in **Plan** or **Agent with MCP**, never casual Ask.

***

### 10. Final Rule to End the Section

End with this line:

> If you are typing one-line prompts, you are operating at a junior level.\
> Senior engineers design prompts the same way they design systems.
