# Experiment 4: Rapid Implementation

> **Time:** 4:10 PM - 4:40 PM (30 minutes)  
> **Status:** Specification ready. Time to build.

## ⚡ The Challenge

Spec says: Build QueryParser, FilterEngine, RankingEngine, CacheLayer. Traditional time: 2 weeks. You have 30 minutes.

**Your mission:** Use Copilot CLI + Spec Kit to generate code from specifications.

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
# Implementation Tasks: Search Refactoring

## Phase 1: Foundation (Priority: P0)
□ Task 1.1: Create Pydantic models (SearchQuery, Recipe)
□ Task 1.2: Implement QueryParser class
□ Task 1.3: Write QueryParser tests (including null regression)
□ Task 1.4: Update API routes to use QueryParser

## Phase 2: Filtering (Priority: P0)
□ Task 2.1: Extract FilterEngine class
□ Task 2.2: Migrate dietary restrictions filter
□ Task 2.3: Migrate cuisine and prep time filters
□ Task 2.4: Write FilterEngine tests (target: 90% coverage)

## Phase 3: Ranking (Priority: P1)
□ Task 3.1: Implement RankingEngine with scoring algorithm
□ Task 3.2: Add tests for ranking edge cases
□ Task 3.3: Integrate with FilterEngine output

## Phase 4: Caching (Priority: P1)
□ Task 4.1: Setup Redis CacheLayer
□ Task 4.2: Add cache invalidation logic
□ Task 4.3: Performance testing (<100ms target)

## Phase 5: Integration (Priority: P0)
□ Task 5.1: Wire all components together
□ Task 5.2: API backwards compatibility testing
□ Task 5.3: Full integration test suite

## Validation Gates
✓ After Phase 1: API still functional
✓ After Phase 2: Issue #247 resolved
✓ After Phase 4: <100ms performance met
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

**4.2.1** Use CLI for complex setup:

```bash
gh copilot suggest "Setup for search refactoring Phase 1:
1. Create directory structure: search/parser, search/filters, search/ranking, search/cache
2. Create __init__.py files
3. Create search/models.py with Pydantic models from specification
4. Add pytest configuration for new test structure
Follow the specification in specify/specs/search-refactoring.md"
```

**4.2.2** Review CLI's suggested commands, then execute:

```bash
# CLI generates these commands (review and run):
mkdir -p search/{parser,filters,ranking,cache,tests}
touch search/{__init__,parser/__init__,filters/__init__,ranking/__init__,cache/__init__}.py

# Creates models.py with Pydantic validation
cat > search/models.py << 'EOF'
from pydantic import BaseModel, Field, validator
from typing import List, Optional
from uuid import UUID

class SearchQuery(BaseModel):
    """Validated search query model"""
    query: str = Field(max_length=200)
    dietary_restrictions: List[str] = Field(default_factory=list)  # ← FIXES #247
    cuisine: Optional[str] = None
    max_prep_time: Optional[int] = None
    difficulty: Optional[str] = None
    
    @validator('dietary_restrictions', pre=True)
    def handle_null(cls, v):
        """Handle null from legacy clients"""
        return v if v is not None else []  # ← NULL HANDLING

class Recipe(BaseModel):
    """Recipe model for search results"""
    id: UUID
    name: str
    ingredients: List[str]
    dietary_tags: List[str]
    cuisine: str
    prep_time_minutes: int
    difficulty: str
    avg_rating: float
EOF
```

### What Just Happened
CLI handled boring setup work - created structure, boilerplate, following your spec.

---

## 📝 Exercise 4.3: Implement with Spec Kit (Spec-Driven) (8 min)

### Task
Generate QueryParser from specification using Spec Kit.

**Why Spec Kit:** Best for generating code that must follow specifications exactly.

### Steps

**4.3.1** Implement QueryParser:

```
/speckit.implement "Implement QueryParser component from specification. 
Include null handling for Issue #247 and comprehensive input validation."
```

