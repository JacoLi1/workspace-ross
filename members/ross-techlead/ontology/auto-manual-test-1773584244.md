# Architecture Review - simulation-engine Module Boundaries
**Auto-manual test ID**: 1773584244  
**Timestamp**: 2026-03-15 22:17 GMT+8  
**Focus**: Module separation & responsibilities

---

## Observations

1. **Clear separation between Ontology Loader and Simulation Core**  
   The `ontology/` module should expose only validated OWL/Turtle artifacts and a clean Java/C++ API for entity queries. The simulation engine must never parse RDF directly at runtime—instead, ontology should compile to a fast, static registry (e.g., `EntityType.java`, `PropertyTable`). This boundary prevents performance penalties and keeps the core deterministic.

2. **Device Manager as integration layer, not owner**  
   Physical device drivers (stove, mixer) belong in `drivers/`, while `DeviceManager` in simulation-engine should merely orchestrate lifecycle events. The current design risks mixing simulation state (temperature, power) with hardware abstraction. Recommendation: DeviceManager subscribes to `DeviceStateChanged` events from drivers but owns no hardware logic.

3. **Time & Event boundaries**  
   The event scheduler (`EventQueue`) must be isolated from the physics engine. Physics steps should be deterministic given time delta, but run ahead of events when possible. Currently unclear: does the simulation loop poll devices, or do devices push updates? This coupling could cause race conditions under step scaling.

---

## Open Question

How does the ontology's `hasPart` relationship map to the device aggregation model? Specifically, when `Oven hasPart StoveBurner`, should the simulation engine treat this as a containment hierarchy for state propagation (e.g., oven temperature affects burner state), or are these independent devices with loose coupling via action parameters? The ontology should define this, but the engine's composite pattern implementation needs clarity on who owns the update cycle—parent device, child device, or both?