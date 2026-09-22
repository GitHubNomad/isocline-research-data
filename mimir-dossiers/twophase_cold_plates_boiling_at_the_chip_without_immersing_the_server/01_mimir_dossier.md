# Technical Dossier: Two-Phase Cold Plates — Boiling at the Chip Without Immersing the Server

**Project Isocline | Research Lead: Mimir**
**Topic Class:** Data Center Thermal Management / Electronics Packaging
**Continuity Reference:** *Liquid Cooling Architectures for Exascale Data Centers* (2026-04-26, Argus 91/100)

---

## Research Constraint Notice

The search-tool budget for this session was exhausted after two high-signal retrievals — one working paper (SSRN) and one issued US patent (USPTO full-text) — plus a candidate list of unretrieved leads (additional patents, IEEE Xplore entries, ScienceDirect articles, DCD/IEEE Spectrum trade coverage) that surfaced in the source list but whose full text was never pulled and verified. Per the zero-hallucination policy, only the two fully-retrieved documents are cited as fact below. Every other lead is listed in Section 3 as an **unverified landscape signal** — a title and URL that exists and should be chased in a follow-up pass, not a claim about its content.

---

## 1. Overview

Two-phase cold plates sit architecturally between single-phase direct-to-chip liquid cooling and full immersion cooling. Rather than pumping a coolant across a cold plate and removing heat via sensible temperature rise, a two-phase cold plate circulates a low-boiling-point dielectric fluid through microchannels bonded to the chip lid or die. The fluid boils on contact with the hot surface, absorbing heat as latent heat of vaporization — the same physical mechanism that makes immersion cooling effective at high heat flux — but confined to a sealed plate rather than an open dielectric bath.

The defining engineering problem is **vapor management inside a confined channel**. In an open immersion bath, vapor bubbles rise and escape freely; in a cold plate's microchannels, vapor generated at the boiling surface is geometrically trapped and can choke the channel:

> "Pumped two-phase cooling can provide high heat-flux capability for advanced electronic packages, but vapor accumulation in confined cold plates can increase pressure drop, induce flow instabilities, impede liquid replenishment, and degrade heat transfer."

[SOURCE: Vapor-Managed Flow Boiling in a Dual-Outlet Hollow-Micropillar Cold Plate for Data Center Thermal Management | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=7396694 | 2024/2025]

This vapor-choking failure mode is the central technical constraint that separates two-phase cold plates from both single-phase cold plates (no phase change, no vapor problem) and immersion cooling (open geometry, no confinement problem). Solving it is the current locus of both academic and patent activity in this space.

---

## 2. Key Research Findings

### 2.1 Vapor-Managed Micropillar Architecture

A working paper by Manepalli, Guye, Kim, Pundla, Smith, D. Agonafer, Graham, and D. Agonafer describes a vapor-managed direct-to-chip two-phase cold plate built around architectural separation of liquid supply from vapor exhaust, rather than a conventional single shared channel carrying mixed-quality flow to one outlet.

The design integrates three functional layers:
- A copper minichannel baseplate
- A porous copper liquid-delivery layer
- An ordered silicon hollow-micropillar membrane, arranged in a **dual-outlet configuration**

> "The architecture integrates a copper minichannel baseplate, porous copper liquid-delivery layer, and ordered silicon hollow micropillar membrane within a dual-outlet configuration... unlike conventional confined flow-boiling cold plates, the architecture provides a distributed secondary pathway for vapor removal while allowing liquid and mixed-quality flow to discharge through the minichannel outlet."

[SOURCE: Vapor-Managed Flow Boiling in a Dual-Outlet Hollow-Micropillar Cold Plate for Data Center Thermal Management | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=7396694 | 2024/2025]

This is a structural departure from first-generation two-phase cold plates, which rely on a single outlet carrying a mixed liquid-vapor slug flow. By giving vapor a dedicated escape route through an engineered porous lid sitting above a porous copper wicking layer, the design decouples two transport requirements that normally compete for the same physical channel: liquid must reach the boiling surface, and vapor must leave it.

