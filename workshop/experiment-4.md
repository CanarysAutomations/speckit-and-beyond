# Experiment 4: Rapid Implementation

> **Time:** 4:18 PM - 4:48 PM (30 minutes)  
> **Status:** Specification and plan ready. Time to build.

## ⚡ The Challenge

Spec says: Break 1103-line monolith into 4 modules (validation, filtering, aggregation, formatting). Each module independently testable, dead code removed, caching fixed. Traditional time: 3-4 days. You have 30 minutes.

**Your mission:** Use Copilot CLI + Spec Kit to generate 4 clean modules from architectural specification.

---

## 🎯 Learning Objectives

- ✅ Use /speckit.tasks to break specs into actionable work
- ✅ Use Copilot CLI for multi-step operations
- ✅ Use /speckit.implement for component generation
- ✅ Generate code that follows constitution automatically

**Agent Capabilities:** Copilot CLI (automation) + /speckit.implement (code generation)

---

## 📝 Exercise 4.1: Generate Task Breakdown (5 min)

### Task
Turn specification into ordered implementation tasks.

### Steps

**4.1.1** In Copilot Chat:

```
/speckit.tasks
```

### Expected Output

```markdown
# Implementation Tasks: Search Modularization Refactor

## Phase 1: validation_module.py (Priority: P0 - CRITICAL)
□ Task 1.1: Add Pydantic dependency to requirements.txt
□ Task 1.2: Create validation_module.py with SearchQuery model
□ Task 1.3: Add validators for null-to-list conversion (NULL_DIETARY_BUG fix)
□ Task 1.4: Create validate_search_request() entry point
□ Task 1.5: Write comprehensive tests (15 test cases)

## Phase 2: filtering_module.py (Priority: P0 - CRITICAL)
□ Task 2.1: Create filtering_module.py
□ Task 2.2: Move filter_recipes() and helper functions
□ Task 2.3: Remove 3 deprecated filter versions (v1, v2, v3)
□ Task 2.4: Implement optimized filter ordering
□ Task 2.5: Write filter tests (20 test cases, all edge cases)

## Phase 3: aggregation_module.py (Priority: P0 - CRITICAL)
□ Task 3.1: Create aggregation_module.py
□ Task 3.2: Move rank_recipes() with hybrid_v3 algorithm only
□ Task 3.3: Remove 2 old ranking algorithms (basic, weighted_v2)
□ Task 3.4: Replace 12 magic weights with named constants
□ Task 3.5: Fix caching with LRU (CACHE_LEAK_BUG - memory leak)
□ Task 3.6: Write ranking + caching tests (18 test cases)

## Phase 4: formatting_module.py (Priority: P1 - HIGH)
□ Task 4.1: Create formatting_module.py
□ Task 4.2: Move format_search_response() and pagination
□ Task 4.3: Remove half-implemented XML support
□ Task 4.4: Write formatting tests (12 test cases)

## Phase 5: Integration & Cleanup (Priority: P0 - CRITICAL)
□ Task 5.1: Update search.py to import and use 4 modules
□ Task 5.2: Remove old monolithic functions from search.py
□ Task 5.3: Update all imports across codebase
□ Task 5.4: Delete dead code (deprecated functions, old algorithms)
□ Task 5.5: Run integration tests
□ Task 5.6: Verify >80% coverage for each module
□ Task 5.7: Verify API backward compatibility

## Validation Gates
✓ After Phase 1: SearchQuery validates inputs, NULL_DIETARY_BUG fixed
✓ After Phase 2: All filters null-safe, deprecated versions removed
✓ After Phase 3: Single ranking algorithm, caching leak fixed
✓ After Phase 4: Clean JSON formatting only
✓ After Phase 5: 4 modules <300 lines each, >80% coverage, API compatible
```

**4.1.2** Note: Spec Kit understood dependencies automatically.

---

## 📝 Exercise 4.2: Use Copilot CLI for Setup (10 min)

### Task
Use CLI to handle multi-step foundation work.

### Prerequisites

**Install Copilot CLI:**
```bash
npm install -g @githubnext/github-copilot-cli
```

### Steps

**4.2.1** Use CLI for setup tasks:

```bash
"Setup for search modularization refactor:
1. Add pydantic to requirements.txt
2. Create 4 new module files: validation_module.py, filtering_module.py, aggregation_module.py, formatting_module.py
3. Create corresponding test files in tests/
4. Add pytest and pytest-cov to dev dependencies
5. Create .coveragerc configuration for 80% coverage target per module"
```

**4.2.2** Review CLI's suggested commands, then execute:

```bash
# CLI generates these commands (review and run):
pip install pydantic
touch validation_module.py filtering_module.py aggregation_module.py formatting_module.py
mkdir -p tests
touch tests/test_validation.py tests/test_filtering.py tests/test_aggregation.py tests/test_formatting.py
cat >> requirements.txt <<EOF
pydantic>=2.0.0
EOF

cat >> requirements-dev.txt <<EOF
pytest>=7.0.0
pytest-cov>=4.0.0
EOF

cat > .coveragerc <<EOF
[run]
source = .
omit = tests/*,venv/*

[report]
precision = 2
fail_under = 80
EOF
```

### What Just Happened
CLI handled boring setup work - installed dependencies, created 4 module files + test structure, configured coverage following your spec.

---

## 📝 Exercise 4.3: Implement Modules with Spec Kit (10 min)

### Task
Generate the 4 modules from specification using Spec Kit.

**Why Spec Kit:** Best for generating code that must follow specifications exactly.

### Steps

**4.3.1** Implement validation_module.py:

