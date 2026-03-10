# Exercise 1: Reproduce the Bug

> **Time:** ~5 minutes
> **Standalone:** No prior exercises needed.

## Goal

Run the failing test, read the crash output, and locate the exact line in `search.py` that causes it.

---
## Context
The `search.py` module in the `recipe-manager` project is crashing for a significant portion of users. The crash is due to a `TypeError` caused by a `NoneType` object being treated as iterable. 

---
## Steps

**1.** Reproduce the Bug


Navigate to the recipe-manager folder:
```bash
cd recipe-manager
```

Run the bug reproduction script:

```bash
python test_bug.py
```

Read the crash output carefully. Notice which test case crashes and what the error message says.

---

**2.**  Create Issue Analyzer Skill 

Build an agent skill that can analyze errors intelligently.

1. Open **GitHub Copilot Chat** 

2. Click the **⚙️ Configure** button (top-right of chat panel)

3. Select **Skills** from the menu

   ![Configure Menu - Select Skills](assets/skills.png)
   

4. Click **➕ New Skill** button

   ![New Skill Button](assets/newskill.png)
   

5. In the file dialog, select location:

   ![Select Location Dialog](assets/githubskills.png)
   *Choose .github\skills as the location for your skill*
   - Enter skill name: `issue-analyzer`
   - Click Save



Edit the generated `SKILL.md` file:

Replace the template content with:

```yaml
---
name: issue-analyzer
description: Expert at diagnosing production errors and identifying code quality gaps, Analyzes stack traces AND scans codebase for infrastructure issues
---

## Your Capabilities

When given error logs or stack traces, you autonomously:
1. **Extract root cause** from stack traces
2. **Scan codebase** for contributing infrastructure issues
   - Missing test infrastructure
   - Unstructured logging
   - Missing type hints
3. **Identify affected files** and line numbers
4. **Assess severity** (critical/high/medium/low)
5. **Estimate impact** (% of users affected)
6. **Suggest immediate hotfix** and long-term solution
7. **Recommend labels** for issue tracking

```
---

**3.** Invoke the Skill 

Open Copilot Chat and paste your prompt:
```
Look at #terminalSelection using #issue-analyzer analyse the errors
```

**What's happening:**
- `#terminalSelection` - Attaches the terminal output you selected
- `#issue-analyzer` - References your custom skill file
- Copilot reads the SKILL.md and applies its analysis format


---

## What You Did

| Item | Detail |
|------|--------|
| Errors | `TypeError: 'NoneType' object is not iterable`, `Unstructured logging`, `type hints` |
| Location | `Multiple files` |
| Impact | ~30% of searches fail |
| Created | Custom agent skill to analyze errors and code quality gaps |

You now have the stack trace and context needed for the next exercises. [Exercise 2: Agent Skills and Github MCP](exercise-2.md)
