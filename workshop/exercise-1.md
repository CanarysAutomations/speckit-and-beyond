# Exercise 1: Reproduce the Bug

> **Time:** ~8 minutes
> **Standalone:** No prior exercises needed.

## Goal

Run the failing test, read the crash output, and locate the exact line in `search.py` that causes it.

---

## Steps

**1.** Open a terminal and navigate to the recipe-manager folder:

```bash
cd recipe-manager
```

**2.** Run the bug reproduction script:

```bash
python test_bug.py
```

**3.** Read the crash output carefully. Notice which test case crashes and what the error message says.

**4.** Open `recipe-manager/search.py` and jump to **line 447**:

```python
for restriction in user.dietary_restrictions:  # CRASHES IF None!
```

---

## Expected Output

```
Testing with Alice (has dietary restrictions)...
Search succeeded!

Testing search with user who has dietary_restrictions=None...
CRASH! (This is the NULL_DIETARY_BUG)
Error: 'NoneType' object is not iterable

Stack trace points to search.py line 447:
  for restriction in user.dietary_restrictions:
  TypeError: 'NoneType' object is not iterable

Testing with Bob (SAMPLE_USERS[1])...
CRASH! Same issue as production
```

---

## What You Found

| Item | Detail |
|------|--------|
| Bug | `dietary_restrictions` can be `None` for users with no preferences set |
| Location | `search.py:447` |
| Error | `TypeError: 'NoneType' object is not iterable` |
| Impact | ~30% of searches fail |

You now have the stack trace and context needed for the next exercises.
