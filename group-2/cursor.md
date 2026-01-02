# Cursor

### 1. First. Reframe Cursor Correctly (Very Important)

You must say this clearly:

> Cursor is not “ChatGPT inside VS Code”.\
> Cursor is a **context-aware coding assistant with execution authority**.

If devs treat it like a chat box, they waste 70 percent of its value.

Cursor’s power comes from:

* Full repo awareness
* File-level diff generation
* Multi-step planning
* Agentic execution

***

### 2. Cursor Modes Explained. When to Use What

Cursor has **four core interaction modes** that matter in practice:

* Ask
* Plan
* Agent
* Browser (Web)

Most developers randomly use them. That is wrong.

***

### 3. ASK Mode. “Read, Explain, Reason”

#### What it is best at

* Understanding existing code
* Explaining unfamiliar logic
* Answering “why” questions
* Fast reasoning without changing code

#### When to use ASK

Use ASK when:

* You did not write the code
* You are onboarding to a new module
* You are reviewing a PR
* You are debugging and still forming hypotheses

#### Examples

* “Explain this service flow end-to-end.”
* “Why is this query written this way?”
* “What are the edge cases in this function?”
* “Is this code thread-safe?”

#### When NOT to use ASK

* When you want code changes
* When you want refactoring
* When you want files created

**Hard rule**\
ASK = zero side effects.\
If code changes happen here, you are using it wrong.

***

### 4. PLAN Mode. “Think Before You Touch Code”

This is the most underused and most important mode.

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

#### Rule

If a task touches **more than 2 files**, start with PLAN.

***

### 5. AGENT Mode. “Execute With Supervision”

This is where Cursor becomes dangerous or powerful, depending on discipline.

#### What AGENT does

* Modifies multiple files
* Creates new files
* Applies diffs across the repo
* Follows the plan you approved

#### When to use AGENT

Use AGENT when:

* You already understand the problem
* Scope is well-defined
* Changes are mechanical or repetitive

Best use cases:

* CRUD scaffolding
* DTOs, schemas, migrations
* Test generation
* Boilerplate refactors
* Consistent changes across files

#### When NOT to use AGENT

Do NOT use AGENT when:

* Core business logic is unclear
* Security-sensitive logic is involved
* Financial or compliance rules are involved
* You cannot easily review the diff

#### Mandatory discipline

* Always review diffs
* Never “Accept All” blindly
* Treat AGENT as a junior engineer writing code for review

Say this clearly:

> If you cannot review what AGENT generated, you should not have used it.

***

### 6. BROWSER (WEB) MODE. “External Truth Source”

#### What Browser mode is for

* Framework docs
* Library APIs
* Error explanations
* Comparing versions or best practices

#### When to use Browser

Use Browser when:

* You are unsure about library behavior
* You need up-to-date syntax
* You are dealing with breaking changes
* You need examples from official docs

#### When NOT to use Browser

* For internal code understanding
* For company-specific logic
* For proprietary systems

#### Key warning

Browser mode introduces **external assumptions**.\
Always validate against your actual codebase.

***

### 7. MCP (Model Context Protocol). When It Actually Makes Sense

Now, let us talkabout  MCP. Do not hype it. Most teams do not need it yet.

#### What is an MCP in simple terms

MCP lets AI:

* Talk to **structured systems**
* Pull data from tools like:
  * Jira
  * GitHub
  * Databases
  * Internal APIs
* Act with **live context**, not pasted text

#### When MCP is useful

Use MCP when:

* Context cannot be pasted manually
* Data changes frequently
* Decisions depend on the live state

Strong use cases:

* Reading Jira tickets directly
* Analyzing PRs across repos
* Querying internal APIs
* Generating reports from real data

#### When MCP is overkill

Do NOT use MCP when:

* You are coding a single feature
* Context fits in a prompt
* Security controls are unclear

Say this bluntly:

> MCP is for teams, not individuals hacking features.

***

### 8. How Tools Fit Together (This is the mental model)

Teach this workflow explicitly:

1. **Ask**
   * Understand code
   * Clarify intent
2. **Plan**
   * Decide approach
   * Identify risks
3. **Agent**
   * Execute changes
   * Generate boilerplate
4. **Browser**
   * Validate against external truth

If they jump directly to Agent, they are skipping thinking.

***

### 9. What You Should Warn Them About (Non-negotiable)

Be direct here.

* Cursor will confidently generate wrong code
* It will follow bad instructions perfectly
* It will not understand your business context unless you provide it

Rules you should enforce:

* Never paste secrets
* Never trust generated auth logic blindly
* Never let AI define business rules alone

***

### 10. Final Message You Should End With

Say this verbatim or close to it:

> Cursor does not replace engineering skill.\
> It **amplifies** it.\
> If your thinking is weak, Cursor will make your mistakes faster.
