# Best Practices

### 1. Intent — Why AI in Engineering

AI is a **productivity accelerator**, not a decision maker. Its role is to:

* Reduce **repeat effort** and redundant work
* Improve **speed and consistency**
* Increase **estimation confidence**
* Maintain **engineering ownership, quality, and accountability**

AI should _augment human expertise_, not replace it. Decisions about architecture, design, and production outcomes must remain with engineers. This aligns with the workshop framing that AI is a **developer tool, not a human substitute**.

> **Pro tip:** Whenever a prompt produces code, treat it as a first draft, _not_ a final commit.

***

### 2. Where AI _Should_ Be Used (Bounded Primary Expectations)

#### 2.1 Standard & Repeatable Work

AI excels at predictable, template-based work.

**Use AI for:**

* CRUD modules scaffolding (Admin panels, APIs, Models)
* Boilerplate code generation
* Validation logic
* DTOs, schemas, contracts
* Standard third-party integrations
* Auto-generated tests and mocks
* First-pass documentation drafts

**Expectation:**\
If a task is _manual and repeatable_, the effort should start with AI-assisted generation and then be refined by an engineer. _Do not manually rewrite what can be responsibly generated and improved._

**Example:**\
Instead of writing 80% of the CRUD layer manually, generate it via AI and focus your energy on business rules, edge cases, and correctness.

#### 2.2 Optimization & Refactoring

AI should help you see improvements, not enforce them.

**Useful AI outputs:**

* Suggestions for performance improvement
* Query and schema optimization ideas
* Reductions in cyclomatic complexity
* Cleaner, more readable code suggestions

**Expectation:**\
Treat these as **ideas, not mandates**. Engineers must review, benchmark when appropriate, and selectively apply.

**Example:**\
If an AI suggests an index change, test performance impact before applying it.

#### 2.3 Support & Maintenance

AI can accelerate understanding of complex issues.

**Applications:**

* Root cause analysis suggestions
* Log interpretation summaries
* Drafts of knowledge base entries and runbooks

**Expectation:**\
AI accelerates **hypothesis generation**, not **production fixes**. Always validate with context and human judgment.

**Example:**\
AI summarizes a log pattern - the developer uses that summary as a starting point for investigation.

***

### 3. Where AI _Should Not_ Be Used (Hard Boundaries)

AI output must **never** be treated as a source of truth or authority in:

* Core business logic decisions
* Architecture direction and ownership
* Implementing security-critical systems
* Reproducing license-protected or proprietary code
* Production fixes applied without human verification
* Prompts that include client data or confidential logic

**Expectation:**\
**Engineers** are accountable for outcomes. not AI.

> **If you cannot explain it or justify it to a peer, do not commit it.**

***

### 4. Quality & Ownership Expectations

All AI-generated content **must** be:

* Reviewed line-by-line by a competent engineer
* Understood completely (able to explain it without referencing AI)
* Covered with tests that verify correctness
* Owned by the human developer

> **“AI-generated” is not an excuse for bugs.**

**Expectation:**\
If you _cannot explain why the code does what it does_, it _does not go into the codebase_.

***

### 5. Estimation & Planning Expectations

AI-assisted work **still takes effort**.

When planning, break down tasks into:

1. **AI-assisted generation**
2. **Customization and validation**
3. **Testing and integration**

**Expectation:**\
Do not estimate AI output as “free.” Your estimates should become _more predictable_, not fuzzier, with AI assistance.

***

### 6. Standardization Expectations

Develop a **shared prompt library** and enforce reuse of high-quality prompts.

* Standardize prompts for recurring tasks
* Review and improve prompts periodically
* Avoid ad-hoc prompt patterns

**Expectation:**\
Reusable prompts save time and avoid inconsistent results.

***

### 7. Accountability Model

Establish clear ownership hierarchy:

* **AI is a tool**
* **The developer is accountable**
* **Tech Lead approves patterns**
* **An architect enforces constraints**

No one defers responsibility to AI.

***

### 8. Success Indicators (What R\&D Will Be Measured On)

You should measure adoption based on:

* Faster delivery on standard modules
* Reduced rework on repetitive components
* Improved estimation accuracy
* Fewer defects in routine patterns
* Higher reuse of standardized solutions

***

### 9. Bottom Line

> **Use AI to accelerate what is known.**\
> **Do&#x20;**_**not**_**&#x20;use AI to guess at what is unknown.**

AI should make predictable work predictable **faster** while leaving hard judgment calls where they belong - with humans.

