# Exercise 1: Reproduce the Bug

> **Time:** ~8 minutes
> **Standalone:** No prior exercises needed.

## Goal

Run the failing test, read the crash output, and locate the exact line in `search.py` that causes it.

---

## Steps


## 1. Reproduce the Bug

### Task
Before analyzing the error, let's reproduce it to get the actual stack trace and understand the problem. 

**Steps**


**1.** Navigate to the recipe-manager folder:
```bash
cd recipe-manager
```

**2.** Run the bug reproduction script:

```bash
python test_bug.py
```

**3.** Read the crash output carefully. Notice which test case crashes and what the error message says.

---

## 2.  Create Issue Analyzer Skill 

### Task
Build an agent skill that can analyze errors intelligently.

### Steps

**2.1** Create the skill using Copilot Chat UI:

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



---

**2.2** Edit the generated `SKILL.md` file:

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
### What You Created
An agent that thinks like a senior engineer - not just reading errors, but **analyzing root causes and impacts**.

---

## 3. Invoke the Skill 

### Task
Use your skill to analyze the production error.

### Steps

**3.1** In the terminal where you ran `python test_bug.py`, **select the complete error output (the crash section with stack trace) which sets a context to copilot using #terminalSelection**

**3.2** Open Copilot Chat and invoke your skill:
```
Look at #terminalSelection using #issue-analyzer analyse the errors
```

**What's happening:**
- `#terminalSelection` - Attaches the terminal output you selected
- `#issue-analyzer` - References your custom skill file
- Copilot reads the SKILL.md and applies its analysis format



### What Just Happened
Your agent **autonomously analyzed** the error, traced it across files, estimated impact, and proposed both quick and proper fixes. Let's assign the Copilot coding agent for the immediate fix and investigate deeper architectural issues for the long-term fix.

---

## What You Found

| Item | Detail |
|------|--------|
| Bug | `dietary_restrictions` can be `None` for users with no preferences set |
| Location | `Multiple files` |
| Errors | `TypeError: 'NoneType' object is not iterable`, `Unstructured logging`, `type hints` |
| Impact | ~30% of searches fail |

You now have the stack trace and context needed for the next exercises.
