# Exercise 2: Agent Skills — GitHubMCP

> **Time:** ~10 minutes
> **Standalone:** No prior exercises needed.

## Goal

Create a GitHub issue from the analysis using  agent skills and GitHub MCP.

---

## Context

Issue details from Excercise 1 are in hand. Now, let's use GitHub MCP to create a well-formatted issue directly from our analysis, and assign it to the appropriate team for resolution.

---
### Steps

**1.** Add the GitHub Issues skill from the community library:

GitHub maintains a curated collection of reusable skills. Let's add the official `github-issues` skill:

1. Visit [GitHub's Awesome Copilot Skills Library](https://github.com/github/awesome-copilot/tree/main/skills/github-issues)

   ![Awesome Copilot Skills](assets/awesomeskills.png)
   *GitHub's official skills library with community-contributed skills*

2. Create the `github-issues` skill folder structure in your repo inside VS Code or terminal with the following command:
   ```bash
   mkdir -p .github/skills/github-issues/references
   ```

3. Copy the official SKILL.md from GitHub:
   - Navigate to the `github-issues` skill in the [Awesome Copilot Skills repository](https://github.com/github/awesome-copilot/blob/main/skills/github-issues/SKILL.md)
   - Click **Raw** button to view the raw markdown
   - Copy the entire content
   - Paste into your `.github/skills/github-issues/SKILL.md` file (VS Code → File Explorer → .github → skills → github-issues → New File → SKILL.md)
   - Save the file

4. Copy the reference template:
   - Navigate to [issue-template.md](https://github.com/github/awesome-copilot/blob/main/skills/github-issues/references/templates.md) in the same repository
   - Click **Raw** button
   - Copy the entire content
   - Paste into your `.github/skills/github-issues/references/templates.md` file (VS Code → File Explorer → .github → skills → github-issues → references → New File → templates.md)
   - Save the file

**What You Created:**
- Official GitHub Issues skill for consistent formatting
- Reference folder with example templates
- Reusable pattern for future issues

**2.** Reload VS Code window (Ctrl+Shift+P → "Developer: Reload Window")

**3.** In Copilot Chat, create the issue and assign to Copilot:
**Note** When you run the below command, use **#** it refers to list of folders/files so select the appropriate one from the dropdown. 

![Awesome Copilot Skill selection](assets/skillselect.png)
   *GitHub's official skills library with community-contributed skills*
```
Create a GitHub issue based on the #file:issue-analyzer analysis, Use #file:github-issues format and use #mcp_github_assign_copilot_to_issue to fix the issue.
```

**What's happening:**

- `#file:github-issues` - Applies the issue formatting skill
- `#mcp_github_assign_copilot_to_issue` - Automatically assigns the created issue to @copilot agent
- Copilot creates the issue, assigns it to itself, and will create **PR** with a fix

![GitHub Issue Created](assets/issuecreated.png)
*Issue created with proper formatting and automatically assigned to @copilot*


---

## What you did
| Item | Detail |
|------|--------|     
| Skill Creation | Used a custom skill from skills library |
| GitHub MCP | Used GitHub MCP to create and assign an issue based on the analysis |  
