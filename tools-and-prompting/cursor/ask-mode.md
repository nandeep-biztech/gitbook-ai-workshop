---
description: Read, Explain, Reason
---

# ASK Mode

### What it is best at

* Understanding existing code
* Explaining unfamiliar logic
* Answering “why” questions
* Fast reasoning without changing code

### When to use ASK

Use ASK when:

* You did not write the code
* You are onboarding to a new module
* You are reviewing a PR
* You are debugging and still forming hypotheses

### Examples

* “Explain this service flow end-to-end.”
* “Why is this query written this way?”
* “What are the edge cases in this function?”
* “Is this code thread-safe?”

### When NOT to use ASK

* When you want code changes
* When you want refactoring
* When you want files created

> **Hard rule**\
> ASK = zero side effects.\
> If code changes happen here, you are using it wrong.
