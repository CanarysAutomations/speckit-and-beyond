# FlavorHub Recipe Manager - Brownfield Application

> **Warning:** This codebase has **intentional issues** for the workshop. Do NOT use in production!

## About This Codebase

This is FlavorHub's legacy Recipe Manager - a 2-year-old application with technical debt. It has:

- ✅ Working recipe search (mostly)
- ❌ Production bug: Crashes for users without dietary restrictions
- ❌ 1006-line monolith in `search.py`
- ❌ No input validation
- ⚠️ Partial test coverage (tests exist but incomplete)
- ❌ Unstructured logging (print statements, not using logging_config.py)
- ❌ Missing type hints in critical functions
- ❌ Performance issues (280ms average searches)
- ❌ No API documentation

**Your mission:** Fix the crisis and modernize this system using GitHub agents.

---

## The Bug (NULL_DIETARY_BUG)

**Error:**
```
TypeError: 'NoneType' object is not iterable
  File "search.py", line 447, in filter_by_dietary
```

**Impact:** 30% of searches failing (users without dietary preferences)

---

## Setup

### Prerequisites
- Python 3.11+

### Install Dependencies
```bash
cd recipe-manager
pip install -r requirements.txt
```

### Run Application
```bash
python main.py
```

API runs on: `http://localhost:8000`

---

## Project Structure

```
recipe-manager/
├── main.py              # FastAPI application entry
├── search.py            # 1006-line monolith (THE PROBLEM)
├── models.py            # User and Recipe models
├── logging_config.py    # Structured logging configuration
├── test_bug.py          # Bug reproduction script
├── pytest.ini           # Pytest configuration
├── requirements.txt     # Dependencies
├── api/
│   ├── __init__.py
│   └── routes.py        # API endpoints
└── tests/
    ├── __init__.py
    ├── conftest.py      # Test configuration
    ├── test_api.py      # API endpoint tests
    ├── test_models.py   # Model tests
    └── test_search.py   # Search functionality tests
```

---

## Known Issues

1. **🔴 CRITICAL:** Null pointer exception in dietary restrictions filter (Line 447)
2. **🟠 HIGH:** Search performance degrading (280ms average)
3. **🟠 HIGH:** Incomplete test coverage
4. **🟡 MEDIUM:** 1006-line god object in search.py
5. **🟡 MEDIUM:** No input validation
6. **� MEDIUM:** Unstructured logging (print statements instead of proper logging)
7. **🟡 MEDIUM:** Missing type hints in multiple functions
8. **�🟢 LOW:** Missing API documentation

**Fix these using the workshop exercises!**

---

## API Endpoints

### Search Recipes
```bash
POST /api/search
Content-Type: application/json

{
  "query": "pasta",
  "dietary_restrictions": ["vegan"],  # This can be null (BUG!)
  "cuisine": "Italian",
  "max_prep_time": 30
}
```

---

## Workshop Usage

This codebase is designed for the **"Agent-Driven Spec Development"** workshop. 

**Timeline:** 3:00 PM (bug discovered) → 5:00 PM (crisis resolved)

Follow workshop experiments to:
1. Create NULL_DIETARY_BUG with agent skills
2. Analyze root cause with custom agents
3. Design solution with Spec Kit
4. Implement refactoring with Copilot CLI
5. Validate with /speckit.analyze

---

## DO NOT

- ❌ Use in production
- ❌ Fix bugs manually (let agents guide you through workshop)
- ❌ Refactor before completing Experiment 3 (need spec first!)

---

## License

Educational use only. Part of GitHub Copilot workshop materials.
