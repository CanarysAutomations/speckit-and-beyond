# Exercise 4: Spec Kit Setup

> **Time:** ~5 minutes
> **Standalone:** No prior exercises needed.

## Goal

Install the Spec Kit CLI and initialize it in the `recipe-manager` project so the `/speckit.*` slash commands are available in Copilot Chat.

---
## Context
Spec Kit is a powerful tool for defining software specifications, creating implementation plans, and ensuring code quality. In this exercise, you'll set up Spec Kit in the existing `recipe-manager` project to prepare for the upcoming refactoring work.


## Steps

**1.** Install `uv` (required to install Spec Kit):

```bash
# Windows — run in PowerShell
winget install astral-sh.uv

# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```
After installation, you may need to restart your terminal or add `uv` to your PATH. Verify installation:

```bash
uv --version
```

---

**2.** Install Spec Kit:

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

---

**3.** Initialize Spec Kit in the project:


```bash
specify init --here
```

When prompted *"Initialize Spec Kit in existing repository?"*, type **yes**.

![Spec Kit initialization prompt](assets/specsamerepo.png)

---

**4.** Open Copilot Chat and type `/spec` to verify the commands appear:

| Command | Purpose |
|---------|---------|
| `/speckit.constitution` | Define governance principles |
| `/speckit.specify` | Generate a detailed specification |
| `/speckit.plan` | Create the technical plan |
| `/speckit.tasks` | Break plan into tasks |
| `/speckit.implement` | Generate code from spec |
| `/speckit.analyze` | Validate compliance |

![Spec Kit slash commands](assets/specfinal.png)

---
## What you did
|Item|Description|
|----|-----------|
| Spec Kit Installation | You installed the Spec Kit CLI using `uv`. |
| Project Initialization | You initialized Spec Kit in the `recipe-manager` project. |
| Slash Commands | The `/speckit.*` commands are now available in Copilot Chat. |

In the next exercises, you'll use these commands to define the refactor specification, create a plan, and implement the changes.  [Exercise 5: Spec Kit — Constitution & Specification](exercise-5.md)