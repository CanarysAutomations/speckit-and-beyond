# Experiment 3: Designing the Solution

> **Time:** 3:45 PM - 4:10 PM (25 minutes)  
> **Status:** Decision made to refactor. Need specification.

## 🎯 The Challenge

@search-architect gave us principles, but how do we turn *"refactor 847-line monolith into clean architecture"* into concrete tasks?

**Your mission:** Use Instruction Files + Spec Kit to create governance-driven specifications.

---

## 🎯 Learning Objectives

- ✅ Create instruction files that teach agents domain knowledge
- ✅ Use Spec Kit to establish project constitution
- ✅ Generate detailed specifications from high-level intent
- ✅ Break specifications into actionable implementation tasks

**Agent Capabilities:** Instruction Files (domain context) + Spec Kit (structured specs)

---

## 📝 Exercise 3.1: Create Search Domain Instructions (8 min)

### Task
Teach agents about FlavorHub's search domain.

### Steps

**3.1.1** Create `.github/instructions/search-domain.instructions.md`:

```markdown
# FlavorHub Search Domain Instructions

## Business Context
FlavorHub users search 2M+ recipes using multiple criteria:
- **Ingredients**: "chicken, garlic, lemon"
- **Dietary restrictions**: vegan, gluten-free, nut-free, dairy-free
- **Cuisine types**: Italian, Thai, Mexican, Indian, Japanese
- **Prep time**: <30min, 30-60min, >60min
- **Difficulty**: beginner, intermediate, advanced

## Critical Requirements
- **Performance SLA**: <100ms search latency (p95)
- **Accuracy**: Relevant results ranked first
- **Safety**: MUST respect dietary restrictions (legal/health liability)
- **Offline-capable**: PWA requirement for mobile users

## Data Model
```python
Recipe:
  id: UUID
  name: str
  ingredients: List[str]
  dietary_tags: List[str]  # ["vegan", "gluten-free"]
  cuisine: str
  prep_time_minutes: int
  difficulty: str
  avg_rating: float
```

## Edge Cases to Handle
- Null/empty search queries
- Users with no dietary preferences (current bug!)
- Ingredient substitutions (e.g., "milk" includes "almond milk")
- Regional naming (UK: "aubergine" = US: "eggplant")
- Partial ingredient matches
- Typos and fuzzy matching

## Technical Constraints  
- Database: PostgreSQL 14 with full-text search
- Cache: Redis 7 for hot queries
- API: REST, must maintain v2 backwards compatibility
- Response size: Max 50 results per page
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

Context: "Refactoring FlavorHub search system. Priorities from @search-architect:
1. Performance <100ms
2. Reliability (graceful degradation)
3. Clean architecture (separation of concerns)
4. Testability >85% coverage
5. Backward compatibility

Follow domain requirements from search-domain.instructions.md"
```

### Expected Output

Spec Kit creates `constitution.md`:

```markdown
# FlavorHub Search Refactoring Constitution

## Mission
Transform search from 847-line monolith to clean architecture supporting 
10M users with <100ms latency and zero-downtime evolution.

## Core Principles

### 1. Performance First
- All searches <100ms (p95)
- Redis caching for hot queries
- Database indexes on search fields
- Async operations where possible

### 2. Reliability & Safety
- Graceful degradation (partial results > errors)
- Input validation using Pydantic models
- Dietary restrictions are HARD filters (never skip)
- Comprehensive error handling

### 3. Clean Architecture
Components with single responsibilities:
- **QueryParser**: Input validation
- **FilterEngine**: Apply search criteria
- **RankingEngine**: Relevance scoring
- **CacheLayer**: Performance optimization

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
Create detailed spec for the refactoring.

### Steps

**3.3.1** Continue with Spec Kit:

```
/speckit.specify "Refactor search system following constitution. 
Break 847-line search.py into: QueryParser, FilterEngine, RankingEngine, CacheLayer.
Fix Issue #247 null handling as part of QueryParser validation."
```

### Expected Output

Spec Kit generates (reading constitution + instructions):

```markdown
# SPECIFICATION: Search System Refactoring

## Architecture

```
User Request
    ↓
QueryParser (validate, sanitize)
    ↓
FilterEngine (dietary, cuisine, time)
    ↓
RankingEngine (relevance scoring)
    ↓
CacheLayer (Redis)
    ↓
Response
```

## Component 1: QueryParser
**Purpose:** Validate and parse all search inputs  
**Input:** Raw API request (dict)  
**Output:** SearchQuery (Pydantic model)

**Behavior:**
- Validate query string (not null, max 200 chars)
- Parse dietary_restrictions (handle null → empty list) ← FIXES ISSUE #247
- Normalize ingredients (handle regional variants)
- Sanitize for XSS prevention

**Tests:** 15 test cases including null handling regression test

## Component 2: FilterEngine  
**Purpose:** Apply search criteria to recipe collection  
**Input:** SearchQuery + Recipe[]  
**Output:** Filtered Recipe[]

**Behavior:**
- Apply filters in selectivity order (dietary first)
- Use database indexes
- Return partial results if filter fails (reliability principle)

**Performance:** Must complete in <50ms for 2M recipes

## Component 3: RankingEngine
**Purpose:** Score and sort results by relevance  
**Input:** Filtered Recipe[]  
**Output:** Ranked Recipe[]

**Algorithm:**
- Ingredient match score (0-1)
- Rating weight (0.3)
- Popularity weight (0.2)
- Recency weight (0.1)

## Component 4: CacheLayer
**Purpose:** Reduce database load  
**Input:** SearchQuery  
**Output:** Cached results or cache miss

**Strategy:**
- Cache hot queries (>10 requests/hour)
- TTL: 5 minutes
- Invalidate on recipe updates

## Success Criteria (from constitution)
✓ Performance: <100ms (currently 280ms)
✓ Tests: >85% coverage
✓ Issue #247: Resolved via QueryParser
✓ API: v2 backwards compatible
```

**3.3.2** Review the specification. All implementation will follow this.

---

## ✅ Checkpoint: What You Accomplished

🎯 **Domain knowledge captured** in instruction file  
🎯 **Constitution established** as governance layer  
🎯 **Detailed specification generated** with component breakdown  
🎯 **Success criteria defined** - know when "done" means done  

**Current Time:** 4:10 PM  
**Status:** Blueprint complete. Ready to implement.

---

## 🚀 Next: Experiment 4

Specification is ready. Now let's build it.

**Continue to:** [Experiment 4: Rapid Implementation](experiment-4.md)

Time to use: **Copilot CLI + /speckit.implement** for fast, spec-driven code generation.