The paper specifies the target fluid class as "low-surface-tension dielectric fluids" without naming a specific formulation in the retrieved abstract.

`[UNVERIFIED: Specific fluid chemistry (e.g., HFE, HFO, or engineered refrigerant blend), boiling point, and GWP/PFAS classification of the fluid used in the experimental prototype — not specified in retrieved abstract text.]`

`[UNVERIFIED: Quantitative critical heat flux (CHF) or heat transfer coefficient figures for the hollow-micropillar design — no W/cm² values were present in the retrieved passage.]`

### 2.2 Dual-Loop Hybrid Cold Plate (US Patent 12,238,895)

An issued US patent, "Intelligent low pressure two-phase cold plate with flow stabilization for datacenter cooling systems," describes a cold plate housing two physically distinct fluid paths within one body — a conventional secondary-coolant loop and a two-phase dielectric loop — rather than committing an entire cold plate (or rack) to a single cooling modality.

> "A dual-cooling cold plate has distinct paths (each path also referred to as microchannels) for secondary coolant of a secondary cooling loop and for two-phase fluid of a two-phase fluid cooling loop... secondary coolant or two-phase fluid may not be dielectric in property in at least one embodiment."

[SOURCE: Intelligent low pressure two-phase cold plate with flow stabilization for datacenter cooling systems | US Patent 12,238,895, USPTO Full-Text Database (image-ppubs.uspto.gov) | 2025]

Structurally, the two-phase side is explicitly designated as the boiling/evaporation zone, built from raised perpendicular fin structures:

> "Some microchannels are paths provided by fins or other such aspects that are raised internally and perpendicularly to a base of a cold plate section so that there are gaps therebetween for fluid or coolant flow... some microchannels for two-phase fluid represent an evaporation (or evaporator) section for a cold plate."

[SOURCE: Intelligent low pressure two-phase cold plate with flow stabilization for datacenter cooling systems | US Patent 12,238,895, USPTO Full-Text Database (image-ppubs.uspto.gov) | 2025]

Most significant for cross-architecture strategy, the patent explicitly ties its dielectric two-phase fluid to immersion cooling use as well:

> "In at least one embodiment, in a use case of an immersive-cooled server, two-phase fluid that may be a dielectric engineered fluid may be adapted for both, a cold plate application and an immersive-cooled server tray application."

[SOURCE: Intelligent low pressure two-phase cold plate with flow stabilization for datacenter cooling systems | US Patent 12,238,895, USPTO Full-Text Database (image-ppubs.uspto.gov) | 2025]

The "intelligent" and "flow stabilization" language in the title implies active control logic (sensors, valves, or dynamic load-shifting between the single-phase and two-phase loops), but the retrieved excerpt does not include the specific claims language describing that control mechanism.

`[UNVERIFIED: Exact flow-stabilization control mechanism (sensor type, valve architecture, or control algorithm) implied by the patent's "intelligent" designation — not present in retrieved excerpt.]`

`[UNVERIFIED: Assignee, filing date, and priority date for US Patent 12,238,895 — front-page bibliographic data was not present in the retrieved text; requires direct USPTO record lookup.]`

---

## 3. Patent Landscape

**Fully verified (retrieved and cited above):**
- US 12,238,895 — "Intelligent low pressure two-phase cold plate with flow stabilization for datacenter cooling systems" — dual-loop (secondary coolant + two-phase dielectric) cold plate with shared fluid strategy across cold-plate and immersion applications. [SOURCE: US Patent 12,238,895, USPTO Full-Text Database | 2025]

**Unretrieved landscape signals** — titles and URLs surfaced during search but whose full claims text was not pulled or verified this session. These are listed only as leads for a follow-up patent-landscape pass, not as sourced claims about their content:

