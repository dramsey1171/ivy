# STON + BVS: Complete Implementation Plan

**Author**: Dana Ramsey
**Date**: 2025-10-17
**Status**: Ready for Implementation
**Timeline**: 10 weeks (2.5 months)

---

## Executive Summary

This plan integrates two complementary systems:

- **Issue #6: Geometric Semantic Architecture** - The mathematical foundation for meaning representation using fullerene vocabulary, Platonic solids, and φ17 rhythm
- **Issue #8: Biomimetic Vocabulary System (BVS)** - The lifecycle management system that grows, maintains, and evolves the vocabulary that populates the geometry

**Key Architectural Relationship**:

```
┌─────────────────────────────────────────────────────────┐
│  Issue #6: SEMANTIC GEOMETRY (ston_chatbot)             │
│                                                          │
│  • Fullerene Structure: GP(17,0) = 5,780 vertices       │
│  • Platonic Solids: Variable-arity semantic frames      │
│  • SDFI: Soft coherence scoring                         │
│  • DFGC: Role extraction parser                         │
│  • QTT: Integer-only learning                           │
└─────────────────┬───────────────────────────────────────┘
                  │
                  ↕  Layer 10: Fullerene Mapper + Governor
                  │
┌─────────────────┴───────────────────────────────────────┐
│  Issue #8: VOCABULARY LIFECYCLE (BVS)                   │
│                                                          │
│  • 9 Layers: Config → Growth → Decay → Compost          │
│  • Entropy Management: Controlled collapse & renewal    │
│  • Domain Bridging: Cross-topic vocabulary sharing      │
│  • Integer-only: Embedded deployment ready              │
│  • Auditability: Deterministic replay                   │
└─────────────────────────────────────────────────────────┘
```

**What Layer 10 Does**:
- **Mapper**: Assigns mature vocabulary entries from BVS to vertices on the fullerene surface
- **Governor**: Enforces geometric constraints and provides feedback to BVS for better growth

**Core Insight**: Issue #6 defines *what meaning is* (geometry). Issue #8 defines *how vocabulary evolves* to fill that geometry. Layer 10 is the bridge between them.

---

## Phase-by-Phase Roadmap

### Phase 1: Foundation (Weeks 1-2)

**Goal**: Establish type system, basic data structures, and minimal working infrastructure

**BVS Components**:
- Types & Constants (`types.hpp`, `constants.hpp`)
- Layer 0: Config & Primes
- Layer 1: Form Generator (minimal version)

**STON Components**:
- Fullerene structure types
- φ17 phase tables
- Basic SemanticFrame and DFU types

**Deliverable**:
- Core types compile
- Can generate basic word forms
- Can represent vertices in fullerene
- Unit tests for prime generation

**Acceptance Criteria**:
```cpp
// Can create a fullerene vertex
FullereneVertex v = {.id = 0, .phi = 5, .role_affinity = AGENT};

// Can generate word forms
FormGenerator gen(config);
vector<Form> forms = gen.generate_batch(100);
assert(forms.size() == 100);
```

---

### Phase 2: Core Loop (Weeks 3-4)

**Goal**: Get the BVS lifecycle running end-to-end (without geometric feedback yet)

**BVS Components**:
- Layer 2: Role Mapper (simplified - random assignment)
- Layer 3: Growth Engine (bloom, competition, selection)
- Layer 4: Decay Manager (age tracking, half-life)
- Layer 5: Fragment Store (composting)
- Minimal Orchestrator (tick loop)

**STON Components**:
- DFGC: Basic role extraction (agent, action, patient only)
- SemanticFrame storage

**Deliverable**:
- Words are born, bloom, compete, decay, compost
- Can track vocabulary size over time
- Can parse simple SVO sentences

**Acceptance Criteria**:
```cpp
// Run 1000 cycles
for (int i = 0; i < 1000; i++) {
    orchestrator.tick();
}

// Vocabulary should have stabilized
auto stats = orchestrator.get_stats();
assert(stats.mature_count > 50 && stats.mature_count < 200);
assert(stats.fragment_count > 0);  // Some decay happened
```

---

### Phase 3: Entropy & Control (Weeks 5-6)

**Goal**: Add homeostatic regulation to prevent runaway growth or collapse

**BVS Components**:
- Layer 8: Entropy Controller
- Collapse Protocol (when entropy exceeds threshold)
- Renewal Protocol (reseed from fragments)

