# Technical Dossier: Panel-Level Fan-Out Packaging (FOPLP)
## Rectangular Substrates for Packages Larger Than the Reticle

---

## Overview

Panel-level fan-out packaging (FOPLP) is an advanced semiconductor packaging approach that replaces the traditional circular, 300mm wafer as the carrier substrate with large rectangular panels, allowing many more chips — or much larger reconstituted packages — to be processed in a single production unit. The core technique descends from fan-out wafer-level packaging (FOWLP), in which dies are singulated from the native wafer, re-embedded ("reconstituted") into a mold compound carrier, and built up with redistribution layers (RDL) that "fan out" I/O beyond the original die footprint.

[SOURCE: Fan-Out Wafer and Panel Level Packaging as Packaging Platform for Heterogeneous Integration | https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6562530/ | n.d.]

The panel-level variant swaps the round wafer carrier for rectangular panels commonly cited in the 515mm x 510mm range, a geometry that dramatically increases usable area versus a 300mm wafer and, critically, decouples maximum package size from the photolithographic reticle limit that constrains monolithic die and standard 2.5D/3D interposer designs.

[SOURCE: FOPLP Gains Traction in Advanced Semiconductor Packaging | https://semiengineering.com/foplp-gains-traction-in-advanced-semiconductor-packaging/ | 2023]

The driving motivation is AI accelerator scaling: GPU and AI ASIC designs have already hit or exceeded the standard single-reticle limit (~858 mm²), forcing multi-die, multi-reticle assemblies (e.g., TSMC's CoWoS-L) that must be fabricated on an interposer or carrier substantially larger than any single lithographic exposure field. Panel-level processing offers a path to building these oversized carriers more economically than wafer-based approaches by exploiting the higher area utilization and lower per-unit cost of rectangular panel formats.

[SOURCE: TSMC Achieves 98% Yield on CoWoS-L with 5.5x Reticle Size, 14x Comes in 2029 | https://www.techpowerup.com/351584/tsmc-achieves-98-yield-on-cowos-l-with-5-5x-reticle-size-14x-comes-in-2029 | 2025]

Two architectural variants dominate current development: chip-first (die-embed-first, RDL-last) and chip-last / RDL-first processing, where the redistribution layers are built on a temporary carrier before dies are attached, improving RDL yield since known-good RDL can be verified before expensive dies are committed.

[SOURCE: Chip-Last (RDL-First) Fan-Out Panel-Level Packaging (FOPLP) for Heterogeneous Integration | https://meridian.allenpress.com/jmep/article/17/3/89/445954/Chip-Last-RDL-First-Fan-Out-Panel-Level-Packaging | 2020]

---

## Key Research Findings

### Die Shift and Warpage: The Central Engineering Problem

The dominant technical obstacle in FOPLP is maintaining dimensional accuracy during the reconstitution and molding process. When dies are placed into a mold compound across a panel many times larger than a wafer, thermal expansion mismatches between the silicon dies, mold compound, and carrier cause dies to drift from their intended placement ("die shift") during cure, and the panel itself bows ("warpage") during thermal cycling.

A 2019 IEEE study quantified die-shift behavior during reconstitution, establishing the compaction and thermal variables that drive positional error in reconstituted wafers/panels — error that directly determines the achievable RDL line pitch, since finer redistribution lines require tighter die placement tolerance.

[SOURCE: Die Shift Assessment of Reconstituted Wafer for Fan-Out Wafer-Level Packaging | https://ieeexplore.ieee.org/document/8952624/ | 2019]

More recent modeling (2025) extends this to panel-level formats specifically, developing predictive models of die shift that jointly account for warpage-driven and resin-flow-driven displacement mechanisms with temperature-dependent material properties — a refinement necessary because panel-scale thermal gradients are non-uniform in ways that wafer-scale models do not capture.

[SOURCE: Die shift prediction of fan out panel level packages considering both warpage and flow induced mechanisms with temperature dependent properties | https://www.nature.com/articles/s41598-025-34076-2 | 2025]

Industry analysis frames this as a fundamental process-control discontinuity rather than a simple scale-up: techniques that are well-controlled at 300mm wafer scale (vacuum chucking, uniform cure ovens, lithographic alignment systems) behave differently or fail outright at 515mm x 510mm panel scale, requiring new equipment architectures for lamination, exposure, and inspection.

[SOURCE: What Breaks When Packaging Moves From Wafer To Panel? | https://semiengineering.com/what-breaks-when-packaging-moves-from-wafer-to-panel/ | 2024]

A complementary study on warpage specifically in RDL-first FOPLP builds found that the warpage trajectory across temperature cycling depends strongly on the sequencing of RDL dielectric cure versus mold compound cure, informing process windows for panel flatness.

[SOURCE: Panel Warpage of Fan-Out Panel Level Packaging using RDL-First Technology | https://www.researchgate.net/publication/334555995_Panel_Warpage_of_Fan-Out_Panel_Level_Packaging_using_RDL-First_Technology | 2019]

An IMAPS technical proceedings paper similarly catalogs the development path and remaining challenges of warpage control specific to FOWLP/FOPLP technology, treating warpage as the gating reliability variable for RDL fine-pitch scaling.

[SOURCE: Development and Challenges of Warpage for Fan-Out Wafer-Level Package Technology | https://imapsource.org/api/v1/articles/57203-development-and-challenges-of-warpage-for-fan-out-wafer-level-package-technology.pdf | n.d.]

### Inspection and Metrology Gaps

Because panels are rectangular and far larger than wafers, existing wafer-metrology tooling (built around circular-wafer handling and radially symmetric inspection patterns) does not transfer directly. A 2025 SemiEngineering technical analysis reports the industry is still developing inspection and metrology equipment capable of catching defects across full panel area at the resolution needed for fine-pitch RDL, identifying this as a trailing bottleneck relative to the maturity of the lamination/molding equipment itself.

[SOURCE: Inspection And Metrology Catching Up For High-Density Fan-Out Panel Packaging | https://semiengineering.com/inspection-and-metrology-catching-up-for-high-density-fan-out-panel-packaging/ | 2025]

### Chiplet Interconnect Defects and Test

As FOPLP is increasingly used to host multi-chiplet reconstituted packages, test and repair strategy becomes a first-order design concern. A 2025 arXiv paper on defect analysis and built-in self-test (BIST) for chiplet interconnects specifically targets fan-out wafer-level packaging structures, proposing test architectures to detect interconnect-level defects (e.g., RDL opens/shorts, micro-bump failures) introduced during the reconstitution and lamination steps.

[SOURCE: Defect Analysis and Built-In-Self-Test for Chiplet Interconnects in Fan-out Wafer-Level Packaging | https://arxiv.org/html/2503.14784v1 | 2025]

A related 2026 publication extends this to defect-aware BIST with dynamic repair capability for fan-out wafer-level packaging, suggesting the field is moving toward self-healing or redundancy-based interconnect schemes to manage the larger defect population inherent to bigger reconstituted areas.

[SOURCE: Defect-Aware Built-In Self-Test and Dynamic Repair for Fan-Out Wafer-Level Packaging | https://www.computer.org/csdl/journal/si/2026/03/11318978/2cSbFhZ5fuU | 2026]

### Floorplanning for Oversized Packages

The shift to reticle-exceeding package sizes also changes EDA requirements upstream of manufacturing. A 2025/2026-era arXiv paper on differentiable, routability-driven package floorplanning with pin assignment addresses the combinatorial floorplanning problem created when multiple chiplets, interposer tiles, and RDL routing must be co-optimized across a package substrate whose area is no longer bounded by a single reticle field.

[SOURCE: Differentiable Routability-Driven Package Floorplanning with Pin Assignment | https://arxiv.org/pdf/2607.15005 | n.d.]

### Glass Core Substrates as a Complementary Path

A parallel research track investigates structured glass substrates for wafer- and panel-level packaging, pursued specifically for their lower coefficient of thermal expansion (CTE) mismatch and superior flatness relative to organic or mold-compound carriers — directly addressing the warpage mechanism identified above as the central FOPLP reliability limiter.

[SOURCE: Structured glass substrates in wafer- and panel level packaging: Status and recent achievements | https://imapsource.org/api/v1/articles/68221-structured-glass-substrates-in-wafer-and-panel-level-packaging-status-and-recent-achievements.pdf | n.d.]

### Comprehensive Process Review

A broadly cited review paper on the design, materials, process, fabrication, and reliability of fan-out wafer-level packaging (authored in the lineage of J.H. Lau's packaging literature) provides the baseline process taxonomy — chip-first vs. chip-last, RDL-first vs. RDL-last — against which panel-level variants are benchmarked.

[SOURCE: Design, Materials, Process, Fabrication, and Reliability of Fan-Out Wafer-Level Packaging | https://www.academia.edu/83716774/Design_Materials_Process_Fabrication_and_Reliability_of_Fan_Out_Wafer_Level_Packaging | 2022]

### Material and Equipment Review (ScienceDirect)

A review on panel-level IC-package technology development documents the materials science progression (mold compounds, dielectric RDL materials, carrier materials) required to make panel sizes viable for IC-density redistribution, complementing the warpage-focused papers with a materials-selection lens.

[SOURCE: Panel level IC-package technology development | https://www.sciencedirect.com/science/article/abs/pii/S0026271422001810 | 2022]

---

## Patent Landscape

The patent record shows FOPLP/FOWLP intellectual property concentrated around four functional clusters: (1) warpage control, (2) RDL formation methods, (3) molding/reconstitution methods, and (4) stacking/PoP (package-on-package) integration.

**Warpage Control**
A patent directly titled for FOPLP warpage control describes structural and process innovations intended to counteract panel-scale bow during thermal cycling — direct evidence that warpage mitigation is treated as patentable, commercially defensible IP rather than a solved process parameter.

[SOURCE: US11450620B2 — Innovative fan-out panel level package (FOPLP) warpage control | https://patents.google.com/patent/US11450620B2/en | n.d.]

**Foundational Fan-Out Molding**
Earlier foundational patents establish the "fully molded fan-out" concept — embedding dies fully within a molded carrier prior to RDL buildup — which underlies both wafer- and panel-level fan-out approaches.

[SOURCE: US20130168849A1 — Fully Molded Fan-Out | https://patents.google.com/patent/US20130168849A1/en | n.d.]

Earlier still, a patent for manufacturing a fan-out embedded panel-level package describes an early (2011-era) construction method explicitly using "panel level" terminology, indicating the panel concept predates the current AI-driven resurgence by over a decade.

[SOURCE: US20110156239A1 — Method for manufacturing a fan-out embedded panel level package | https://patents.google.com/patent/US20110156239A1/en | n.d.]

**RDL Formation and Fine-Pitch Routing**
A patent describing a method for building up fan-out RDL structures with fine-pitch line width and line spacing addresses the lithographic challenge of achieving fine RDL geometries on large-format, non-wafer carriers — a key enabler since panel-scale exposure tools historically had coarser resolution than wafer steppers.

[SOURCE: US10504845 — Method for building up a fan-out RDL structure with fine pitch line-width and line-spacing | https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/10504845 | n.d.]

A related patent covers RDL and vertical interconnect formation using laser direct structuring (LDS), an alternative patterning method to conventional photolithography that is of particular interest for panel formats where traditional stepper exposure fields do not match panel dimensions.

[SOURCE: US9330994 — Semiconductor device and method of forming RDL and vertical interconnect by laser direct structuring | https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/9330994 | n.d.]

Further RDL-adjacent IP includes an embedded resistor-capacitor film for fan-out wafer-level packaging, integrating passive components directly into the RDL stack to save package area — relevant to panel-level designs where routing density competes with passive integration.

[SOURCE: US20190139911A1 — Embedded Resistor-Capacitor Film for Fan Out Wafer Level Packaging | https://patents.google.com/patent/US20190139911A1/en | n.d.]

**Stacking and Package-on-Package**
A patent on fan-out packaging PoP mechanical attach methods addresses how additional packages (e.g., memory) are mechanically and electrically stacked atop a fan-out base package — a capability carried forward into panel-scale heterogeneous integration schemes.

[SOURCE: US20190312016A1 — Fan out packaging pop mechanical attach method | https://patents.google.com/patent/US20190312016A1/en | n.d.]

A multi-molding method patent for fan-out stacked semiconductor packages addresses sequential or multi-stage molding steps needed when stacking multiple reconstituted layers, a process complexity that scales with panel size.

[SOURCE: US20210280522A1 — Multi-molding method for fan-out stacked semiconductor package | https://patents.google.com/patent/US20210280522A1/en | n.d.]

**Wafer-Level Baseline and Chinese Patent Activity**
Baseline wafer-level packaging method patents continue to be filed and remain foundational prior art against which panel-level claims are differentiated.

[SOURCE: US9396999B2 — Wafer level packaging method | https://patents.google.com/patent/US9396999B2/en | n.d.]

Chinese patent filings show parallel independent development activity, including a fan-out wafer-level chip scale packaging and manufacturing method patent and a separate fan-out type wafer-level packaging method for semiconductor devices, indicating active competing IP development outside the traditional Taiwan/US/Korea packaging-IP axis.

[SOURCE: CN111725074A — Fan-Out Wafer-Level Chip Scale Packaging and Manufacturing Method | https://patents.google.com/patent/CN111725074A/en | n.d.]

[SOURCE: CN105957836A — Fan-out type wafer-level packaging method for semiconductor device | https://patents.google.com/patent/CN105957836A/en | n.d.]

**Adjacent Equipment Patent**
A recently indexed patent on substrate architecture for enhanced electrostatic chucking is relevant to panel handling during process steps, since electrostatic chucking methods designed for round wafers must be re-engineered for rectangular, larger-area panel substrates to maintain flatness during exposure and processing.

[SOURCE: Substrate architecture for enhanced electrostatic chucking | https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12660680 | n.d.]

---

## Industry and Competitive Landscape

### TSMC: CoWoS-L and the CoPoS Transition

TSMC's current reticle-exceeding packaging technology, CoWoS-L, uses a local silicon interconnect (LSI) bridge approach rather than a full monolithic interposer, and has reportedly achieved 98% yield at 5.5x reticle size, with a roadmap extending to 14x reticle size by 2029.

[SOURCE: TSMC Achieves 98% Yield on CoWoS-L with 5.5x Reticle Size, 14x Comes in 2029 | https://www.techpowerup.com/351584/tsmc-achieves-98-yield-on-cowos-l-with-5-5x-reticle-size-14x-comes-in-2029 | 2025]

TSMC has publicly stated that panel-level packaging will not replace CoWoS for the largest future AI processors in the near term, asserting that wafer-level technology can scale to packages containing up to 58 dies within a single reticle-exceeding assembly.

[SOURCE: TSMC says panel packaging won't replace CoWoS anytime soon for the largest future AI processors | https://www.tomshardware.com/tech-industry/semiconductors/tsmc-says-panel-packaging-wont-replace-cowos-anytime-soon-for-the-largest-future-ai-processors-wafer-level-tech-can-scale-to-58-massive-dies-in-one-package | 2025]

Despite that public positioning, industry reporting describes TSMC developing "CoPoS" (Chip on Panel on Substrate) as its next-generation rectangular panel packaging technology, representing a structural shift from circular wafer-based CoWoS processing toward panel-based processing for future AI package generations.

[SOURCE: TSMC's CoPoS: The Revolutionary Shift to Rectangular Panel Packaging | https://markets.financialcontent.com/wral/article/tokenring-2026-2-2-tsmcs-copos-the-revolutionary-shift-to-rectangular-panel-packaging | 2026]

Market/supply-chain analysis frames CoPoS as addressing the supply-demand gap in advanced packaging capacity that CoWoS alone cannot meet as AI chip demand grows, with a projected 2026–2029 roadmap.

[SOURCE: CoWoS to CoPoS: Is TSMC's Next-Gen Advanced Packaging Finally Closing the Supply-Demand Gap for AI Chips? | https://supplyics.com/insights/market-intelligence/cowos-to-copos-tsmc-advanced-packaging-roadmap-2026-2029/ | 2026]

A separate analysis piece speculates on the value-chain implications of CoPoS adoption — i.e., which equipment and materials suppliers stand to benefit first if TSMC commits to rectangular panel formats at scale.

[SOURCE: If CoPoS Arrives, Who Makes Money First | https://damnang2.substack.com/p/if-copos-arrives-who-makes-money | 2025]

### Samsung: Early Mover in Panel-Level Packaging

Multiple industry reports (TrendForce, DigiTimes, TweakTown) indicate Samsung has moved earlier than TSMC into commercial panel-level packaging, reportedly targeting high-performance computing and advanced packaging customers including automotive/AI compute demand from Tesla.

[SOURCE: [News] Samsung Gains Early Market Entry Advantage in the Panel-Level Packaging Sector Ahead of TSMC | https://www.trendforce.com/news/2024/06/25/news-samsung-gains-early-market-entry-advantage-in-the-panel-level-packaging-sector-ahead-of-tsmc/ | 2024]

[SOURCE: Samsung aims to meet Tesla's chip packaging demand with new panel-level technology | https://www.digitimes.com/news/a20250814PD213/samsung-packaging-tesla-technology-demand.html | 2025]

Reporting also indicates Samsung and TSMC are diverging on panel carrier materials — a materials-strategy fork that will likely determine which warpage-control and RDL-fine-pitch approaches become dominant industry practice.

[SOURCE: Samsung, TSMC diverge on panel materials for advanced packaging | https://www.digitimes.com/news/a20241225PD212/samsung-tsmc-foplp-packaging-materials.html | 2024]

More recent reporting (August 2025) describes Samsung pushing "SoP" (System on Panel) packaging on ultra-large panel formats, explicitly framed as a competitive challenge to both TSMC and Intel within the Tesla AI-compute supply chain.

[SOURCE: [News] Samsung Reportedly Pushes SoP Packaging on Ultra-Large Panel, Challenging TSMC and Intel | https://www.trendforce.com/news/2025/08/13/news-samsung-pushes-sop-packaging-on-ultra-large-panel-challenging-tsmc-and-intel-in-tesla-supply-chain/ | 2025]

TrendForce frames panel-level packaging broadly as a new competitive battleground among TSMC, Samsung, and Intel.

[SOURCE: [News] New Battleground for TSMC, Samsung & Intel in Panel-Level Packaging | https://www.trendforce.com/news/2024/06/21/news-new-battleground-for-tsmc-samsung-intel-in-panel-level-packaging/ | 2024]

### Intel: EMIB and Reticle-Exceeding Encapsulation

Intel's competing approach centers on Embedded Multi-die Interconnect Bridge (EMIB) rather than full fan-out panel packaging, though Intel has also published on scaling advanced packaging encapsulation to 24x reticle size and beyond, addressing the mold/encapsulation engineering needed for hyper-large chip assemblies regardless of carrier geometry.

[SOURCE: Intel Cracks The Encapsulation Wall Blocking Hyper-Large Chips, Pushing Advanced Packaging To 24x Reticle Size and Beyond | https://wccftech.com/intel-cracks-encapsulation-wall-blocking-hyper-large-chips-using-advanced-packaging/ | 2025]

Intel's own foundry communications describe its advanced packaging program as leading the industry in scaling past reticle limits, positioning EMIB and related technologies as competitive alternatives to both CoWoS/CoPoS and FOPLP approaches.

[SOURCE: Intel Foundry's Advanced Packaging Innovations Lead the Industry in Scaling Past Reticle Limits | https://community.intel.com/t5/Blogs/Intel-Foundry/Systems-Foundry-for-the-AI-Era/Intel-Foundry-s-Advanced-Packaging-Innovations-Lead-the-Industry/post/1738888 | 2025]

A comparative analysis contrasts Intel's EMIB bridge-based approach against TSMC's CoWoS interposer-based approach as the two dominant competing philosophies for reticle-exceeding packaging.

[SOURCE: Advanced Packaging: Intel's EMIB vs TSMC's CoWoS | https://www.chipstrat.com/p/advanced-packaging-intels-emib-vs | 2025]

### Market Sizing

Independent market research projects the panel-level packaging market reaching USD 3.41 billion by 2032, driven by demand for compact, high-density advanced semiconductor packaging solutions.

[SOURCE: Panel Level Packaging Market Size to Hit USD 3.41 Billion by 2032 | https://finance.yahoo.com/news/panel-level-packaging-market-size-130000205.html | 2024]

Yole Group's strategic analysis frames 2024 as an inflection point asking whether panel-level packaging is "finally emerging" after years of being a perennially-promised but slow-to-commercialize technology.

[SOURCE: Is Panel-Level Packaging (PLP) finally emerging? | https://www.yolegroup.com/strategy-insights/is-panel-level-packaging-plp-finally-emerging/ | 2024]

An earlier (2021) IMAPS/3D InCites report had already documented panel-level packaging "taking off" as a technology trend, indicating the current 2024–2026 commercialization wave follows at least one prior hype cycle.

[SOURCE: Fan Out Panel Level Packaging Takes Off | https://www.3dincites.com/2021/03/fan-out-panel-level-packaging-takes-off/ | 2021]

A 2023 SemiEngineering piece similarly documented "the rise of panel-level packaging" as an industry trend gaining real commercial traction rather than remaining purely experimental.

[SOURCE: The Rise Of Panel-Level Packaging | https://semiengineering.com/the-rise-of-panel-level-packaging/ | 2024]

Broader competitive framing describes the advanced packaging market as structured around a "TSMC monopoly" with Intel as the primary challenger and Amkor operating as an outsourced assembly/test (OSAT) arms-dealer supplying packaging capacity to multiple fabless customers.

[SOURCE: Battle for Advanced Packaging: TSMC Monopoly, Intel Challenge, and Amkor Arms-Dealing | https://www.viksnewsletter.com/p/battle-for-advanced-packaging-tsmc-intel-amkor | 2025]

---

## Future Implications

**Panel-scale processing as a structural requirement, not an optimization.** The trajectory of AI accelerator die area — already exceeding single-reticle limits and driving TSMC toward 14x-reticle CoWoS-L packages by 2029 — means reticle-exceeding packaging is becoming the default case for flagship AI silicon rather than an edge case. [SOURCE: TSMC Achieves 98% Yield on CoWoS-L with 5.5x Reticle Size, 14x Comes in 2029 | https://www.techpowerup.com/351584/tsmc-achieves-98-yield-on-cowos-l-with-5-5x-reticle-size-14x-comes-in-2029 | 2025] Given this, panel-level formats' area-utilization advantage over circular wafers (a rectangular panel wastes far less area at the edges than a circular wafer when tiling large rectangular dies/packages) positions FOPLP/CoPoS-style approaches as the probable long-term economic winner for the largest packages, even though TSMC currently maintains CoWoS as sufficient for near-term roadmaps. [SOURCE: TSMC says panel packaging won't replace CoWoS anytime soon for the largest future AI processors | https://www.tomshardware.com/tech-industry/semiconductors/tsmc-says-panel-packaging-wont-replace-cowos-anytime-soon-for-the-largest-future-ai-processors-wafer-level-tech-can-scale-to-58-massive-dies-in-one-package | 2025]

**Glass core substrates as the probable warpage solution.** Since warpage and die-shift are repeatedly identified as the central reliability-limiting mechanisms across multiple independent studies [SOURCE: Die shift prediction of fan out panel level packages considering both warpage and flow induced mechanisms with temperature dependent properties | https://www.nature.com/articles/s41598-025-34076-2 | 2025] [SOURCE: Panel Warpage of Fan-Out Panel Level Packaging using RDL-First Technology | https://www.researchgate.net/publication/334555995_Panel_Warpage_of_Fan-Out_Panel_Level_Packaging_using_RDL-First_Technology | 2019], and glass substrates are being actively researched specifically for their CTE and flatness advantages [SOURCE: Structured glass substrates in wafer- and panel level packaging: Status and recent achievements | https://imapsource.org/api/v1/articles/68221-structured-glass-substrates-in-wafer-and-panel-level-packaging-status-and-recent-achievements.pdf | n.d.], it is reasonable to project that glass-core or glass-reinforced panel carriers will be the technology that finally unlocks fine-pitch RDL at full panel scale. [UNVERIFIED: specific timeline or company commitment to glass-core panel carriers reaching high-volume manufacturing]

**Built-in self-test and dynamic repair will become standard for panel-scale packages.** As reconstituted panels host more chiplets across larger areas, defect density per package rises proportionally with area, which is precisely why BIST and dynamic repair research is already appearing for fan-out structures. [SOURCE: Defect-Aware Built-In Self-Test and Dynamic Repair for Fan-Out Wafer-Level Packaging | https://www.computer.org/csdl/journal/si/2026/03/11318978/2cSbFhZ5fuU | 2026] This suggests future packages will increasingly resemble fault-tolerant distributed systems at the interconnect level, with redundant RDL paths and runtime-repairable links — a convergence of packaging engineering and classical fault-tolerant systems design.

**EDA tooling must co-evolve with panel-scale floorplanning.** The emergence of differentiable, routability-aware floorplanning tools targeting package-level (not just die-level) optimization [SOURCE: Differentiable Routability-Driven Package Floorplanning with Pin Assignment | https://arxiv.org/pdf/2607.15005 | n.d.] indicates EDA vendors are treating the package substrate itself as a first-class design surface requiring automated placement/routing — a discipline historically reserved for die-level layout. This implies future packaging flows will blur further into standard chip design toolchains.

**Competitive bifurcation between bridge-based and full-panel approaches may persist.** Intel's EMIB-centric strategy and TSMC's interposer/CoWoS-L strategy represent architecturally distinct answers to the reticle problem [SOURCE: Advanced Packaging: Intel's EMIB vs TSMC's CoWoS | https://www.chipstrat.com/p/advanced-packaging-intels-emib-vs | 2025], and Samsung's early FOPLP/SoP commercialization suggests a third path is gaining customer traction independently. [SOURCE: [News] Samsung Reportedly Pushes SoP Packaging on Ultra-Large Panel, Challenging TSMC and Intel | https://www.trendforce.com/news/2025/08/13/news-samsung-pushes-sop-packaging-on-ultra-large-panel-challenging-tsmc-and-intel-in-tesla-supply-chain/ | 2025] It is plausible no single architecture wins outright, with bridge-based, interposer-based, and panel-based packaging coexisting for different die-count and power envelopes. [UNVERIFIED: long-term market share projections distinguishing EMIB-style vs. CoWoS-style vs. FOPLP-style adoption by package segment]

---

## Continuity Hooks

- **Prior-article linkage — Reticle-Limit Die Design:** This dossier presumes familiarity with the underlying reticle-size constraint (~858 mm² max single-exposure die area) that motivates both chiplet disaggregation and advanced packaging generally; a dedicated prior/companion piece on reticle-limit lithography economics would sharpen the "why packaging must scale past the reticle" framing used throughout this piece.
- **Forward-article linkage — Wafer-Scale Integration (Cerebras):** The dossier's AI-scaling context connects directly to wafer-scale integration as an alternative, more radical answer to the same reticle-limit problem — Cerebras's approach avoids packaging-level stitching entirely by fabricating a single wafer-scale compute surface. [SOURCE: A Comparison of the Cerebras Wafer-Scale Integration Technology with Nvidia GPU-based Systems for Artificial Intelligence | https://arxiv.org/pdf/2503.11698 | n.d.] A future deep dive contrasting FOPLP/CoPoS against wafer-scale integration as competing philosophies (package many reticle-sized dies vs. never dice the wafer at all) is a natural next article.
- **Forward-article linkage — Chiplet Design-Space Exploration:** The reinforcement-learning-based chiplet accelerator design optimization work is a strong candidate for a follow-up piece on how EDA/ML tooling is adapting to chiplet-based design flows that presuppose FOPLP-style packaging as the physical substrate. [SOURCE: Chiplet-Gym: Optimizing Chiplet-based AI Accelerator Design with Reinforcement Learning | https://arxiv.org/pdf/2406.00858 | n.d.]
- **Forward-article linkage — CoPoS Supply Chain:** The equipment/materials value-chain speculation around TSMC's CoPoS transition is underdeveloped in current public reporting and is a strong candidate for a dedicated follow-up once TSMC provides firmer roadmap commitments. [SOURCE: If CoPoS Arrives, Who Makes Money First | https://damnang2.substack.com/p/if-copos-arrives-who-makes-money | 2025]
- **Forward-article linkage — Trillion-Transistor GPU roadmap:** The broader narrative of GPUs approaching trillion-transistor scale ties panel-level packaging directly to the headline AI hardware narrative and would make a strong framing device for a reader-facing follow-up. [SOURCE: How to Achieve a Trillion-Transistor GPU | https://www.allpcb.com/allelectrohub/how-to-achieve-a-trillion-transistor-gpu | 2024]

---

## Unverified Claims

- [UNVERIFIED: Exact standardized panel dimension across the industry — the commonly cited 515mm x 510mm figure appears in industry trade coverage but was not independently confirmed against a standards document (e.g., SEMI standard) in the sources gathered.]
- [UNVERIFIED: Specific technical definition and architecture of TSM