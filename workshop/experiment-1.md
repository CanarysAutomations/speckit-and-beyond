# Experiment 1 Creating Order from Chaos

> **Time:** 3:00 PM - 3:20 PM (20 minutes)  
> **Crisis State:** 500+ errors, users angry, no structure

## 🔥 The Situation

FlavorHub's search feature crashed. Production logs show:
```
TypeError: 'NoneType' object is not iterable
  File "search.py", line 145, in filter_by_dietary
    for restriction in user.dietary_restrictions:
```

30% of searches failing. Error reports flooding Slack. No GitHub issue yet. No analysis. Just chaos.

**Your mission:** Create structure from chaos using Agent Skills + GitHub MCP.

---

## 🎯 Learning Objectives

By end of this experiment, you will:
- ✅ Create and invoke agent skills for autonomous problem analysis
- ✅ Use GitHub MCP to automate GitHub operations
- ✅ Transform error logs into structured, actionable documentation

**Agent Capabilities:** Agent Skills (analysis) + GitHub MCP (automation)

---

## 📝 Exercise 1.1: Create Issue Analyzer Skill (8 min)

### Task
Build an agent skill that can analyze production errors intelligently.

### Steps

**1.1.1** Create the skill directory:
```bash
mkdir -p .github/skills/issue-analyzer
```

**1.1.2** Create `.github/skills/issue-analyzer/SKILL.md`:
```markdown
# Issue Analyzer Skill

You are an expert at diagnosing production errors and creating structured issue reports.

## Your Capabilities

When given error logs or stack traces, you autonomously:
1. **Extract root cause** from stack traces
2. **Identify affected files** and line numbers
3. **Assess severity** (critical/high/medium/low)
4. **Estimate impact** (% of users affected)
5. **Suggest immediate hotfix** and long-term solution
6. **Recommend labels** for issue tracking

## Output Format

Always structure your analysis as:
- **Title:** [Component] Brief description
- **Severity:** Critical/High/Medium/Low
- **Root Cause:** Technical explanation
- **Affected Files:** List with line numbers
- **Impact:** User-facing impact description
- **Immediate Fix:** Quick resolution
- **Long-term Fix:** Proper solution approach
```

**1.1.3** Save the file and restart VS Code (for skill to load).

### What You Created
An agent that thinks like a senior engineer - not just reading errors, but **analyzing root causes and impacts**.

---

## 📝 Exercise 1.2: Invoke the Skill (5 min)

### Task
Use your skill to analyze the production error.

### Steps

**1.2.1** Open Copilot Chat (Ctrl+Alt+I / Cmd+Opt+I)

**1.2.2** Invoke your skill:
```
@issue-analyzer Analyze this production error:

[Stack trace]
TypeError: 'NoneType' object is not iterable
  File "search.py", line 145, in filter_by_dietary
    for restriction in user.dietary_restrictions:

[Context]
- Started after deployment v2.3.1
- 547 errors in last 30 minutes
- Only affects users without dietary restrictions set
- Search endpoint: POST /api/search
```

### Expected Output
```
ANALYSIS COMPLETE

Title: [Search] Null handling error in dietary restrictions filter
Severity: CRITICAL
Root Cause: Line 145 assumes dietary_restrictions is a list, 
            but returns None for users with no preferences set
Affected Files: 
  - search.py:145 (primary failure point)
  - models.py:67 (User model returns None)
  - api/routes.py:201 (calls filter function)

Impact: ~30% of searches (users without dietary preferences)
Users see: 500 Internal Server Error

Immediate Fix: Add null check at line 144
  if user.dietary_restrictions is None:
      user.dietary_restrictions = []

Long-term Fix: Refactor search architecture
  - Add input validation layer
  - Use Pydantic models (defaults to empty list)
  - Separate concerns (parsing vs filtering)

Labels: bug, critical, production, search
Estimated Time: 10 min hotfix, 2-4 hours for proper refactor
```

### What Just Happened
Your agent **autonomously analyzed** the error, traced it across 3 files, estimated impact, and proposed both quick and proper fixes.

---

## 📝 Exercise 1.3: Auto-Create GitHub Issue (7 min)

### Task
Use GitHub MCP to automatically create Issue #247.

### Prerequisites
- GitHub Copilot subscription with MCP access
- Repository write permissions

### Steps

**1.3.1** Continue in Copilot Chat:
```
@issue-analyzer Create a GitHub issue in the FlavorHub/recipe-manager repository with your analysis
```

**1.3.2** Agent uses GitHub MCP to:
- Connect to GitHub API
- Create issue with structured content
- Add labels automatically
- Set priority to P0
- Link affected files

**1.3.3** Verify issue created:
```bash
gh issue list --label "critical"
```

### Expected Result

**GitHub Issue #247** appears:
```markdown
Title: [Search] Null handling error in dietary restrictions filter
Labels: bug, critical, production, search
Priority: P0

## Analysis

**Root Cause:** Line 145 in search.py assumes dietary_restrictions 
is a list, but returns None for users without preferences.

**Affected Files:**
- search.py:145
- models.py:67  
- api/routes.py:201

**Impact:** 30% of searches failing (547 errors in 30 min)

## Immediate Fix
[null check code provided]

## Long-term Recommendation
Refactor search architecture - this bug exposes larger design issues:
- 847-line monolith
- No input validation
- Missing separation of concerns

Recommend: Complete search refactoring (see Experiment 2 for analysis)
```

### What Just Happened
Agent **automated the boring stuff**: Created issue, added labels, linked files, structured everything. No manual GitHub clicking.

Time saved: 15 minutes of manual issue creation.

---

## ✅ Checkpoint: What You Accomplished

🎯 **Crisis documented** in 20 minutes vs 1+ hour manually  
🎯 **Issue #247 created** with root cause analysis  
🎯 **Agent Skills** doing autonomous problem diagnosis  
🎯 **GitHub MCP** automating GitHub operations  

**Current Time:** 3:20 PM  
**Status:** Crisis documented. Now we need to understand if this is just a patch or bigger problem...

---

## 🚀 Next: Experiment 2

The issue is created, but @issue-analyzer hinted at deeper problems: *"847-line monolith", "no input validation", "larger design issues"*.

Is this just a null check? Or do we need to refactor?

**Continue to:** [Experiment 2: Understanding the Real Problem](experiment-2.md)

Time to bring in a specialist: **Custom Architect Agent**.



