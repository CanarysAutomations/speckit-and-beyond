# Experiment 3: Designing the Solution

> **Time:** 3:45 PM - 4:18 PM (33 minutes)  
> **Status:** Decision made to refactor. Need specification and technical plan.

## 🎯 The Challenge

@search-architect gave us principles and identified the problem: 1103-line monolith needs breaking into 4 modules.

But how do we turn *"refactor 1103-line God Object into clean architecture"* into concrete, trackable implementation tasks?

**Your mission:** Use Instruction Files + Spec Kit to create governance-driven specifications.

---

## 🎯 Learning Objectives

- ✅ Create instruction files that teach agents domain knowledge
- ✅ Use Spec Kit to establish project constitution
- ✅ Generate detailed specifications from high-level intent
- ✅ Create technical plans with architectural decisions
- ✅ Understand the complete Spec Kit workflow

**Agent Capabilities:** Instruction Files (domain context) + Spec Kit (constitution → specify → plan → tasks)

---

## 📝 Exercise 3.1: Create Search Domain Instructions (8 min)

### Task
Teach agents about FlavorHub's search domain.

### Steps

**3.1.1** Create `.github/instructions/search-domain.instructions.md`:

```markdown
---
description: FlavorHub recipe search domain knowledge - data models, business requirements, and technical constraints for search functionality
# applyTo: 'search\.py|models\.py'
---

# FlavorHub Search Domain Instructions

## Business Context
FlavorHub users search 2M+ recipes using multiple criteria:
- **Ingredients**: "chicken, garlic, lemon"
- **Dietary restrictions**: vegan, gluten-free, nut-free, dairy-free
- **Cuisine types**: Italian, Thai, Mexican, Indian, Japanese
- **Prep time**: <30min, 30-60min, >60min
- **Difficulty**: beginner, intermediate, advanced

## Critical Requirements
- **Safety**: MUST validate inputs (dietary restrictions are legal/health liability)
- **Reliability**: Graceful handling of null/missing data
- **Quality**: Test coverage for critical paths
- **Accuracy**: Relevant results ranked first

## Data Model (matches models.py)

    class Recipe:
        id: UUID
        name: str
        ingredients: List[str]
        dietary_tags: List[str]  # ["vegan", "gluten-free"]
        cuisine: str
        prep_time_minutes: int
        difficulty: str
        avg_rating: float
        created_at: datetime

    class User:
        id: UUID
        name: str
        email: str
        dietary_restrictions: Optional[List[str]]  # BUG: Can be None!
        created_at: datetime

    # Search Request Format (from search.py parse_search_request)
    SearchRequest:
        query: str
        dietary_restrictions: Optional[List[str]]  # Current bug - can be None
        cuisine: Optional[str]
        max_prep_time: Optional[int]
        difficulty: Optional[str]
        min_rating: float = 0.0

## Edge Cases to Handle

- **Null/empty search queries** (current issue!)
- **Users with no dietary preferences** (dietary_restrictions = None - NULL_DIETARY_BUG!)
- Ingredient substitutions (e.g., "milk" includes "almond milk")
- Regional naming (UK: "aubergine" = US: "eggplant")
- Partial ingredient matches
- Typos and fuzzy matching

## Technical Constraints

- Database: PostgreSQL 14 with full-text search
- Cache: Redis 7 for hot queries
- API: REST, must maintain v2 backwards compatibility
- Response size: Max 50 results per page
- **Current file: search.py (1103 lines) - God Object anti-pattern**
```

**3.1.2** Save the file.

### What You Created
Domain knowledge that **every agent** (Spec Kit, Copilot, custom agents) will now understand.

---

## 📝 Exercise 3.2: Establish Constitution (7 min)

### Task
Use Spec Kit to create governance from architect's principles.

### Steps

**3.2.1** In Copilot Chat:

```
/speckit.constitution

Context: "Refactoring FlavorHub search.py (1103 lines) into clean architecture. 
Based on @search-architect analysis:
1. Reliability (input validation + null handling)
2. Architecture (break into 4 modules: validation, filtering, aggregation, formatting)
3. Testability (>80% coverage, each module independently testable)
4. Performance (fix caching leak, optimize filter ordering)
5. Maintainability (remove 74 magic numbers, eliminate dead code)

Follow domain requirements from search-domain.instructions.md"
```

