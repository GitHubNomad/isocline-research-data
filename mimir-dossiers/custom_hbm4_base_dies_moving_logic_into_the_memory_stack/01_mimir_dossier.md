All 24 citation tags are balanced, with 21 source attributions. The dossier is complete and well-formed. Here is the final deliverable:

---

# Technical Dossier: Custom HBM4 Base Dies — Moving Logic Into the Memory Stack

**Project Isocline | Prepared by Mimir, Lead Researcher**

---

## Overview

High Bandwidth Memory has always had one non-memory layer: the base die, a logic component sitting beneath the stack of DRAM core dies that routes signals and power up through the through-silicon-via (TSV) bus to the host processor. For three generations — HBM1 through HBM3E — this layer was fabricated on the same mature, cell-density-optimized DRAM process as the memory dies stacked on top of it. That changed with HBM4.

The core architectural break is a *process split*: HBM4 is the first generation in which the most obvious architectural change from HBM3E to HBM4 is the process technology split between the core DRAM dies and the base die. HBM4 uses the DRAM process node only for the core die while the base die is manufactured with an advanced logic node unlike previous generations of HBM that used the same process for both. Through the prior generation, through HBM3E the base die had a simple job: signal routing, test, power distribution. A mature logic node (28–14nm class) was plenty — SK hynix handled it in its own fab, Samsung in its own foundry.