**STON Components**:
- SDFI: Basic coherence scoring (without full geometry yet)
- φ17 phase assignment

**Deliverable**:
- System maintains stable vocabulary size
- Can survive and recover from collapse
- Entropy tracking and logging

**Acceptance Criteria**:
```cpp
// Run for 10,000 cycles
for (int i = 0; i < 10000; i++) {
    orchestrator.tick();

    // Should never explode or die completely
    auto stats = orchestrator.get_stats();
    assert(stats.total_count < 10000);  // No explosion
    assert(stats.mature_count > 10);    // Didn't collapse to zero
}

// Should have experienced at least one collapse
assert(orchestrator.collapse_count() >= 1);
```

---

### Phase 4a: Fullerene Interface (Week 7) ⭐ CRITICAL INTEGRATION POINT

**Goal**: Connect BVS to geometric constraints via Layer 10

**Components**:
- **Layer 10a: Fullerene Mapper**
  - Greedy assignment (V1)
  - Annealed assignment (V2)
  - Cost function: role_fit + domain_spread + degree_balance

- **Layer 10b: Fullerene Governor**
  - Constraint checker (3-neighbor rule, role distribution)
  - Nudge generator (boost words that fit geometry better)
  - Enforcement (reject assignments that violate hard constraints)

**STON Components**:
- Complete fullerene graph (5,780 vertices)
- Edge definitions (3 per vertex)
- Role affinity per vertex

**Data Contracts**:

```cpp
// BVS → Mapper: Here are mature words ready for placement
struct MappingRequest {
    vector<LexEntry> mature_entries;  // From Layer 3
    SystemStats current_stats;
};

// Geometry → Mapper: Here are placement constraints
struct GeometricConstraints {
    FullereneGraph graph;              // From Issue #6
    map<uint16, RoleAffinity> vertex_roles;  // φ17 assignments
    vector<EdgeConstraint> forbidden_pairs;
};

// Mapper → BVS: Here's the assignment + feedback
struct MappingResponse {
    vector<VertexAssignment> assignments;
    vector<Nudge> growth_feedback;  // Boost these forms, suppress those
    vector<Violation> warnings;     // Violations detected
};

// Governor → BVS: Adjust growth to improve fit
struct GovernanceFeedback {
    vector<RoleBoost> boost_roles;     // Need more instruments
    vector<DomainBoost> boost_domains;  // Need more food vocabulary
    vector<FormSuppression> suppress;   // Too many similar forms
};
```

**Deliverable**:
- Can assign 500+ words to fullerene vertices
- Assignment respects geometric constraints
- Governor provides actionable feedback to BVS

**Acceptance Criteria**:
```cpp
// Generate vocabulary
for (int i = 0; i < 5000; i++) orchestrator.tick();

// Map to fullerene
auto mapping_req = orchestrator.get_mature_entries();
auto assignments = mapper.assign_annealed(mapping_req, seed);

// Check constraints
auto violations = governor.check_constraints(assignments);
assert(violations.size() < assignments.size() * 0.05);  // <5% violations

// Each vertex should have exactly 3 neighbors (eventually)
auto graph = governor.get_fullerene_graph();
for (auto& vertex : graph.vertices) {
    assert(vertex.degree == 3);
}
```

---

### Phase 4b: Innovation (Week 8)

**Goal**: Add mutation and domain bridging for vocabulary evolution

**BVS Components**:
- Layer 6: Mutation Kernel (point mutation, crossover, inversion)
- Layer 7: Domain Bridge (cross-pollination between topics)

**STON Components**:
- Domain tracking in SemanticFrames
- Cross-domain coherence scoring

**Deliverable**:
- New forms emerge through mutation
- Vocabulary can adapt to new domains
- Cross-domain metaphors work

**Acceptance Criteria**:
```cpp
// Seed with food vocabulary
orchestrator.seed_domain(Domain::FOOD);
for (int i = 0; i < 2000; i++) orchestrator.tick();

// Introduce medical domain
orchestrator.introduce_domain(Domain::MEDICAL);
for (int i = 0; i < 2000; i++) orchestrator.tick();

// Should see cross-domain transfers
auto bridges = orchestrator.get_domain_bridges();
assert(bridges.size() > 0);  // Some cross-pollination happened

// Should see novel mutations
auto mutants = orchestrator.get_mutant_forms();
assert(mutants.size() > 10);
```