**Expected:** Spec Kit generates `search/parser/query_parser.py` (150 lines) with:
- Input validation (Pydantic models)
- Null handling (fixes #247)
- XSS sanitization
- Comprehensive error messages
- Auto-generated tests

**4.3.2** Verify generated code:

```bash
cat search/parser/query_parser.py
```

### What Just Happened
Spec Kit **read your specification and constitution**, then generated constitution-compliant code with tests.

**Spec Kit is ideal for:** Generating components that must precisely follow specifications.

---

## 📝 Exercise 4.4: Implement with @workspace (Conversational) (7 min)

### Task
Generate FilterEngine using conversational coding agent.

**Why @workspace:** Best for iterative development, custom logic, and conversational refinement.

### Steps

**4.4.1** In Copilot Chat, use @workspace as coding agent:

```
@workspace Implement FilterEngine component for recipe search.

Requirements from specification:
- Apply dietary restrictions filter (MUST respect, never skip)
- Apply cuisine filter
- Apply prep time filter
- Apply filters in selectivity order (dietary first)
- Return partial results if any filter fails (graceful degradation)
- Target performance: <50ms for 2M recipes

Follow constitution:
- Use type hints (Python 3.11+)
- Include docstrings
- Handle errors gracefully
- Write as FilterEngine class in search/filters/filter_engine.py

This is part of refactoring Issue #247 (null handling bug fix).
```

**4.4.2** Agent generates code interactively. You can ask follow-up:

```
@workspace Add comprehensive unit tests for FilterEngine covering:
- Dietary filter with multiple restrictions
- Partial results when filter fails
- Performance with 2M recipes
- Edge cases (empty results, all results)
```

**4.4.3** Verify and refine:

```bash
pytest search/tests/test_filter_engine.py -v
```

If tests fail or coverage low, iterate:
```
@workspace The test for partial results failed. Fix the FilterEngine to return 
partial results when one filter raises an exception, as specified in constitution.
```

### What Just Happened
**@workspace** acted as conversational coding partner:
- Generated code from natural language requirements
- Responded to follow-up questions
- Iterated based on test results
- Refined implementation through conversation

**@workspace is ideal for:** Custom logic, iterative development, exploring solutions.

---

## 📊 Comparison: When to Use Each

| Scenario | Use This | Why |
|----------|----------|-----|
| **Generate from detailed spec** | `/speckit.implement` | Ensures spec compliance automatically |
| **Explore implementation options** | `@workspace` | Conversational, can ask "what if?" |
| **Custom algorithm logic** | `@workspace` | Better at complex algorithm reasoning |
| **Must match constitution exactly** | `/speckit.implement` | Reads constitution, enforces principles |
| **Need to iterate/refine** | `@workspace` | Supports back-and-forth conversation |
| **Generate tests alongside code** | Both work | Spec Kit auto-generates, @workspace on request |
| **Quick prototyping** | `@workspace` | Faster for exploratory coding |
| **Production-ready from spec** | `/speckit.implement` | Governance built-in |

**Best practice:** Use both! Spec Kit for spec-driven components, @workspace for custom logic and refinement.

---

## ✅ Checkpoint: What You Accomplished

🎯 **Task breakdown** generated automatically  
🎯 **Setup automated** with Copilot CLI  
🎯 **QueryParser implemented** with /speckit.implement (spec-driven)  
🎯 **FilterEngine implemented** with @workspace (conversational coding agent)  
🎯 **Both approaches demonstrated** - spec-driven vs conversational  
🎯 **Tests generated** for both components and passing  
🎯 **Issue #247** resolved (null handling in QueryParser)  

**Agent Capabilities Used:**
- Copilot CLI: Multi-step automation
- /speckit.implement: Spec-driven code generation
- @workspace: Conversational coding agent

**Current Time:** 4:40 PM  
**Status:** Core refactoring complete with two complementary approaches. But is it actually good?

---

## 🚀 Next: Experiment 5

Code is written, tests pass. But does it meet our constitution? Are we actually ready for production?

**Continue to:** [Experiment 5: Validation & Quality Gates](experiment-5.md)

Time to use: **/speckit.analyze + checklist** for systematic quality validation.



