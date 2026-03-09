# Exercise 7: Spec Kit — Implement

> **Time:** ~10 minutes
> **Prerequisite:** Spec Kit initialized, `specification.md` and `plan.md` created (`exercise-4.md` through `exercise-6.md`)

## Goal

Use `/speckit.implement` to generate the validation module that fixes the null crash, wire it into the existing codebase, and verify the fix.

---

## Context

Highest priority task: fix the `NULL_DIETARY_BUG` (crash at `search.py:447`) by extracting input validation into a dedicated `validation_module.py`.

---

## Steps

**1.** In Copilot Chat, run:

```
/speckit.implement

Implement validation_module.py following the specification and plan.
Requirements:
- Fix the NULL_DIETARY_BUG (null-safe dietary_restrictions handling from line 447)
- Add input validation for all search parameters
- Full type hints
- Unit tests with >80% coverage
- Importable by search.py without breaking the existing API

Reference: tasks.md Phase 1
```

> Copilot generates `validation_module.py` and a corresponding test file.

---

**2.** Wire the new module into `search.py` using @workspace:

```
@workspace
I have created validation_module.py.
Update search.py to import and use it instead of the inline logic at line 447.
Make sure api/routes.py continues to work without any changes.
```

---

**3.** Run the tests to confirm the fix:

```bash
cd recipe-manager
python test_bug.py
```

Expected:

```
Testing with Alice (has dietary restrictions)...
Search succeeded!

Testing search with user who has dietary_restrictions=None...
Search succeeded!   ← NULL_DIETARY_BUG is now fixed

Testing with Bob (SAMPLE_USERS[1])...
Search succeeded!
```

---

**4.** Repeat for the remaining modules using the same prompt pattern:

```
/speckit.implement

Implement filtering_module.py following the specification and plan.
Requirements:
- Extract all active filter functions from search.py
- Remove the 3 deprecated filter versions
- Unit tests for each filter function
- Same function signatures for backward compatibility

Reference: tasks.md Phase 2
```

Use the same approach for `aggregation_module.py` (Phase 3) and `formatting_module.py` (Phase 4).

---

## What You Did

| Module | Target lines | Bug fixed |
|--------|-------------|-----------|
| `validation_module.py` | ~200 | NULL_DIETARY_BUG |
| `filtering_module.py` | ~300 | — |
| `aggregation_module.py` | ~250 | CACHE_LEAK_BUG |
| `formatting_module.py` | ~150 | — |

Next, you'll validate the refactor against the constitution using `/speckit.analyze` and generate a pre-deployment checklist with `/speckit.checklist`using Copilot CLI. [Exercise 8: Validation & Quality Gates using Copilot CLI](exercise-8.md)
