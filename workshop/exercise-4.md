# Exercise 4: Spec Kit Setup

> **Time:** ~5 minutes
> **Standalone:** No prior exercises needed.

## Goal

Install the Spec Kit CLI and initialize it in the `recipe-manager` project so the `/speckit.*` slash commands are available in Copilot Chat.

---

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


### What Just Happened
Using Copilot CLI's checklist agent, you generated a prioritized pre-deployment checklist AND received code to fix all critical blockers in one prompt. The code is now production-ready with quality gates passed.

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

## Result

- Spec Kit CLI installed
- `.specify/` folder created in `recipe-manager/`
- Slash commands ready in Copilot Chat
