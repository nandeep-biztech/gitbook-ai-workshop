---
description: Enforcing Discipline at the Project Level
---

# IDE Rules

Cursor Rules are **project-level instructions** that define how AI should behave inside a repository.

> They are not prompts.\
> They are **constraints**.

[http://git.biztechcs.lan/ebdaa/stock-analysis-platform/ebdaa-stock-analysis-portal/-/tree/main/.cursor?ref\_type=heads](http://git.biztechcs.lan/ebdaa/stock-analysis-platform/ebdaa-stock-analysis-portal/-/tree/main/.cursor?ref_type=heads)

Rules exist to prevent AI from:

* Making inconsistent decisions
* Violating architectural patterns
* Generating noisy or irrelevant output
* Acting differently for each developer

### Why Cursor Rules Exist

Without rules, AI behavior becomes:

* Developer-dependent
* Prompt-dependent
* Inconsistent across sessions
* Hard to review and trust

At the team scale, this leads to:

* Style drift
* Conflicting patterns
* Unpredictable diffs
* Slower reviews

Cursor Rules solve this by enforcing **shared context**.

***

### What Cursor Rules Actually Do

Cursor Rules tell the AI:

* How the project is structured
* Which patterns are allowed
* Which practices are forbidden
* What level of output is acceptable

They apply:

* Automatically
* Consistently
* Across all Cursor interactions

No one has to remember them.\
The AI does.

***

### What to Put in Cursor Rules

#### 1. Architecture Constraints

Define:

* Layer boundaries
* Dependency direction
* Allowed abstractions

Example intent (not syntax):

* Controllers must not access the database directly
* Business logic lives in services
* DTOs are required for API boundaries

#### 2. Coding Standards

Define:

* Language conventions
* Error-handling approach
* Naming rules
* File organization

This avoids:

* Style debates in PRs
* Inconsistent patterns from AI output

#### 3. Tooling Expectations

Specify:

* Testing framework usage
* Linting expectations
* Logging standards

This ensures AI-generated code:

* Compiles
* Lints
* Fits existing pipelines

#### 4. Output Discipline

Tell AI:

* Be concise
* Avoid speculative refactors
* Do not change unrelated files
* Prefer explanations over large diffs

This enforces **signal over noise**.

***

### How Cursor Rules Improve Engineering Outcomes

* **Consistency**
  * All developers get the same AI behavior.\
    No personal prompting hacks.
* **Predictability**
  * AI output follows known patterns.\
    PRs are easier to review.
* **Accountability**
  * Rules are version-controlled.\
    Violations are visible.
* **Signal over Noise**
  * AI stops flooding the codebase with unnecessary changes.

***

### How to Use Cursor Rules Correctly

* Treat rules like documentation, not comments
* Keep them short and opinionated
* Update them when the architecture evolves
* Review changes to rules like code changes

Bad rules are worse than no rules.

***

### ❌ What Not to Do

* Do not write generic rules
* Do not duplicate documentation
* Do not encode temporary decisions
* Do not allow rules to drift from reality

**If rules lie, AI will lie.**

***

### Key Principle

> **Cursor Rules define how AI thinks inside your codebase.**\
> **Weak rules create weak output.**

Rules are not optional at scale.\
They are mandatory if AI is used daily.

***

### Transition to Cursor Modes

With rules in place, AI behavior is now:

* Controlled
* Predictable
* Reviewable

Now we can safely use Cursor modes:

* Ask
* Agent
* Plan
* Debug
* Browser

Each mode solves a different problem.\
Using the wrong one costs time.

{% columns %}
{% column %}
{% embed url="https://youtube.com/shorts/fbR4dBPH5ZI?si=w7mY9iEmAf1xjvzC" %}


{% endcolumn %}

{% column %}
{% embed url="https://youtube.com/shorts/LL2k9Xp04rE?si=zNyb06XtC1hovYJI" %}


{% endcolumn %}
{% endcolumns %}