---

### Phase 5: Audit & Polish (Weeks 9-10)

**Goal**: Production-ready system with full observability and real linguistic features

**BVS Components**:
- Layer 9: Audit Interface (deterministic replay, event log)
- Real morphology in Form Generator
- Real role mapping in Layer 2 (φ17 alignment)

**STON Components**:
- Full DFGC parser (all roles: manner, time, purpose, etc.)
- Full SDFI scoring (all geometric terms)
- FUME filter integration
- Platonic solid assignment

**Deliverable**:
- Complete system working end-to-end
- Can reproduce any run from seed
- Linguistic quality high enough for real testing

**Acceptance Criteria**:
```cpp
// Run experiment
auto seed = orchestrator.get_seed();
orchestrator.run(10000);
auto results = orchestrator.get_final_state();

// Replay from same seed
orchestrator.reset(seed);
orchestrator.run(10000);
auto replay_results = orchestrator.get_final_state();

// Should be byte-for-byte identical
assert(results == replay_results);

// Parse complex sentence
auto frame = dfgc.parse("She carefully explained the theory with diagrams to students in the auditorium.");
assert(frame.roles.size() == 7);  // agent, manner, action, patient, instrument, recipient, location

// All words should map to fullerene
auto vocab = orchestrator.get_vocabulary();
for (auto& word : vocab) {
    assert(word.fullerene_vertex != UNASSIGNED);
}
```

---

## Directory Structure

```
ston_chatbot/
├── bvs/                          # Issue #8: Vocabulary Lifecycle
│   ├── include/bvs/
│   │   ├── types.hpp
│   │   ├── constants.hpp
│   │   ├── config_primes.hpp     # Layer 0
│   │   ├── form_generator.hpp    # Layer 1
│   │   ├── role_mapper.hpp       # Layer 2
│   │   ├── growth_engine.hpp     # Layer 3
│   │   ├── decay_manager.hpp     # Layer 4
│   │   ├── fragment_store.hpp    # Layer 5
│   │   ├── mutation_kernel.hpp   # Layer 6
│   │   ├── domain_bridge.hpp     # Layer 7
│   │   ├── entropy_controller.hpp # Layer 8
│   │   ├── audit_interface.hpp   # Layer 9
│   │   ├── fullerene_mapper.hpp  # Layer 10a ← BRIDGE
│   │   ├── fullerene_governor.hpp # Layer 10b ← BRIDGE
│   │   └── orchestrator.hpp
│   └── src/bvs/
│       └── (matching .cpp files)
│
├── core/                          # Issue #6: Semantic Geometry
│   ├── include/core/
│   │   ├── types.hpp
│   │   ├── constants.hpp
│   │   ├── fullerene_graph.hpp   # GP(17,0) structure
│   │   ├── platonic_solids.hpp   # Variable-arity frames
│   │   ├── phi17.hpp              # Prime lattice
│   │   ├── dfgc.hpp               # Role extraction parser
│   │   ├── sdfi.hpp               # Coherence scorer
│   │   ├── qtt.hpp                # Learning engine
│   │   └── fume.hpp               # Quality filter
│   └── src/core/
│       └── (matching .cpp files)
│
├── integration/                   # Layer 10 lives here
│   ├── include/integration/
│   │   ├── bvs_to_geometry.hpp
│   │   └── geometry_to_bvs.hpp
│   └── src/integration/
│       └── (matching .cpp files)
│
├── tests/
│   ├── bvs/
│   ├── core/
│   └── integration/
│
├── config/
│   ├── bvs_config.yml
│   └── geometry_config.yml
│
└── examples/
    ├── simple_lifecycle.cpp      # BVS only
    ├── simple_parsing.cpp         # STON only
    └── full_integration.cpp       # BVS + STON + Layer 10
```

---

## Testing Strategy

### Unit Tests (Each Layer)

```cpp
// Layer 3: Growth Engine
TEST(GrowthEngine, Bloom) {
    GrowthEngine engine(config);
    LexEntry entry = {.age = 0, .energy = 50};
    engine.bloom(entry);
    EXPECT_GT(entry.energy, 50);  // Energy increased
}

// Layer 10a: Fullerene Mapper
TEST(FullereneMapper, ThreeNeighborRule) {
    FullereneMapper mapper(graph);
    auto assignments = mapper.assign(mature_entries);

    for (auto& vertex : graph.vertices) {
        EXPECT_EQ(vertex.degree, 3);
    }
}
```

