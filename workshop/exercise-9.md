# Exercise 9: Copilot CLI — Custom Agent Creation & Delegation 

> **Time:** ~8 minutes
> **Standalone:** No prior exercises needed.
> **Track:** 🟡 Optional — Standalone/Alternative to exercise 7

## Goal

Create a custom agent using Copilot CLI that turns a raw stack trace and search bug report into a structured engineering report.

---

## Context

`search.py` in the recipe-manager project crashes for ~30% of users, and users complain about slow responses.
Let's create a **search-architect** agent with specialized expertise to:
- Analyze the entire search system architecture
- Identify root causes at the design level
- Recommend proper solutions, not just patches

---

## Setup: Copilot CLI

Install the Copilot CLI if not already installed:


```
# For Windows users, use winget:
winget install GitHub.Copilot
```
or
```bash
npm install -g @github/copilot
```

---

## Steps

**1.** Open Copilot CLI in the terminal:
```
/ide
```

It shows VS Code open respository list, from that select your current working repository

```
/agent
```

This shows available agents and an option to create a custom one.

---

**2.** Create a new custom agent from the `/agent` flow:

- Choose **Create new agent**
- Select the target path (recommended: `.github/agents`)
- Select **Create manually** (not create with Copilot)
- Name: `search-architect`
- Description: Senior software architect specializing in search systems, scalability, and code architecture. Analyzes search implementations for performance, reliability, and maintainability issues.

When prompted for content, paste the given agent instructions:

```yaml

When prompted for description/content, paste the given agent instructions:

```yaml
---
name: search-architect
description: Senior software architect specializing in search systems, scalability, and code architecture. Analyzes search implementations for performance, reliability, and maintainability issues.
argument-hint: A codebase, file, or GitHub issue to analyze for architectural problems and modernization opportunities.
tools: ['vscode', 'execute', 'read', 'agent', 'edit', 'search', 'web', 'todo'] # specify the tools this agent can use. If not set, all enabled tools are allowed.
---

# Search Architect Agent

## Identity
You are a senior software architect specializing in search systems, scalability, and maintainability.

## Expertise
- Search algorithm design and optimization
- Code architecture patterns and anti-patterns
- Performance analysis and bottleneck identification
- Reliability and fault tolerance patterns

## Context: FlavorHub Recipe Manager
- 2M recipes in database
- 10M monthly active users
- Current search: filter-based, file-based implementation
- Tech stack: Python 3.11, FastAPI, PostgreSQL

## Your Mission
When analyzing search code, you autonomously:
1. Evaluate architecture (monolith vs modular)
2. Identify performance bottlenecks
3. Find reliability issues (not just the reported bug)
4. Assess code maintainability and testability
5. Recommend modernization strategy with priorities
6. Document findings in `search-architect-report.md` in the repo

## Behavior
- **Scan entire subsystem**, not just bug location
- **Provide concrete evidence** from actual code
- **Prioritize recommendations** by business impact
- **Think long-term**: What breaks at 100M users?
```

Save the agent file.

---

**3.** Invoke the new custom agent:

Run again:

```
/agent
```

Then select **search-architect** from the list.

---

**4.** Enter the prompt to assign task to coding agent using CLI:
> **Note:** Coding agent has to be enabled in your Copilot settings for your repository.
```
/delegate 
```
Followed by prompt

```
Review #file:issue-analyzer analysis and analyze search.py comprehensively. 
The null handling bug is just a symptom - what's the real architectural state?

Context: search.py has grown to 1103 lines over 18 months. 
Users complained about slow searches before this bug appeared.
```

---

## What you Did

| Item | Description |
|------|-------------|
| Custom Agent Creation (CLI) | You created a custom agent using `/agent` with manual setup and architect-focused instructions. |
| Deep Analysis | You invoked `search-architect` to perform a comprehensive analysis of the search subsystem, beyond the immediate bug. |



