# KitchenSim Ontology - Recovery Action Plan
**Created**: 2026-03-15 22:05 GMT+8  
**Status**: Recovery from 5h 38m stall (last output 16:25)  
**Tech Lead**: Ross  

---

## 🎯 Target Module: **Core Kitchen Entities** (Module 1 of Month 1 Plan)

**Rationale**: This is the foundational module that establishes the primary domain concepts (Kitchen, Appliance, Ingredient, Recipe, Agent). It must be completed first to enable all subsequent modules.

---

## 📚 Concepts to Learn

### 1. Ontology Design Patterns
- **Basic Formal Ontology (BFO)** alignment
- **Domain-specific entity modeling** (separation of processes vs. objects)
- **Property hierarchies** (hasPart, locatedIn, capableOf)

### 2. Kitchen Domain Knowledge
- Kitchen layout standard (work triangle, zones)
- Common appliance categories (prep, cook, store, clean)
- Ingredient taxonomy (dairy, produce, protein, pantry)
- Recipe structure (ingredients, steps, equipment, time)

### 3. OWL 2 DL Constructs
- `owl:Class` with `rdfs:subClassOf`
- `owl:ObjectProperty` and `owl:DatatypeProperty`
- Domain and range constraints
- `owl:Restriction` patterns (some, only, min/max cardinality)

---

## 📋 Deliverables Specification

### File: `members/ross-techlead/ontology/core_kitchen_entities.ttl`
**Format**: Turtle (RDF/OWL)

**Required Classes**:
```turtle
:Kitchen a owl:Class .
:Appliance a owl:Class .
:Ingredient a owl:Class .
:Recipe a owl:Class .
:CookingAgent a owl:Class .  # human or robot chef
```

**Required Object Properties**:
- `:locatedIn` (domain: Appliance/Ingredient, range: Kitchen)
- `:hasPart` (domain: Kitchen/Appliance, range: Appliance)
- `:uses` (domain: Recipe, range: Appliance)
- `:requires` (domain: Recipe, range: Ingredient)
- `:preparedBy` (domain: Recipe, range: CookingAgent)

**Required Data Properties**:
- `:hasName` (literal string)
- `:hasPowerRating` (for appliances, in watts)
- `:hasShelfLife` (for ingredients, in days)

**Axioms**:
- `Appliance ⊑ hasPart only Appliance` (appliances only contain appliances)
- `Ingredient ⊑ locatedIn only (Kitchen or Appliance)` (ingredients are in kitchen or storage appliance)
- At least 3 disjoint subclass hierarchies (e.g., FoodAppliance, PrepAppliance, CleanAppliance)

---

## ⏱️ Time Estimate

| Phase | Duration | Checkpoint |
|-------|----------|------------|
| Study ontology patterns | 45 min | Understand BFO alignment |
| Learn kitchen domain | 30 min | Ingredient/appliance taxonomy |
| Draft class hierarchy | 30 min | 5 core classes defined |
| Implement OWL properties | 45 min | 4 object + 2 data properties |
| Add restrictions & axioms | 30 min | Cardinality constraints |
| Review & validate | 30 min | Syntax check in Protégé (optional) |
| **TOTAL** | **3.5 hours** | **Working ontology file** |

---

## ✅ Success Criteria

- [ ] `core_kitchen_entities.ttl` parses without RDF/XML errors
- [ ] All 5 core classes present with proper labels
- [ ] Minimum 6 object properties + 2 data properties defined
- [ ] At least 3 subclass hierarchies with 2+ levels each
- [ ] 5+ logical axioms (cardinality, domain/range, disjoints)
- [ ] File committed to workspace with descriptive commit message

---

## 🚀 Next Steps (after completion)

1. Move to Module 2: **State & Process Ontology** (actions, states, transitions)
2. Integrate with SimpleABM demonstration framework
3. Start Month 1 milestone: "Basic static ontology validated"

---

**Action**: Begin learning phase immediately. Use web search for "OWL 2 DL ontology design patterns kitchen domain" and "BFO basic formal ontology tutorial".
