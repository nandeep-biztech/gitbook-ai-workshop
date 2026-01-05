# AI as a Force Multiplier (Not a Crutch)

## Force Multiplier

A force multiplier is something that **amplifies your existing ability**.

In development terms

> **A strong engineer + AI = faster decisions, less waste, higher output**
>
> **A weak engineer + AI = faster mistakes, more rework**

AI does not create skill. It **multiplies whatever is already there**.

## Not a Crutch

> A **crutch** is something you rely on to compensate for a lack of ability, instead of developing or using that ability yourself.

A crutch is something you **lean on because you cannot walk properly**.

In development terms:

* Using AI because you do not understand the problem
* Letting AI decide business logic
* Copy-pasting code without reasoning
* Skipping design and validation because “AI generated it.”

That is dependency, not productivity.

***

## Common Failure Pattern

* Minimal prompt
* Blind acceptance of output
* Late discovery of issues
* Rework and debugging increase

This is misuse, not AI failure.

## Three Non-Negotiable Rules

1. **You must provide context**
   * Stack, constraints, intent, scope
   * AI does not infer business meaning
2. **You must validate the output**
   * AI produces plausible code, not guaranteed correct code
   * Review is mandatory, not optional
3. **You must own the final decision**
   * AI does not take responsibility
   * Production impact is always on you

If any of these are violated, productivity drops instead of improving.

***

## Practical Meaning for Developers

### Wrong Usage (Crutch):

* Blindly copy-paste generated code
* Letting AI decide core logic
* Treating AI output as “good enough.”
* Skipping review because “it looks fine.”

**Result:**

* Subtle bugs
* Incorrect domain behavior
* Hard-to-maintain code

### Correct Usage (Force Multiplier):

Utilize AI to automate mechanical and repetitive tasks.

#### AI should generate:

* Boilerplate code
* DTOs and interfaces
* Validation schemas
* Database migrations
* Test scaffolding

This reduces typing, not thinking.

{% hint style="success" %}
AI should help you make better decisions faster, not make decisions on your behalf.
{% endhint %}

***

### What You Must Write Yourself

#### You own:

* Business logic
* Domain rules
* Edge cases
* Failure handling
* Trade-offs and decisions

These require context that AI does not have.

***

### Tool Usage in This Flow

* **GitHub Copilot**\
  Inline suggestions and repetitive patterns
* **Cursor**\
  Context-aware generation, refactors, multi-file changes
* **Codeium**\
  Alternative inline completion and suggestions

Tools assist execution.\
They do not replace reasoning.

***

### Key Rule

**If AI writes more than 70% of your core business logic, you are doing it wrong.**

At that point:

* You are not an engineer
* You are delegating understanding
* You are increasing long-term risk
