# Experiment 2: Understanding the Real Problem

> **Time:** 3:20 PM - 3:45 PM (25 minutes)  
> **Status:** Issue #247 documented. But is it just a null check?

## 🔍 The Question

Issue #247 says: *"Refactor search architecture - this bug exposes larger design issues"*

Is this true? Or should we just add `if x is None` and move on?

**Your mission:** Use a Custom Agent to perform deep architectural analysis.

---

## 🎯 Learning Objectives

- ✅ Create custom agents with specialized expertise
-  Use `.agent.md` files to give agents domain knowledge
- ✅ Perform autonomous codebase-wide analysis
- ✅ Identify systemic issues beyond surface symptoms

**Agent Capability:** Custom Agents (specialized intelligence)

---

## 📝 Exercise 2.1: Build Search Architect Agent (10 min)

### Task
Create a custom agent that's an expert in search systems.

### Steps

**2.1.1** Create `.github/agents/search-architect.agent.md`:

```markdown
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

## Behavior
- **Scan entire subsystem**, not just bug location
- **Provide concrete evidence** from actual code
- **Prioritize recommendations** by business impact
- **Think long-term**: What breaks at 100M users?
```

**2.1.2** Save and restart VS Code.

### What You Created
A specialist agent with **deep domain expertise** in search systems.

---

## 📝 Exercise 2.2: Invoke Deep Analysis (8 min)

### Task
Ask your architect agent to analyze Issue #247 deeply.

### Steps

**2.2.1** In Copilot Chat:

```
@search-architect Review GitHub Issue #247. The null handling bug is the symptom. 
What are the real architectural problems? Should we patch or redesign?

Context: search.py is 847 lines. Users complained about slow searches before this bug.
```

### Expected Analysis

```
DEEP ARCHITECTURAL ANALYSIS - Issue #247

SURFACE PROBLEM: Null handling at line 145
Fix time: 5 minutes
Impact: Stops immediate bleeding

ROOT PROBLEM DISCOVERED (scanned 12 files):

1. ARCHITECTURE SMELL: God Object Anti-Pattern
   File: search.py (847 lines)
   - Lines 1-200: Query parsing logic
   - Lines 201-450: 7 different filters混淆一起 (dietary, cuisine, time, etc.)
   - Lines 451-680: Ranking algorithm
   - Lines 681-847: Response formatting
   
   ISSUE: Single file doing 4 different jobs. Violates Single Responsibility Principle.

2. RELIABILITY ISSUES (23 found across search subsystem)
   - search.py:145 (reported bug)
   - search.py:203, 298, 412 (similar null assumptions)
   - models.py:67, 89, 134 (missing defaults)
   - api/routes.py:201, 245, 301 (no input validation)
   
   PATTERN: No validation layer. Assumes perfect inputs.

3. PERFORMANCE PROBLEMS
   - O(n) linear scan through 2M recipes (search.py:305-450)
   - N+1 query problem loading ingredients (search.py:520)
   - No caching layer (every search hits database)
   - Average response time: 280ms (target: <100ms)

4. MAINTAINABILITY CRISIS
   - Test coverage: 0% for search logic
   - Hard-coded business rules (74 magic numbers found)
   - Zero documentation
   - Last commit: 18 months ago (abandoned code?)

DECISION MATRIX:

Option A: Patch null check
- Time: 5 minutes
- Risk: LOW (simple fix)
- Outcome: Next bug in 2 weeks (21 other null issues waiting)

Option B: Refactor properly
- Time: 2 hours with agents (2 weeks traditional)
- Risk: MEDIUM (architecture change)
- Outcome: Reliable, fast, maintainable search
- Prevents: 6 months of future bugs

RECOMMENDATION: Refactor. This is technical debt bankruptcy.
```

### What Just Happened
Custom agent **thought like an architect**: Found 4 categories of problems, provided evidence, gave strategic recommendation.

---

## 📝 Exercise 2.3: Make the Strategic Decision (7 min)

### Task
Decide: patch or refactor? Then establish governance.

### Steps

**2.3.1** You decide to refactor. Ask:

```
@search-architect We'll refactor. What principles should guide this work? 
What's non-negotiable for FlavorHub's search?
```

### Expected Response

```
PRINCIPLES FOR SEARCH REFACTORING

Based on FlavorHub's 10M user scale and production criticality:

1. PERFORMANCE: <100ms search latency (95th percentile)
   - Current: 280ms average (UNACCEPTABLE)
   - Use caching, indexes, async where possible

2. RELIABILITY: Graceful degradation over crashes
   - Partial results better than errors
   - Input validation at entry point (Pydantic models)
   - Fail-safe defaults (null → empty list)

3. ARCHITECTURE: Clean separation of concerns
   - QueryParser: Validate input
   - FilterEngine: Apply search criteria
   - RankingEngine: Score results
   - CacheLayer: Reduce DB load
   - EACH component: Single responsibility, testable independently

4. TESTABILITY: >85% code coverage
   - Every component has unit tests
   - Integration tests for end-to-end flows
   - Performance tests (regression prevention)

5. BACKWARD COMPATIBILITY: Zero downtime deployment
   - API v2 maintained during transition
   - Database schema changes are additive only
   - Feature flags for gradual rollout

These should become your CONSTITUTION.
```

**2.3.2** Note these principles - you'll use them in Experiment 3.

### What Just Happened
Agent provided **strategic guardrails** that will guide all implementation decisions.

---

## ✅ Checkpoint: What You Accomplished

🎯 **Root cause identified**: Not a bug, it's architectural bankruptcy  
🎯 **Custom agent** scanned 12 files, found 23 issues  
🎯 **Strategic decision**: Refactor, don't patch  
🎯 **Principles established**: Performance, reliability, architecture, tests, compatibility  

**Current Time:** 3:45 PM  
**Status:** We know the problem. Now we need a plan.

---

## 🚀 Next: Experiment 3

We have principles, but no specification. How do we translate *"refactor search"* into actionable tasks?

**Continue to:** [Experiment 3: Designing the Solution](experiment-3.md)

Time to use: **Instruction Files + Spec Kit** for governance-driven design.



