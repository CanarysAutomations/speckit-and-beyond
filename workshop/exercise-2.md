# Exercise 2: Agent Skills — Issue Analyzer

> **Time:** ~10 minutes
> **Standalone:** No prior exercises needed.

## Goal

Create a custom agent skill that turns a raw stack trace into a structured engineering report.

---

## Context

`search.py` in the recipe-manager project crashes for ~30% of users:

```
TypeError: 'NoneType' object is not iterable
  File "search.py", line 447, in filter_by_dietary
    for restriction in user.dietary_restrictions:
```

You will create a skill that diagnoses this kind of error automatically.

---

## Steps

**1.** Open Copilot Chat and click **Configure** (gear icon, top-right of the chat panel).

Select **Skills**, then click **New Skill**.

![Configure Menu - Select Skills](assets/skills.png)

In the file dialog:
- Navigate to `.github\skills` as the save location
- Enter skill name: `issue-analyzer`
- Click **Save**

> VS Code creates `.github/skills/issue-analyzer/SKILL.md`.

---

**2.** Replace the entire content of `SKILL.md` with:

```markdown
---
name: issue-analyzer
description: Expert at diagnosing production errors, analyzing stack traces, and creating structured issue reports. Use keywords like: error analysis, stack trace, bug diagnosis, production issues.
---

# Issue Analyzer Skill

When given an error log or stack trace, you:
1. Extract the root cause
2. List affected files and line numbers
3. Rate severity (Critical / High / Medium / Low)
4. Estimate user impact
5. Suggest an immediate fix and a long-term fix

## Output Format
- **Title:** [Component] Brief description
- **Severity:** Critical / High / Medium / Low
- **Root Cause:** Technical explanation
- **Affected Files:** With line numbers
- **Impact:** Who is affected and how
- **Immediate Fix:** Quick patch
- **Long-term Fix:** Proper solution
```

---

**3.** In the terminal ```(ctrl+`)```, run the test and select the crash section of the output:

```bash
cd recipe-manager
python test_bug.py
```

Select the lines from `Testing search with user who has dietary_restrictions=None...` through the `TypeError` line from integrated terminal output.

---

**4.** In Copilot Chat, enter:

```
Look at #terminalSelection using #issue-analyzer analyse the production error
```

| Syntax | What it does |
|--------|--------------|
| `#terminalSelection` | Attaches the crash output you selected |
| `#issue-analyzer` | Loads your custom skill |

---

## Expected Output

```
Title: [Search] Null handling error in dietary restrictions filter
Severity: CRITICAL
Root Cause: Line 447 assumes dietary_restrictions is always a list,
            but it can be None for users with no preferences set.
Affected Files:
  - search.py:447  (primary failure)
  - models.py      (User model allows None)
Impact: ~30% of searches fail
Immediate Fix: Add null guard before line 447
Long-term Fix: Add a dedicated validation layer
```