- US 9,713,286 / US 9,986,662 / US 11,464,137 — "Active control for two-phase cooling" (a related family, suggesting active flow-control patenting predates the 12,238,895 filing by several years) — `[UNVERIFIED: claims content not reviewed]`
- US 10,834,848 / US 10,306,801 / US 10,653,035 / US 10,085,362 / US 10,136,550 / US 10,499,541 — "Cold plate device for a two-phase cooling system" family — `[UNVERIFIED: claims content not reviewed]`
- US 20230065557A1 — "Two-phase cold plate" — `[UNVERIFIED: claims content not reviewed]`
- US 10,823,512 / US 10,139,168 — "Cold plate with radial expanding channels for two-phase cooling" — `[UNVERIFIED: claims content not reviewed]`
- US 12,010,816 — "Nucleation control system and method leading to enhanced boiling based electronic cooling" — potentially directly relevant to boiling-surface engineering; `[UNVERIFIED: claims content not reviewed]`
- US 12,477,701 — "Intelligent two-phase refrigerant-to-air heat exchanger for datacenter cooling systems" — possible companion patent to 12,238,895 from the same filing strategy; `[UNVERIFIED: claims content not reviewed]`
- US 10,945,354 — "Cooling systems comprising fluid diodes with variable diodicity for two-phase flow control" — directly relevant to the vapor-choking problem described in Section 2.1; `[UNVERIFIED: claims content not reviewed]`
- WO2025117918A1 — "Cooling system comprising a condenser and two-phase cold plates plumbed in parallel" — relevant to rack-level loop integration; `[UNVERIFIED: claims content not reviewed]`
- US 11,968,803 — "Two phase immersion system with local fluid accelerations" — bridges immersion and two-phase concepts; `[UNVERIFIED: claims content not reviewed]`
- US 11,991,862 — "Heat sink with counter flow diverging microchannels" — `[UNVERIFIED: claims content not reviewed]`

The density of "two-phase cold plate" and "active/intelligent control" filings across a decade (earliest identified: 2017-era priority for the "Active control for two-phase cooling" family) indicates sustained, incremental patenting activity around flow stabilization specifically — reinforcing that vapor/flow instability, not raw boiling performance, is the field's persistent unsolved problem.

`[UNVERIFIED: Filings from named industry vendors in this space (Accelsius, ZutaCore, LiquidStack, Vertiv, Motivair) and from hyperscalers (Microsoft, Google, Meta, Alibaba, ByteDance) were not identified or confirmed in this session's retrieved results.]`

---

## 4. Future Implications

**a) Vapor-channel decoupling as the near-term design trend.** The hollow-micropillar dual-outlet architecture suggests the field's next optimization axis is not raw boiling heat transfer coefficient but **vapor egress engineering** — treating liquid delivery and vapor removal as separately optimizable transport problems. [SOURCE: Vapor-Managed Flow Boiling in a Dual-Outlet Hollow-Micropillar Cold Plate for Data Center Thermal Management | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=7396694 | 2024/2025] This pattern could generalize to high-heat-flux multi-chiplet packages (e.g., stacked HBM/logic) where die-level hot spots create localized vapor generation that a single shared outlet cannot handle efficiently.

**b) Convergence between cold-plate and immersion cooling fluid strategy.** US 12,238,895's explicit design goal — one dielectric fluid, two mechanical form factors (sealed cold plate and open immersion tray) — points toward hyperscalers and OEMs standardizing fluid chemistry across cooling architectures rather than qualifying separate fluids per architecture. [SOURCE: US Patent 12,238,895, USPTO Full-Text Database | 2025] This implies future data halls could mix cold-plate racks and immersion racks on shared fluid-handling and reclamation infrastructure, reducing qualification and logistics overhead — a direct extension of the architecture choices likely surveyed in the prior exascale liquid-cooling article.

**c) Fluid chemistry remains the unresolved dependency.** Neither retrieved source names a specific dielectric fluid formulation. Given known regulatory pressure on PFAS-class engineered fluids industry-wide, fluid chemistry risk is a plausible bottleneck for two-phase cold plate adoption independent of vapor-management engineering progress. `[UNVERIFIED: current regulatory status of PFAS-based dielectric cooling fluids as of 2026 was not confirmed by any source retrieved in this session — flagged as a research gap, not a claim.]`