### Integration Tests (Cross-Layer)

```cpp
// BVS → Mapper → Governor → BVS feedback loop
TEST(Integration, FullCycle) {
    Orchestrator orch(config);

    // Grow vocabulary
    orch.run(5000);

    // Map to geometry
    auto mature = orch.get_mature_entries();
    auto assignments = mapper.assign(mature);

    // Get feedback
    auto nudges = governor.generate_nudges(assignments);

    // Apply feedback
    orch.apply_nudges(nudges);

    // Run more
    orch.run(5000);

    // Check improvement
    auto new_assignments = mapper.assign(orch.get_mature_entries());
    auto violations_before = governor.check_constraints(assignments);
    auto violations_after = governor.check_constraints(new_assignments);

    EXPECT_LT(violations_after.size(), violations_before.size());
}
```

### System Tests (End-to-End)

```cpp
// Parse → Map → Score → Learn → Repeat
TEST(System, RoundTrip) {
    // Parse sentence
    auto frame = dfgc.parse("She cut the kelp with scissors in the ocean.");

    // All roles should map to fullerene vertices
    for (auto& role : frame.roles) {
        auto vertex = mapper.get_vertex(role.word);
        EXPECT_NE(vertex, UNASSIGNED);
    }

    // Coherence should be high
    auto coherence = sdfi.score(frame);
    EXPECT_GT(coherence, 0.8);

    // Learn from this example
    qtt.update(frame, coherence);

    // Parse similar sentence
    auto frame2 = dfgc.parse("He sliced the seaweed with a knife in the water.");
    auto coherence2 = sdfi.score(frame2);

    // Should score even higher (learned from first example)
    EXPECT_GT(coherence2, coherence);
}
```

---

## Data Contracts (Issue #6 ↔ Issue #8)

### BVS → Geometry (Mature Vocabulary)

```cpp
struct LexEntry {
    string form;           // "cut", "scissors", "ocean"
    Role role;             // AGENT, INSTRUMENT, LOCATION
    uint8 phi;             // φ17 phase (0-16)
    Domain domain;         // FOOD, TOOLS, NATURE
    uint16 frequency;      // How often used
    uint8 age;             // Cycles alive
    uint8 energy;          // Vitality score
};
```

### Geometry → BVS (Constraints)

```cpp
struct GeometricConstraints {
    // Fullerene structure
    vector<FullereneVertex> vertices;  // 5,780 slots
    vector<FullereneEdge> edges;       // Each vertex has 3 edges

    // Role affinities (from φ17)
    map<uint16, Role> vertex_role_affinity;

    // Domain clustering
    map<Domain, vector<uint16>> domain_regions;

    // Forbidden pairings
    vector<pair<string, string>> edge_blacklist;
};
```

### Layer 10 → BVS (Feedback)

```cpp
struct Nudge {
    NudgeType type;        // BOOST, SUPPRESS, REDIRECT
    Role target_role;      // Which role to affect
    Domain target_domain;  // Which domain to affect
    float magnitude;       // How much to adjust (0-1)
    string reason;         // "Need more INSTRUMENT words"
};
```

### Layer 10 → Geometry (Assignments)

```cpp
struct VertexAssignment {
    uint16 vertex_id;      // Fullerene vertex (0-5779)
    string word;           // Assigned word
    uint8 phi;             // φ17 phase
    float fit_score;       // How well it fits (0-1)
};
```

---

## How Issues Reference Each Other

### In Issue #6 (STON Geometric Architecture)

**Section: Vocabulary Population**

The fullerene structure defines 5,780 vertex positions, but where do the words come from?

➡️ **See Issue #8 (BVS - Biomimetic Vocabulary System)** for the complete vocabulary lifecycle management system.

The BVS system:
- Generates candidate word forms through morphological rules
- Grows vocabulary through bloom/competition cycles
- Assigns roles using φ17 phase alignment (defined in this issue)
- Manages decay and composting to prevent stagnation
- Provides mature vocabulary entries for fullerene placement

**Integration Point**: Layer 10 (Fullerene Mapper + Governor) in `/integration` bridges BVS to geometry.

---

### In Issue #8 (BVS Vocabulary System)

**Section: Geometric Constraints**

BVS doesn't grow vocabulary in a vacuum. It must respect geometric constraints defined by the STON architecture.

