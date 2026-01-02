---
description: Execute With Supervision
---

# AGENT Mode

**This is where Cursor becomes dangerous or powerful**, depending on discipline.

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