### Expected Output

Spec Kit creates `constitution.md`:

```markdown
# FlavorHub Search Refactoring Constitution

## Mission
Transform search.py from 1103-line monolith into clean 4-module architecture
supporting 10M users with <100ms latency and maintainable codebase.

## Core Principles

### 1. Modular Architecture
- 4 modules with single responsibilities:
  - validation_module.py (~200 lines): Input validation, Pydantic models
  - filtering_module.py (~300 lines): All filter logic, no ranking
  - aggregation_module.py (~250 lines): Ranking algorithms, caching
  - formatting_module.py (~150 lines): Response formatting, pagination
- Each module: <300 lines, independently testable
- No circular dependencies between modules
- Clear interfaces between components

### 2. Reliability & Safety
- Graceful degradation (partial results > errors)
- Input validation using Pydantic models
- Dietary restrictions are HARD filters (never skip)
- Comprehensive error handling

### 3. Clean Code Quality
- Remove all dead code (3 deprecated filter versions, 2 old ranking algorithms)
- Replace 74 magic numbers with named constants
- Remove 12 feature flags (keep only active production flags)
- Fix broken caching (CACHE_LEAK_BUG - memory leak)
- 100% type hints (Python 3.11+)

### 4. Quality Standards
- Test coverage: >85%
- Type hints: 100% (Python 3.11+)
- Documentation: All public interfaces
- Code review: Required for all changes

### 5. Deployment Safety
- API v2 backward compatible
- Feature flags for gradual rollout
- Rollback plan for every change
- Zero downtime deployments
```

### What Just Happened
Spec Kit turned architect's recommendations + domain knowledge into **governing document** for all implementation.

---

## 📝 Exercise 3.3: Generate Specification (10 min)

### Task
Create detailed spec for breaking monolith into 4 modules.

### Steps

**3.3.1** Continue with Spec Kit:

```
/speckit.specify "Break search.py (1103 lines) into 4 modules following constitution.
Fix NULL_DIETARY_BUG (null handling) via validation_module. 
Remove dead code. Fix caching leak (CACHE_LEAK_BUG). 
Each module independently testable with >80% coverage.
Maintain API backward compatibility."
```

### Expected Output

Spec Kit generates (reading constitution + instructions):

