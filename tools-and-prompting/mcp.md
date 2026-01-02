---
description: Model Context Protocol
---

# MCP

**Model Context Protocol (MCP).** This is an open standard introduced by Anthropic in November 2024 that allows AI systems, such as large language models (LLMs) and AI agents, to connect with and interact with external data sources, tools, and services in a standardized way. It enables AI to access real-time information, perform actions (like sending emails or running code), and maintain context across different tasks, moving beyond the limitations of their original training data.

#### What is an MCP in simple terms

MCP lets AI:

* Talk to **structured systems**
* Pull data from tools like:
  * Jira
  * GitHub
  * Databases
  * Internal APIs
* Act with **live context**, not pasted text

#### When is MCP useful

**Use MCP when:**

* Context cannot be pasted manually
* Data changes frequently
* Decisions depend on the live state

**Strong use cases:**

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