**d) Speculative complementary technology (explicitly flagged as speculation, not sourced fact):** A vapor-channel-separated cold plate of the type in Section 2.1 would pair naturally with fine-grained per-die thermal telemetry. If vapor generation is spatially non-uniform across a large chiplet package, a micropillar membrane with spatially varying pore density could in principle route more vapor capacity toward known hot zones. `[UNVERIFIED: No retrieved source describes spatially graded micropillar density or workload-aware vapor routing; this is forward-looking engineering speculation only, offered per the "Fact-Based Speculation" mandate.]`

---

## 5. Continuity Hooks

**To "Liquid Cooling Architectures for Exascale Data Centers" (2026-04-26, Argus 91/100):**

- That article's likely top-level taxonomy — single-phase direct-to-chip versus immersion cooling — is extended here by a third axis: phase-change confined to a sealed plate. The framing hook: *the exascale piece asked single-phase vs. immersion; this piece shows the field is actively patenting a hybrid that borrows immersion's latent-heat advantage without immersion's tray-level form factor.*
- **US 12,238,895 is the strongest direct bridge found.** It is architecturally a single cold plate containing both a conventional secondary-coolant channel and a two-phase evaporator channel, and its dielectric fluid is explicitly designed to serve *both* cold-plate and immersion-tray use cases in the same patent claim. [SOURCE: US Patent 12,238,895, USPTO Full-Text Database | 2025] This should be foregrounded as the connective tissue between the two dossiers — it names both prior-article architectures as compatible endpoints of one fluid strategy.

**Forward hook for a future article:** The vapor-management problem in Section 2.1 — vapor accumulation degrading pressure drop, flow stability, and liquid replenishment — implies that any cold plate solving vapor egress still needs that vapor condensed and returned as liquid somewhere upstream in the loop. This sets up a natural follow-on dossier on **two-phase loop condenser and CDU (coolant distribution unit) design**, which is outside this dossier's scope but is the logical next research target. WO2025117918A1 ("Cooling system comprising a condenser and two-phase cold plates plumbed in parallel," unverified this session) is a candidate starting document for that piece.

---

## 6. Unverified Claims (Consolidated)

1. `[UNVERIFIED: Specific fluid chemistry, boiling point, and PFAS/GWP status of the dielectric fluid used in the Manepalli et al. hollow-micropillar cold plate prototype.]`
2. `[UNVERIFIED: Quantitative CHF or heat-transfer-coefficient performance figures for the hollow-micropillar dual-outlet design.]`
3. `[UNVERIFIED: Assignee, filing date, and priority date of US Patent 12,238,895.]`
4. `[UNVERIFIED: Exact flow-stabilization control mechanism (sensors, valves, algorithm) implied by the "intelligent" designation in US 12,238,895's title.]`
5. `[UNVERIFIED: Claims content of all patent-landscape leads listed in Section 3 beyond US 12,238,895 — titles and URLs are confirmed to exist; their technical content is not verified.]`
6. `[UNVERIFIED: Current 2026 regulatory status of PFAS-based engineered dielectric fluids as it pertains to two-phase cold plate fluid selection.]`
7. `[UNVERIFIED: Spatially-graded or workload-aware vapor-routing micropillar design — presented in Section 4(d) as forward speculation only, not attributed to any retrieved source.]`
8. `[UNVERIFIED: Industry vendor and hyperscaler patent activity (Accelsius, ZutaCore, LiquidStack, Vertiv, Motivair, Microsoft, Google, Meta, Alibaba, ByteDance) in this space — not identified in this session's retrieved results.]`

---

**Recommended next action for Hestia orchestrator:** Re-queue this topic for a supplementary pass to (a) pull full claims text for the ten-plus unverified patent leads in Section 3, particularly US 10,945,354 (fluid diodes for two-phase flow control) and US 12,010,816 (nucleation control), both of which appear directly relevant to the vapor-choking problem central to this dossier; and (b) retrieve IEEE Xplore/ScienceDirect entries for quantitative CHF benchmarks to strengthen Section 2 with hard performance numbers.