```markdown
# SPECIFICATION: Search Modularization Refactor

## Overview
Break search.py (1103 lines) into 4 focused modules with clean interfaces.
Fix systemic issues (null handling, caching leak, dead code) during refactor.

## Module 1: validation_module.py (~200 lines)
**Purpose:** Input validation and type safety  
**Exports:** SearchQuery (Pydantic model), validate_search_request()  
**Implementation:**

    from pydantic import BaseModel, Field, validator
    from typing import Optional, List
    
    class SearchQuery(BaseModel):
        """Validated search query model - FIXES NULL_DIETARY_BUG"""
        query: str = Field(..., min_length=1, max_length=200)
        dietary_restrictions: List[str] = Field(default_factory=list)
        cuisine: Optional[str] = None
        max_prep_time: Optional[int] = Field(None, ge=0, le=360)
        difficulty: Optional[str] = None
        min_rating: float = Field(default=0.0, ge=0.0, le=5.0)
        
        @validator('dietary_restrictions', pre=True)
        def handle_none(cls, v):
            """Convert None to empty list - FIXES NULL_DIETARY_BUG"""
            return v if v is not None else []
    
    def validate_search_request(request_data: dict) -> SearchQuery:
        """Entry point for validation"""
        try:
            return SearchQuery(**request_data)
        except ValidationError as e:
            logger.warning(f"Validation failed: {e}")
            # Return safe defaults instead of crashing
            return SearchQuery(query=request_data.get('query', ''))

**Tests:** 15 test cases (null inputs, type validation, edge cases)

## Module 2: filtering_module.py (~300 lines)
**Purpose:** Apply search filters to recipe list  
**Exports:** filter_recipes(), FilterCriteria  
**Key Changes:**
- Remove 3 deprecated filter versions (filter_by_dietary_v1, v2, v3)
- Keep only production versions
- Null-safe by design (validates inputs at entry)
- Optimized filter ordering (most selective first)

**Implementation:**

    def filter_recipes(recipes: List[Recipe], criteria: SearchQuery) -> List[Recipe]:
        """Apply all filters in optimized order"""
        results = recipes
        
        # Optimized order: most selective first
        if criteria.cuisine:
            results = _filter_by_cuisine(results, criteria.cuisine)
        
        if criteria.difficulty:
            results = _filter_by_difficulty(results, criteria.difficulty)
        
        if criteria.max_prep_time:
            results = _filter_by_prep_time(results, criteria.max_prep_time)
        
        if criteria.dietary_restrictions:
            results = _filter_by_dietary(results, criteria.dietary_restrictions)
        
        if criteria.query:
            results = _filter_by_query(results, criteria.query)
        
        results = _filter_by_rating(results, criteria.min_rating)
        
        return results
    
    def _filter_by_dietary(recipes: List[Recipe], restrictions: List[str]) -> List[Recipe]:
        """Null-safe dietary filter - NULL_DIETARY_BUG fixed"""
        if not restrictions:  # Empty list is valid
            return recipes
        return [r for r in recipes if all(d in r.dietary_tags for d in restrictions)]

**Tests:** 20 test cases (each filter function, edge cases, optimization)

## Module 3: aggregation_module.py (~250 lines)
**Purpose:** Ranking, scoring, and caching  
**Exports:** rank_recipes(), CacheManager  
**Key Changes:**
- Remove 2 old ranking algorithms (keep only hybrid_v3)
- Fix caching memory leak (CACHE_LEAK_BUG) - add LRU eviction
- Replace 12 magic weights with named constants
- Remove 8 unused feature flags

**Implementation:**

    # Named constants (replaces magic numbers)
    RELEVANCE_WEIGHT = 0.45
    RATING_WEIGHT = 0.30
    POPULARITY_WEIGHT = 0.25
    
    def rank_recipes(recipes: List[Recipe], query: str) -> List[Recipe]:
        """Rank recipes by relevance using hybrid_v3 algorithm"""
        if not query:
            return sorted(recipes, key=lambda r: r.avg_rating, reverse=True)
        
        scored = []
        for recipe in recipes:
            relevance = _calculate_relevance(recipe, query) * RELEVANCE_WEIGHT
            rating = recipe.avg_rating * RATING_WEIGHT
            popularity = _calculate_popularity(recipe) * POPULARITY_WEIGHT
            
            final_score = relevance + rating + popularity
            scored.append((final_score, recipe))
        
        scored.sort(key=lambda x: x[0], reverse=True)
        return [recipe for score, recipe in scored]
    
    # Fixed caching with LRU eviction (CACHE_LEAK_BUG)
    from functools import lru_cache
    
    @lru_cache(maxsize=1000)  # Fixes memory leak
    def get_cached_results(cache_key: str) -> Optional[List[Recipe]]:
        """LRU cache with automatic eviction"""
        # Implementation...

**Tests:** 18 test cases (ranking accuracy, cache behavior, eviction)

## Module 4: formatting_module.py (~150 lines)
**Purpose:** Response formatting and pagination  
**Exports:** format_search_response(), PaginationOptions  
**Key Changes:**
- Remove half-implemented XML support
- Clean JSON formatting only
- Proper pagination (not loading everything in memory)

**Implementation:**

    def format_search_response(
        recipes: List[Recipe],
        page: int = 1,
        page_size: int = 50
    ) -> dict:
        """Format search results with pagination"""
        start = (page - 1) * page_size
        end = start + page_size
        page_results = recipes[start:end]
        
        return {
            "results": [_format_recipe(r) for r in page_results],
            "page": page,
            "page_size": page_size,
            "total": len(recipes),
            "has_more": end < len(recipes)
        }
    
    def _format_recipe(recipe: Recipe) -> dict:
        """Convert Recipe to API response format"""
        return {
            "id": str(recipe.id),
            "name": recipe.name,
            "ingredients": recipe.ingredients,
            "dietary_tags": recipe.dietary_tags,
            "cuisine": recipe.cuisine,
            "prep_time_minutes": recipe.prep_time_minutes,
            "difficulty": recipe.difficulty,
            "rating": recipe.avg_rating
        }

**Tests:** 12 test cases (formatting, pagination, edge cases)

## Success Criteria (from constitution)
✔ Modularity: 4 modules, each <300 lines, single responsibility
✔ NULL_DIETARY_BUG: Fixed via validation_module
✔ CACHE_LEAK_BUG: Fixed via LRU caching in aggregation_module
✔ Dead code: Removed (3 old filters, 2 old algorithms, XML support)
✔ Magic numbers: Replaced with 8 named constants
✔ Tests: >80% coverage for each module independently
✔ API: Backward compatible (no breaking changes)
```