The immediate driver was the JEDEC-mandated I/O expansion. HBM4's JEDEC standard (JESD270-4, officially released April 2025) doubles the interface width to 2,048 bits, and through HBM3E, the base die was simple I/O circuitry — a 1x-class DRAM node was enough. From HBM4, routing 2,048 I/Os inside the base die no longer works in a DRAM process on area or power. That forced a fab migration: so the base die splits off from the DRAM fab to a logic foundry (TSMC N12FFC+ or N5).
[SOURCE: HBM4 compared to HBM4E | https://www.ersaelectronics.com/blog/hbm4-hbm4e | 2026]

Once the base die lives on a logic node rather than a DRAM node, it stops being a passive interposer-adjacent routing layer and starts being an SoC. This single process change is what the industry now calls "custom HBM" (cHBM) or, under JEDEC's own emerging nomenclature, C-HBM4E — and it is the subject of this dossier: what logic vendors are actually putting into that die, who manufactures it, what patents already cover the architecture, and what it implies for the next several device generations.

---

## Key Research Findings

### 1. The process-node migration is now vendor-confirmed and heterogeneous

All three HBM manufacturers have disclosed different base-die fabrication strategies, which is itself notable — the DRAM dies above the base die remain interoperable across a shared JEDEC envelope, but the logic layer underneath is now a competitive differentiator:

- **Samsung**: Samsung demonstrated a 36 GB, 12-high HBM4 stack featuring 2048 IO pins and 3.3 TB/s of bandwidth, built using 6th-generation 10nm-class (1c) DRAM core dies paired with an SF4 logic base die. Samsung fabricates this in-house: by moving the base die to the SF4 logic process, Samsung enables higher operating speeds and lower power consumption. The operating voltage (VDDQ) fell 32%, from 1.1V in HBM3E to 0.75V in HBM4.
[SOURCE: NVIDIA & Broadcom CPO, HBM4 & LPDDR6, TSMC Active LSI, Logic-Based SRAM, UCIe-S and More (ISSCC 2026 coverage) | https://newsletter.semianalysis.com/p/isscc-2026-nvidia-and-broadcom-cpo | 2026]

- **SK hynix**: outsources the base die to TSMC rather than fabricating in-house. SK hynix builds its HBM4 base die on TSMC's 12-nanometre logic process and has signalled it may move to TSMC's 3-nanometre node for the follow-on HBM4E.
[SOURCE: HBM4 Explained: The AI Memory Bottleneck | https://techdirectory.sg/kb/hbm4-explained | 2026]

- **Micron**: taking a hybrid, generation-staggered approach. Micron disclosed an "in-house advanced CMOS base die" for HBM4 and a shift to TSMC manufacturing for HBM4E base logic die (standard and customized).
[SOURCE: HBM4 Logic Base Die Primer: Architecture, Vendor Strategies, and Value Chain Implications | https://x.com/TheValueist/status/2023497120393916754 | 2026]

This creates a genuine foundry dependency risk that several sources flag explicitly: while it is good to see that HBM4 will finally use base dies made on logic process technologies, base dies supplied by a certain vendor (e.g., TSMC) lock in both the memory maker and its customers to that foundry's process roadmap and capacity allocation.
[SOURCE: Beyond Bandwidth: The Industry is Striving for Custom Memory (Part 1) | https://www.eetimes.com/beyond-bandwidth-the-industry-is-striving-for-custom-memory-part-1/ | 2026]

### 2. Why a logic node matters architecturally, not just electrically

A DRAM process is optimized for cell density and refresh characteristics, not for logic transistor performance or metal-layer richness. A logic-based base die provides higher transistor density, smaller device dimensions, and better area efficiency due to smaller transistors and larger metal-layer stack availability as compared to a base die fabricated on a DRAM process.
[SOURCE: NVIDIA & Broadcom CPO, HBM4 & LPDDR6, TSMC Active LSI, Logic-Based SRAM, UCIe-S and More | https://newsletter.semianalysis.com/p/isscc-2026-nvidia-and-broadcom-cpo | 2026]

Samsung's Hot Chips 2026 disclosure draws an explicit distinction between standard and custom offerings at the architectural level: standard HBM, which Samsung calls sHBM, keeps the B-die boxed to basic data and test-path functions. Custom HBM, or cHBM, instead uses the advanced logic process to build a more SoC-like base die while still sharing standard C-die stacks. The underlying driver is a power-delivery problem that a logic node partially solves: between the base die and host is already the biggest consumer of base die area. Increasing data pin count makes the area problem worse, and is bad for power consumption.
[SOURCE: Hot Chips 2026: Samsung and HBM Base Die Opportunities | https://chipsandcheese.com/p/hot-chips-2026-samsung-and-hbm-base | 2026]
[SOURCE: Samsung Evolving HBM Base Die at Hot Chips 2026 | https://www.servethehome.com/samsung-evolving-hbm-base-die-at-hot-chips-2026/ | 2026]

### 3. What's actually moving onto the base die: three concrete functional blocks

**a) The memory controller itself.** A 2026 arXiv paper describing a heterogeneous HBM-PIM runtime documents this relocation directly: relocated from the GPU die to the base die, the MC sits between the HBM TSV PHY connecting the DRAM layers and the UCIe die-to-die link connecting the host GPU. It arbitrates GPU-originated and PIM-originated requests through separate queues, preventing either path from starving the other. The area reclaimed on the host die is non-trivial: moving the MC onto the base die frees the corresponding GPU-die silicon—the 2,048-bit HBM PHY and its controller logic occupy ≈15 mm² per stack—which the GPU can instead dedicate to compute logic such as CUDA cores, tensor cores, or on-chip SRAM.
[SOURCE: TokenStack: A Heterogeneous HBM-PIM Architecture and Runtime for Efficient LLM Inference | https://arxiv.org/pdf/2605.05639 | 2026]

**b) Die-to-die interconnect migration from parallel HBM PHY to serial UCIe.** The base die connects to the GPU through a UCIe die-to-die (D2D) link rather than the HBM PHY. UCIe D2D replaces the wide, low-speed HBM signal bus with a narrower, serialized point-to-point interface that runs at up to 32 GT/s per lane, delivering comparable aggregate bandwidth at higher bandwidth density per millimeter of package shoreline.
[SOURCE: TokenStack: A Heterogeneous HBM-PIM Architecture and Runtime for Efficient LLM Inference | https://arxiv.org/pdf/2605.05639 | 2026]

**c) Built-in self-test and inline data transformation.** Samsung's ISSCC 2026 disclosure describes a programmable test architecture enabled specifically by the move to a logic process: another key innovation enabled by the logic base die is Samsung's Programmable Memory Built-In Self-Test (PMBIST) architecture. Separately, the TokenStack architecture places an inline compression engine on the base-die control substrate: this unit applies inline K8V4 compression when demoting a KV block from a compute layer to a capacity layer, and reverses the process during promotion. This expands effective capacity-layer KV space without adding a separate compression kernel to the GPU timeline.
[SOURCE: NVIDIA & Broadcom CPO, HBM4 & LPDDR6, TSMC Active LSI, Logic-Based SRAM, UCIe-S and More | https://newsletter.semianalysis.com/p/isscc-2026-nvidia-and-broadcom-cpo | 2026]
[SOURCE: TokenStack: A Heterogeneous HBM-PIM Architecture and Runtime for Efficient LLM Inference | https://arxiv.org/pdf/2605.05639 | 2026]

### 4. PIM vs. PNM: why the base die, not the DRAM die, is where complex compute lands

The AMMA paper lays out the trade-off clearly: Processing in memory (PIM) places arithmetic units directly on DRAM dies. By co-locating compute with individual banks or bank groups, PIM bypasses the TSV bus bottleneck and exposes full DRAM array bandwidth, an order-of-magnitude increase over the external HBM interface. However, compute units consume cell area, directly reducing storage capacity. Moreover, DRAM dies are manufactured on mature nodes optimized for cell density... rather than logic performance, and the tight area and power envelope restricts in-DRAM compute to simple GEMV operations. Commercial precedent for this constrained approach exists already: commercial products such as Samsung HBM2-PIM and SK Hynix AIM adopt this approach, but their limited compute capability and reduced capacity make them insufficient for modern attention workloads.

The base-die alternative avoids the capacity penalty entirely: processing near memory (PNM) integrates compute units into the logic die, incurring no capacity penalty. Because the logic-die process node is independent of the DRAM array, PNM permits far more complex logic than PIM, especially from HBM4 onward where logic dies move to ≤5nm, providing sufficient transistor density for sophisticated logic. The paper notes this isn't a free lunch for bandwidth scaling: PNM alone does not increase per-cube bandwidth, but replacing GPU compute dies with additional PNM-equipped cubes roughly doubl[es] the aggregate package bandwidth. Area and power, however, remain binding constraints: the logic die already hosts the memory controller and PHY, and sits beneath DRAM arrays that dissipate substantial heat.
[SOURCE: AMMA: A Multi-Chiplet Memory-Centric Architecture for Low-Latency 1M Context Attention Serving | https://arxiv.org/pdf/2604.26103 | 2026]

This PIM/PNM framing matters for editorial continuity: HBM2-PIM/AIM represents an early, capacity-constrained DRAM-die approach, while HBM4's logic-node base die represents the generation where PNM becomes genuinely viable at scale.

### 5. C-HBM4E: customization as a priced, JEDEC-compatible option

EE Times' industry sourcing describes the model directly: "the concept behind C-HBM4E is to allow companies to leverage the standard HBM4E interface while adding custom logic to the base die," Kamath said. Crucially, this customization is bounded by the standard rather than breaking it: the added logic must operate within the constraints of the JEDEC specification to ensure interoperability and predictable behavior.
[SOURCE: Beyond Bandwidth: The Industry is Striving for Custom Memory (Part 1) | https://www.eetimes.com/beyond-bandwidth-the-industry-is-striving-for-custom-memory-part-1/ | 2026]

SemiEngineering's reporting frames this as a targeted hyperscaler offering: HBM4 allows a customized base die, a capability that primarily targets hyperscalers. It also surfaces an unresolved supply-chain question: for a custom version, the memory maker still builds the memory dies for the stack, but it no longer owns the base-die design. Who integrates and tests the final known-good-stacked-die (KGSD) assembly remains vendor- and deal-specific.
[SOURCE: How Will The Custom HBM Business Work? | https://semiengineering.com/how-will-the-custom-hbm-business-work/ | 2026]

### 6. PHY design as the near-term bottleneck for custom base dies

Even Korean memory makers with internal advanced-logic access have not fully exploited it: unlike their HBM competitors, they do have internal access to technologies that are true logic nodes. The likely reason they continued to use memory-centric nodes in the base die is because the performance was sufficient to meet JEDEC speed standards for HBM PHY. Going to a true logic node is expensive and is best avoided if unnecessary for performance. This suggests the move to logic-node base dies is driven primarily by the 2,048-bit I/O area/power constraint and the customization business case, not PHY speed requirements alone.
[SOURCE: Why Faster PHY Design is the Low-Hanging Fruit in Custom HBM4 Base Dies | https://www.viksnewsletter.com/p/faster-phy-design-in-custom-hbm | 2026]

---

## Patent Landscape

**US Patent 10,817,422 — "Data processing system with decoupled data operations."** Describes a base-die architecture with an on-die processor-in-memory block co-located with a scratchpad, predating commercial HBM4 disclosures but articulating the same locality argument vendors now cite: bulk memory dies 320 include HBM DRAM dies physically overlying base die 310 and connected to it over a memory channel 330 using die-to-die bonding technology... base die 310 includes an external interface 312 with a set of microbumps, a physical interface circuit (PHY) 314, a memory module scratchpad (MMS) 316, and a processor-in-memory (PIM) 318. The claims state the rationale explicitly: PIM 318 is able to access data in MMS 316 faster than it can access data in bulk memory dies 320 for its own computations because it is co-located with MMS 316 on the same base die 310 and signal routing paths are shorter... using PIM 318 to process data stored in MMS 316 avoids the overhead of data movement through the internal busses and cache hierarchy of host integrated circuit 230.
[SOURCE: US Patent 10,817,422 — Data Processing System with Decoupled Data Operations | USPTO Patent Full-Text and Image Database (ppubs.uspto.gov) | Patent document retrieved 2026]

**US Patent 12,733,264 — "Planar T-coil and integrated circuit including the same."** A more recent grant describing ESD-protection T-coil structures shared between HBM memory dies and the base die: the first to fourth memory dies MD1 to MD4 may be collectively referred to as an HBM. The base die BD may be arranged on the interposer 93... the first memory die MD1, the second memory die MD2, the third memory die MD3, the fourth memory die MD4 and/or the base die BD may include an ESD element, and may include the T-coil for protecting high-speed TSV-connected I/O now carrying 2,048-bit-class traffic.
[SOURCE: US Patent 12,733,264 — Planar T-coil and Integrated Circuit Including the Same | USPTO Patent Full-Text and Image Database (ppubs.uspto.gov) | Patent document retrieved 2026]

**Standards note:** JEDEC JESD270-4 was officially released (April 2025), establishing the 2,048-bit interface that all vendor-specific base-die implementations must satisfy. HBM4E, by contrast, does not have a unified JEDEC standard. Each manufacturer has developed its own version with differentiated specifications in pin speed, process node, and packaging technology.
[SOURCE: HBM4 compared to HBM4E | https://www.ersaelectronics.com/blog/hbm4-hbm4e | 2026]

[UNVERIFIED: Vendor-specific patent filings (Samsung, SK hynix, Micron) explicitly claiming customer-programmable logic blocks on an HBM4/HBM4E base die could not be located within this session's search budget. The two patents above establish strong general prior art but a dedicated Google Patents/IEEE Xplore pass targeting vendor-specific HBM4 filings is recommended before publication.]

---

## Future Implications

**1. The base die as a memory-side network endpoint.** With the memory controller and UCIe D2D link now resident on the base die, it becomes a standardized point for memory disaggregation at the package level — a plausible architectural precursor to CXL-style pooling implemented below the rack layer. [SOURCE: TokenStack: A Heterogeneous HBM-PIM Architecture and Runtime for Efficient LLM Inference | https://arxiv.org/pdf/2605.05639 | 2026]

**2. Foundry concentration risk.** SK hynix's TSMC dependency versus Samsung's in-house SF4 strategy bifurcates the industry's reliance on a single advanced-logic foundry for the base-die layer, even as DRAM core dies remain vendor-internal — a variable worth tracking as SK hynix evaluates TSMC's 3nm node for HBM4E.

**3. Base die as pre-processor.** PMBIST and inline K8V4 compression establish that data transformation — not just routing and test — can live on the base die without breaking JEDEC interoperability. [UNVERIFIED: No committed vendor roadmap was found describing KV-cache eviction or gather/scatter logic specifically for HBM5-class base dies; this is forward-looking synthesis, not a documented claim.]

**4. Floorplanning impact.** A single-stack ≈15 mm² host-die reclaim, multiplied across a multi-stack accelerator package, could free substantial reticle-limited area for compute logic. [UNVERIFIED: Aggregate reclaim figure is an extrapolation, not confirmed against an actual shipped accelerator floorplan.]

---

## Continuity Hooks

- **Prior-article linkage:** HBM2-PIM and SK Hynix AIM are explicitly cited in current literature as the capacity-constrained predecessor to HBM4's base-die PNM approach — a natural link to any earlier PIM coverage. [SOURCE: AMMA: A Multi-Chiplet Memory-Centric Architecture for Low-Latency 1M Context Attention Serving | https://arxiv.org/pdf/2604.26103 | 2026]
- **Forward hook:** UCIe adoption as the base die's host-facing interface sets up a dedicated future piece on UCIe 2.0/3.0 across the broader chiplet ecosystem.
- **Forward hook:** TSMC concentration across SK hynix and Micron's base-die roadmaps sets up a foundry-geopolitics follow-on article.
- **Forward hook:** TokenStack and AMMA both treat the base die as a KV-cache management substrate — material for a dedicated LLM-serving/memory architecture deep-dive once more vendor silicon ships.

---

## Unverified Claims

1. [UNVERIFIED: Vendor-specific HBM4/HBM4E base-die logic patents (Samsung, SK hynix, Micron) — general prior art found, vendor-specific filings not located this session.]
2. [UNVERIFIED: Yield/cost comparisons between logic-node and DRAM-process base dies — no quantified data found.]
3. [UNVERIFIED: Committed roadmap for KV-cache eviction or gather/scatter logic on HBM5+ base dies — speculative synthesis only.]
4. [UNVERIFIED: ~100+ mm² aggregate host-die area reclaim estimate — extrapolated, not confirmed against a real floorplan.]
5. [UNVERIFIED: IP ownership/liability allocation for custom C-HBM4E designs — reported as an open, deal-specific question, not settled practice.]

*Note: Research for this dossier was constrained by a web-search tool-call budget that was exhausted mid-session; the five findings above are flagged for priority follow-up (particularly targeted Google Patents and IEEE Xplore passes) rather than treated as settled gaps.*