```
/speckit.implement "Create validation_module.py from specification. 
Include SearchQuery Pydantic model with null-to-list validators (NULL_DIETARY_BUG fix).
Add validate_search_request() entry point with comprehensive input validation."
```

**Expected:** Spec Kit creates validation_module.py with:
- SearchQuery BaseModel with all fields
- Validators that convert None to empty list (fixes NULL_DIETARY_BUG)
- validate_search_request() function
- Type hints and docstrings
- Error handling with safe defaults

**4.3.2** Implement filtering_module.py:

```
/speckit.implement "Create filtering_module.py from specification.
Include filter_recipes() main function and all helper filter functions.
Remove deprecated versions. Use optimized filter ordering (most selective first)."
```

**Expected:** Spec Kit creates filtering_module.py with:
- filter_recipes() main orchestrator
- Helper functions (_filter_by_cuisine, _filter_by_dietary, etc.)
- Null-safe by design
- Clean, production-ready code

**4.3.3** Implement aggregation_module.py:

```
/speckit.implement "Create aggregation_module.py from specification.
Include rank_recipes() with hybrid_v3 algorithm only. 
Replace magic numbers with named constants (RELEVANCE_WEIGHT, etc.).
Fix caching with LRU (CACHE_LEAK_BUG)."
```

**Expected:** Spec Kit creates aggregation_module.py with:
- Named constants for all weights
- rank_recipes() with hybrid_v3 only
- LRU caching with @lru_cache decorator
- Clean scoring logic

**4.3.4** Implement formatting_module.py:

```
/speckit.implement "Create formatting_module.py from specification.
Include format_search_response() with pagination.
JSON formatting only (remove XML support)."
```

**Expected:** Spec Kit creates formatting_module.py with:
- format_search_response() main function
- _format_recipe() helper
- Clean pagination logic
- Only JSON support

### What Just Happened
Spec Kit **read your specification and constitution**, then generated 4 clean, modular files following clean architecture principles.

**Spec Kit is ideal for:** Generating multiple components that must precisely follow architectural specifications.

---

## 📝 Exercise 4.4: Wire Modules Together with @workspace (7 min)

### Task
Create clean orchestrator in __init__.py that wires all 4 modules together.

**Why @workspace:** Best for integration with full codebase context.

### Steps

**4.4.1** In Copilot Chat:

```
@workspace Create __init__.py orchestrator that wires all 4 modules together.

Wire these modules with proper function signatures:
- validate_search_request(request_data, user) from validation_module
- apply_filters(recipes, query, criteria) from filtering_module  
- rank_and_cache(recipes, query, criteria) from aggregation_module
- format_search_response(ranked_recipes, page, page_size) from formatting_module

The orchestrator should:
1. Import all 4 modules
2. Call validation_module to fix NULL_DIETARY_BUG (None→[] conversion)
3. Pipeline data through filtering → aggregation → formatting
4. Handle errors gracefully per constitution
5. Maintain API backward compatibility

Keep __init__.py clean (<50 lines), just orchestration logic.
```

### What Just Happened
**@workspace** created clean orchestrator:
- Wired all 4 modules with correct function signatures
- Fixed NULL_DIETARY_BUG at validation layer (None converted to empty list)
- Fixed CACHE_LEAK_BUG in aggregation (LRU caching prevents memory leak)
- Pipeline pattern: validate → filter → rank → format
- Backward compatible API

**@workspace is ideal for:** Wiring components together, integration work with full context.

---

## 📊 Comparison: When to Use Each

| Scenario | Use This | Why |
|----------|----------|-----|
| **Generate new modules from spec** | `/speckit.implement` | Ensures spec compliance automatically |
| **Create multiple files at once** | `/speckit.implement` | Understands module boundaries |
| **Wire components together** | `@workspace` | Better at integration with context |
| **Must follow constitution** | `/speckit.implement` | Reads constitution, enforces principles |
| **Update existing orchestrator** | `@workspace` | Understands existing code structure |
| **Generate tests for each module** | Both work | Spec Kit auto-generates, @workspace on request |
| **Maintain architectural boundaries** | `/speckit.implement` | Constitution-aware |
| **Integration work** | `@workspace` | Sees full codebase context |

**Best practice:** Use both! Spec Kit for spec-driven components, @workspace for custom logic and refinement.

---

## ✅ Checkpoint: What You Accomplished

🎯 **Task breakdown** generated automatically  
🎯 **Setup automated** with Copilot CLI  
🎯 **4 modules created** with /speckit.implement (validation, filtering, aggregation, formatting)  
🎯 **Orchestrator created** in __init__.py wiring all modules with correct signatures  
🎯 **Dead code removed** (3 old filters, 2 old algorithms, XML support)  
🎯 **Caching fixed** (CACHE_LEAK_BUG - LRU eviction)  
🎯 **Null handling fixed** (NULL_DIETARY_BUG - validation converts None→[])  
🎯 **Tests generated** for all 4 modules  

**Agent Capabilities Used:**
- Copilot CLI: Multi-step automation for project setup  
- /speckit.implement: Generate 4 clean modules from architectural spec
- @workspace: Create clean orchestrator maintaining backward compatibility

**Current Time:** 4:48 PM  
**Status:** 4-module architecture implemented. From 1103-line monolith to clean separation of concerns. But is it quality?

---

## 🚀 Next: Experiment 5

Code is written, tests pass. But does it meet our constitution? Are we actually ready for production?

**Continue to:** [Experiment 5: Validation & Quality Gates](experiment-5.md)

Time to use: **/speckit.analyze + checklist** for systematic quality validation.



