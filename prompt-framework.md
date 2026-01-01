# Prompt Framework

### 1. Set the Rule First (Say This Clearly)

> AI quality is directly proportional to **clarity of thinking**.\
> Bad prompt = bad engineering.

If they cannot explain the problem to a human, they cannot prompt an AI.

***

### 2. The Universal Prompt Framework (Simple, Practical)

Before examples, give them a **repeatable structure**:

**Context**

* Language, framework, repo structure
* What already exists

**Goal**

* What you want to achieve

**Constraints**

* Performance, security, compatibility
* What NOT to change

**Output format**

* Code, steps, diff, explanation

Bad prompts miss at least two of these.

***

### 3. Development. Good vs Bad Prompts

#### ❌ Bad Prompt (Typical)

> “Create API for user profile”

Why this fails:

* No stack
* No scope
* No constraints
* No success criteria

AI will hallucinate and over-engineer.

***

#### ✅ Good Prompt

> “We are using Node.js with NestJS and PostgreSQL.\
> Create a REST API to fetch and update user profile data.\
> Fields: name, email (read-only), phone.\
> Authentication already exists.\
> Do not modify auth logic.\
> Provide controller, service, DTO, and validation only.”

Why this works:

* Clear scope
* Clear constraints
* Clear deliverables

***

#### Cursor Mode Tip

* **Plan** → Good prompt
* **Agent** → Only after plan is approved

***

### 4. Debugging. Good vs Bad Prompts

Debugging is where AI saves the most time _if prompted correctly_.

***

#### ❌ Bad Prompt

> “This API is failing. Fix it.”

Why this fails:

* No signal
* No reproducibility
* No context

AI will guess.

***

#### ✅ Good Prompt

> “This NestJS API throws a 500 error intermittently.\
> Stack trace: \[paste]\
> Relevant code: \[paste]\
> Expected behavior: always return 200.\
> Observed behavior: fails under concurrent requests.\
> Analyze root cause, list possible fixes, and rank them by risk.”

Why this works:

* Observed vs expected behavior
* Logs included
* Risk-aware output

***

#### Advanced Debug Prompt

> “Explain this bug as if teaching a junior developer, then propose a fix.”

This exposes reasoning flaws quickly.

***

### 5. R\&D / Exploration. Good vs Bad Prompts

This is where most developers under-use AI.

***

#### ❌ Bad Prompt

> “What is the best architecture for real-time notifications?”

Why this fails:

* No scale
* No constraints
* No business context

You will get generic blog answers.

***

#### ✅ Good Prompt

> “We need real-time notifications for \~50k concurrent users.\
> Tech stack: Node.js, Redis, PostgreSQL.\
> Latency target < 200ms.\
> Compare WebSockets vs SSE vs push queues.\
> Provide pros, cons, and failure modes.”

Why this works:

* Real constraints
* Comparison requested
* Failure modes included

***

#### Cursor Mode Tip

* **Ask** → explore concepts
* **Browser** → validate against real-world docs

***

### 6. Refactoring. Good vs Bad Prompts

***

#### ❌ Bad Prompt

> “Refactor this code to be better.”

Meaningless. “Better” is undefined.

***

#### ✅ Good Prompt

> “Refactor this service to:
>
> * Reduce cyclomatic complexity
> * Improve testability
> * Keep public API unchanged
> * Do not change database schema\
>   Return a diff and explain trade-offs.”

Now AI knows what “better” means.

***

### 7. Testing. Good vs Bad Prompts

***

#### ❌ Bad Prompt

> “Write unit tests for this file.”

Results:

* Shallow tests
* Poor coverage
* No edge cases

***

#### ✅ Good Prompt

> “Generate unit test cases for this service based on its behavior.\
> Cover happy paths, edge cases, and failure scenarios.\
> Use Jest.\
> Do not mock database layer.\
> Focus on business rules, not implementation details.”

***

### 8. Code Review. Good vs Bad Prompts

***

#### ❌ Bad Prompt

> “Review this code.”

You get surface-level feedback.

***

#### ✅ Good Prompt

> “Review this PR focusing on:
>
> * Security risks
> * Performance bottlenecks
> * Breaking changes
> * Missing test cases\
>   Summarize risks first, then suggestions.”

***

### 9. The One Exercise You Should Run Live

This is important.

1. Show a **bad prompt**
2. Let Cursor generate bad output
3. Fix the prompt live
4. Re-run
5. Compare results

This teaches more than 10 slides.

***

### 10. Final Rule You Must Drill In

End the section with this line:

> If AI output surprises you, your prompt was incomplete.

That line sticks.
