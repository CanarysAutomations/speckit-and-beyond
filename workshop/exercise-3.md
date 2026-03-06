# Exercise 3: Custom Agent — Search Architect

> **Time:** ~12 minutes
> **Standalone:** No prior exercises needed.

## Goal

Create a custom agent with search architecture expertise, then use it to analyze whether `search.py` has deeper problems beyond the known crash.

---

## Context

`search.py` in the recipe-manager project has grown to **1006 lines** over 18 months.
There is a known crash at line 447, but a generic Copilot prompt will only patch that bug.
A specialized architect agent will examine the whole system and surface systemic issues.

---

## Steps

**1.** Open Copilot Chat and click **Configure** (gear icon).

Select **Custom Agents**, then click **New Custom Agent**.

![Configure Menu - Select Custom Agents](assets/customagent.png)

In the file dialog:
- Navigate to `.github\agents` as the save location
- Enter agent name: `search-architect`
- Click **Save**

> VS Code creates `.github/agents/search-architect.agent.md`.

---

**2.** Replace the entire content of `search-architect.agent.md` with:

```markdown
---
name: search-architect
description: Senior software architect specializing in search systems, scalability, and code architecture. Analyzes search implementations for performance, reliability, and maintainability issues.
argument-hint: A codebase, file, or issue to analyze for architectural problems.
---

# Search Architect Agent

## Identity
You are a senior software architect specializing in search systems.

## Expertise
- Search algorithm design and optimization
- Code architecture patterns and anti-patterns
- Performance and reliability analysis

## Context: FlavorHub Recipe Manager
- 2M recipes, 10M monthly active users
- Current search: filter-based, file-based Python implementation
- Tech stack: Python 3.11, FastAPI, PostgreSQL

## Your Mission
When analyzing search code:
1. Evaluate architecture (monolith vs modular)
2. Identify performance bottlenecks
3. Find reliability issues beyond the reported bug
4. Assess maintainability and testability
5. Recommend a modernization strategy with priorities
```

---

**3.** Save the file, then reload VS Code:

```
Ctrl+Shift+P  →  Developer: Reload Window
```

---

**4.** In Copilot Chat, click the **Agent** dropdown and select **search-architect**.

![Select Search Architect Agent](assets/customarchitectagent.png)

Enter this prompt:

```
Analyze search.py comprehensively.
The null handling bug at line 447 is known — what is the broader architectural state?
Context: search.py has grown to 1006 lines over 18 months.
Users complained about slow searches before this bug appeared.
```

---

## Expected Output

```
ARCHITECTURAL ANALYSIS

Current State: 1006 lines — GOD OBJECT ANTI-PATTERN

What is in this file:
  - Database connection, query parsing, input validation
  - Filtering logic (5 active + 3 deprecated versions)
  - Dietary restriction handling — THE BUG at Line 447
  - Ranking, A/B testing, response formatting
  - Caching (broken, memory leak), metrics, debug logs

Critical Issues:
  1. Line 447: Null bug crashes 30% of users
  2. God Object: 1006 lines, 0% test coverage
  3. Dead code: 300+ deprecated lines
  4. 74 magic numbers throughout
  5. Broken caching causing memory leaks

Recommendation: Refactor into 4 focused modules —
  validation_module.py, filtering_module.py,
  aggregation_module.py, formatting_module.py
```