➡️ **See Issue #6 (Full Geometric Architecture)** for the fullerene structure and φ17 rhythm that constrain vocabulary organization.

Geometric constraints enforced by Layer 10:
- Each word must map to a fullerene vertex
- Each vertex must have exactly 3 semantic neighbors
- Role assignments must align with φ17 phase affinities
- Domain clustering must respect graph distance
- Platonic solids define maximum semantic complexity per word

**Integration Point**: Layer 10b (Fullerene Governor) provides feedback to BVS growth engine based on geometric fit.

---

## Acceptance Criteria (Full System)

### Vocabulary Quality

- [ ] Mature vocabulary size stabilizes between 500-2,000 words
- [ ] All mature words successfully map to fullerene vertices
- [ ] <5% constraint violations after annealing
- [ ] Entropy stays within homeostatic range (40-60%)
- [ ] At least 3 domain regions successfully populated

### Geometric Quality

- [ ] All fullerene vertices have exactly 3 edges
- [ ] Role distribution matches φ17 predictions (within 10%)
- [ ] Domain clustering has >0.7 silhouette score
- [ ] SDFI coherence scores >0.8 for well-formed sentences
- [ ] Platonic solid assignment correct 95%+ of time

### Learning Quality

- [ ] QTT updates improve coherence over 1,000 iterations
- [ ] Round-trip translation maintains >95% fidelity
- [ ] Compositional tests (like Patrick's 500-word test) achieve >95%
- [ ] System recovers from vocabulary collapse within 500 cycles
- [ ] Mutation produces viable new forms (>10% survival rate)

### Engineering Quality

- [ ] All code is integer-only (no floating point)
- [ ] Deterministic replay works (same seed → same results)
- [ ] Full audit log available for all decisions
- [ ] Memory usage <500MB for full system
- [ ] Processing speed >1000 cycles/second

---

## Timeline Summary

| Phase | Weeks | Focus | Key Deliverable |
|-------|-------|-------|-----------------|
| 1 | 1-2 | Foundation | Types compile, basic structures work |
| 2 | 3-4 | Core Loop | Vocabulary lifecycle runs end-to-end |
| 3 | 5-6 | Entropy | Homeostasis and collapse/renewal |
| 4a | 7 | **Fullerene Interface** | **BVS ↔ Geometry bridge working** |
| 4b | 8 | Innovation | Mutation and domain bridging |
| 5 | 9-10 | Polish | Production-ready, auditable |

**Total**: 10 weeks (2.5 months)

---

## Risk Mitigation

### Risk 1: Geometric constraints too restrictive
**Symptom**: Mapper can't place >20% of mature words
**Mitigation**: Relax constraints in Governor, allow temporary violations with decay

### Risk 2: Vocabulary collapse
**Symptom**: Mature count drops to <10
**Mitigation**: Emergency reseed from fragments, lower decay rate temporarily

### Risk 3: Integer overflow
**Symptom**: Energy or frequency counters wrap around
**Mitigation**: Use uint64 for accumulators, periodic normalization

### Risk 4: Slow mapping
**Symptom**: Annealed assignment takes >10 seconds
**Mitigation**: Use greedy V1 for online updates, V2 for batch processing

---

## Success Metrics

### Day 1 (End of Phase 2)
- Vocabulary lifecycle runs for 10,000 cycles without crash
- Mature vocabulary size >50

### Day 30 (End of Phase 4a)
- 500+ words successfully mapped to fullerene
- Constraint violations <10%

### Day 70 (End of Phase 5)
- All acceptance criteria met
- Ready for Patrick's 500-word compositional test
- Round-trip translation >95% fidelity

---

## Next Steps for Patrick

1. **Week 1-2**: Start with types and Layer 0/1 (foundation)
2. **Week 3**: Get first vocabulary lifecycle working (Layers 2-5)
3. **Week 5**: Add entropy control (Layer 8)
4. **Week 7**: Implement Layer 10 (critical integration)
5. **Week 9**: Polish and test full system

**Key Decision Points**:
- After Phase 2: Does vocabulary lifecycle feel "alive"?
- After Phase 4a: Do geometric constraints improve vocabulary quality?
- After Phase 5: Does the system pass Patrick's compositional tests?

---

**Questions? See:**
- Issue #6 for geometric architecture details
- Issue #8 for BVS layer specifications
- This document for how they fit together
