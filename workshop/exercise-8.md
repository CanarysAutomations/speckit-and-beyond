# Exercise 8: Validation & Quality Gates using Copilot CLI

> **Time:** ~10 minutes
> **Standalone:** Can be done with any implementation. Works best after `exercise-7.md`.

## Goal

Use Spec Kit's validation tools to check whether the implementation meets constitution principles — before shipping to production.

---

## Context

Tests pass, but does the code actually satisfy every principle in the constitution?
Spec Kit can run a systematic compliance check and generate a pre-deployment checklist automatically.

---

## Setup: Copilot CLI

Install the Copilot CLI if not already installed:

```bash
npm install -g @github/copilot
```
or
```
# For Windows users, use winget:
winget install GitHub.Copilot
```

Select the Spec Kit agent CLI using /agent:

```
/agent
```
![Spec kit agent selection](assets/copilotagent.png)

In the CLI, select the Spec Kit agent:

![Spec Kit Agent Selection](assets/speckitanalyse.png)

---

## Steps

**1.** Run `/speckit.analyze` in Copilot CLI (or Copilot Chat):

```
Analyze constitution compliance for the search refactoring.

Check:
- NULL_DIETARY_BUG and CACHE_LEAK_BUG fixed with regression tests
- All 4 modules under 300 lines with >80% test coverage
- Type hints 100% complete
- API backward compatible

Identify any critical blockers before deployment.
```

![Running speckit.analyze](assets/speckitanalyse.png)

---

**2.** Review the output. Note any CRITICAL or HIGH blockers.

---

**3.** Run `/speckit.checklist`:

```
Generate a pre-deployment checklist for the search refactoring.

Based on the compliance analysis, create:
1. Critical blockers that must pass before deployment
2. Important items that should pass
3. Nice-to-have improvements

Fix all critical blockers and provide code for each fix.
```

![Running speckit.checklist](assets/speckitchecklist.png)

---

**4.** Apply any fixes the agent suggests, then run the tests if necessary:

```bash
cd recipe-manager
pytest tests/ -v --cov=. --cov-report=term-missing
```
---

## What You Validated

| Check | Tool |
|-------|------|
| Constitution compliance | `/speckit.analyze` |
| Pre-deployment quality gate | `/speckit.checklist` |
| All critical bugs have regression tests | Agent-generated tests |
| Coverage threshold met | `pytest --cov` |

Task Complete! You've validated that the refactor meets the defined principles and is ready for production deployment.
