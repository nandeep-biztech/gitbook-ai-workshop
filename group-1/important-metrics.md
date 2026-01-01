# Important Metrics

Before talking about AI, we need to talk about **where time is really lost**.

> If you cannot quantify these, you are optimizing blindly.

## Key Metrics to Think About

* Time spent **reading and understanding code** vs writing new code
* Time lost due to **context switching**
* Pull request **review turnaround time**
* Debugging time per incident
* Rework caused by **unclear or incomplete requirements**

These are the areas where AI can realistically help.

***

## Hard Truth

If you cannot roughly estimate these for your own work:

* You are already inefficient
* Any AI usage you adopt will be unfocused
* You will likely blame tools instead of fixing workflow issues

This workshop assumes you want to improve **measurable outcomes**, not just experiment with AI.

***

## Real-World Example

#### Where Time Is Actually Lost

**Scenario: A “simple” backend change**

### What Actually Happens

**1. Reading and Understanding Code**

* 1.5–2 hours spent:
  * Finding the correct service
  * Tracing controller → service → repository
  * Understanding why things are structured this way

➡ Time spent reading code > time spent writing code

### Context Switching

* ChatGPT in a browser for understanding logic
* Copilot inline suggestions in the IDE
* Cursor chat for refactor or debugging
* Back to the browser to check framework docs

#### Time Lost Due to Tool Juggling

* Switching between AI tools: **15–20 minutes**
* Re-explaining context to each tool: **10–15 minutes**
* Verifying conflicting AI outputs: **10–20 minutes**

➡ 30–60 minutes lost without writing a single line of code

#### Key Problem (This Is Critical)

The problem is **not** too many AI tools.\
The problem is the **unstructured usage** of AI tools.

* The same context was explained multiple times
* Different tools give slightly different answers
* Developer becomes the “context sync engine.”

This increases mental fatigue and slows execution.

### Implementation and PR Review

* Code written in \~1 hour
* PR raised
* Review comes after 1 day
* Reviewer asks:
  * “Why not reuse the existing mapper?”
  * “What about backward compatibility?”
  * “Missing validation”

➡ Rework introduced due to assumptions, not complexity

### Debugging and Rework

* QA reports:
  * Edge case breaks existing consumer
* Root cause:
  * The requirement was incomplete
  * API contract impact not discussed upfront

➡ Another 1–2 hours lost fixing avoidable issues

### Final Time Breakdown

* Reading and understanding code: **2 hours**
* Context switching and waiting: **1 hour**
* Writing code: **1 hour**
* PR review delay and rework: **2 hours**

**Total time: \~6 hours**\
**Actual coding: \~1 hour**

***

### Key Observation

Most of the time was lost **before and after coding**, not during coding.

This is exactly where AI helps:

* Faster code understanding
* Better upfront clarification
* Structured planning
* Stronger PR reviews
* Fewer assumption-driven mistakes

***

### Why This Matters

If you only use AI to:

* “Write code faster”

You will save **minutes**.

If you use AI to:

* Understand code faster
* Reduce back-and-forth
* Improve clarity before coding

You save **hours**.

***

### Hard Truth (Tie Back)

If you cannot roughly estimate:

* How long have you been reading code
* How long do you wait
* How much rework do you do

You are optimizing the smallest part of the problem.

This workshop focuses on identifying **where time is lost** and **how we can improve by using proper AI tools.**