**3.3.2** Review the specification. This defines WHAT we're building.

---

## 📝 Exercise 3.4: Generate Technical Plan (8 min)

### Task
Create technical plan showing HOW to implement the specification.

### Why This Step Matters

Spec Kit separates concerns:
- `/speckit.specify` → **WHAT** to build (requirements, features, success criteria)
- `/speckit.plan` → **HOW** to build it (technical approach, architecture, design patterns)
- `/speckit.tasks` → **WHEN/WHO** (implementation breakdown, dependencies)

The plan bridges the gap between specification and implementation.

### Steps

**3.4.1** Continue with Spec Kit:

```
/speckit.plan "Create technical implementation plan for 4-module refactor.
Based on specification, detail the technical approach for:
- Module extraction strategy (which functions move where)
- Interface design between modules
- Migration sequence (minimize disruption)
- Testing strategy for each module
- Rollout approach (gradual vs big-bang)

Follow constitution and consider current 1103-line structure."
```

### Expected Output

Spec Kit generates `plan.md` with technical implementation details:

**Key sections include:**

**1. Architecture Decisions**
- Module boundaries showing pipeline: validation → filtering → aggregation → formatting
- Each module exports specific functions with <300 lines
- No circular dependencies

**2. 5-Phase Migration Strategy**
- **Phase 1:** Extract validation_module.py with Pydantic models (fixes NULL_DIETARY_BUG)
- **Phase 2:** Extract filtering_module.py, remove 3 deprecated versions
- **Phase 3:** Extract aggregation_module.py, fix caching (CACHE_LEAK_BUG), replace magic numbers
- **Phase 4:** Extract formatting_module.py, remove XML support
- **Phase 5:** Integration & cleanup, reduce search.py to ~50-line orchestrator

**3. Testing Strategy**
- 65 unit tests total (>80% coverage per module)
- Integration tests for backward compatibility
- Feature flags for rollback safety

**4. Rollout Approach**
- Gradual per-module rollout (10% → 100% over 5 weeks)
- Monitoring error rate and latency
- Zero breaking changes to API

**3.4.2** Review the plan. This defines HOW we'll implement the specification.

### What Just Happened

Spec Kit created **technical implementation plan**:
- **Module extraction strategy** - which functions move where
- **5-phase migration** - incremental, testable, rollback-safe
- **Interface design** - clean boundaries between modules  
- **Testing strategy** - 65 unit tests + integration tests
- **Rollout approach** - gradual with feature flags

This bridges the gap between "what to build" (spec) and "implementation tasks" (next step).

---

## ✅ Checkpoint: What You Accomplished

🎯 **Domain knowledge captured** in instruction file  
🎯 **Constitution established** as governance layer  
🎯 **Detailed specification generated** for 4-module refactor (WHAT to build)  
🎯 **Technical plan created** with migration strategy (HOW to build it)  
🎯 **Success criteria defined** - know when "done" means done  

**Key Insight:** Complete Spec Kit workflow (constitution → specify → plan) provides governance at every level. We know WHAT to build, HOW to build it, and WHY each decision was made.

**Current Time:** 4:18 PM  
**Status:** Complete blueprint ready. Next: break plan into implementation tasks.

---

## 🚀 Next: Experiment 4

Specification is ready. Now let's build the 4 modules.

**Continue to:** [Experiment 4: Rapid Implementation](experiment-4.md)

Time to use: **Copilot CLI + /speckit.implement** for multi-file, spec-driven code generation.




