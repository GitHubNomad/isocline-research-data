I'll research this topic thoroughly, focusing on the mandatory sources for sparse/structured attention accelerators, edge inference hardware, and long-context on-device processing.
The results are returned as JSON strings, need to parse them properly.
Excellent, rich set of results. Let me dig deeper into the most relevant papers with full content, plus search for patents and RFC/networking angles.
Now let me search Google Patents specifically and get detailed content from the key arXiv papers.
Hit a rate limit. Let me wait and try fewer queries at a time.
Let me continue with deep-diving the papers I already have while the search tool cools down.
The search tool seems to be rate-limited for an extended period. Let me continue gathering deep details from the arXiv papers already found, which is the priority source anyway, and retry patents later.
Let me check what content I already have cached from the initial searches for NSA, SALO, DynaX and other key papers — those results already included substantial content excerpts.
I have solid content on NSA already. Let me review the other cached results (SALO, DynaX, and edge/on-device papers) for details.
Now let's look at the on-device/edge-specific papers in detail (IntAttention, NPU inference, TENET, H2EAL, Salca).
Let me get more specific content on Salca's abstract/intro, and search for additional key papers: STAR accelerator, MInference, and CIM-based approaches, plus try patents again.
The rate limit seems persistent. Let me try a longer wait.
I'll proceed with the substantial data already collected, since the search tool appears to be persistently rate-limited. Let me check all cached results comprehensively for additional useful content before finalizing.
This is a rich dataset. I have 64 unique results. Let me examine additional key ones I haven't fully reviewed: SpAtten, Energon, TENET, FAMOUS, STAR, patsnap patent article, and Qwen2.5-1M.
This PatSnap article is very useful for patent landscape info. Let me try the search tool once more for direct patent database queries, then check a few more important papers (SpAtten details, memristor, CIM).
The search tool appears to be unavailable for this session (persistent rate limiting beyond reasonable retry windows). I have substantial, high-quality data already gathered from arXiv and ACM sources covering the core topic comprehensively. Let me check the PatSnap article and a couple more cached results for patent-specific detail, then compile the dossier with what's verified.
That's the full excerpt available. Let me check the memristor CIM paper and SpAtten details more fully.
Let me try the search tool once more with a much longer wait, targeting specifically patent and RFC sources which I still need.
The search tool is unavailable for the remainder of this session. I have gathered comprehensive, high-quality data from arXiv and ACM (16+ primary technical papers plus a patent-landscape secondary source). I'll now compile the dossier, clearly flagging patent claims that lack direct verification via Google Patents/USPTO access as unverified, per the zero-hallucination policy.
I now have sufficient depth. Let me compile the complete Technical Dossier.
# Technical Dossier: Sparse and Structured Attention Accelerators for On-Device Long-Context Edge Inference

**Prepared by:** Mimir, Lead Researcher — Project Isocline
**Prepared for:** Brian / Isocline orchestrator
**Classification:** Deep-Dive Research Brief

---

## 1. Overview

Transformer attention is the dominant computational bottleneck in long-context inference: the standard mechanism requires every query token to attend to every key token, producing quadratic time and memory scaling as context length grows. This is not merely a training-time concern — it is now the central obstacle to running long-context models *on-device*, where DRAM bandwidth, SRAM capacity, and power envelopes are 1–2 orders of magnitude more constrained than in datacenter GPUs.

Two complementary research threads have emerged to address this:

1. **Algorithmic sparsity** — reducing the *set* of query-key pairs that must be computed, via structured block-sparse patterns, dynamic top-k token/block selection, hierarchical compression, or row-wise clustering.
2. **Hardware-algorithm co-design** — building spatial architectures, systolic arrays, compute-in-memory (CIM) macros, or near-memory/near-storage accelerators that natively execute these sparse patterns without the overhead of a general-purpose GPU/CPU memory hierarchy.

The field has converged on the idea that sparsity patterns must be **hardware-aligned** rather than purely algorithmic afterthoughts — a principle made explicit by DeepSeek's Native Sparse Attention (NSA), which frames the problem as achieving 
"substantial speedups through arithmetic intensity-balanced algorithm design, with implementation optimizations for modern hardware"
 while remaining natively trainable end-to-end.

`[SOURCE: Native Sparse Attention: Hardware-Aligned and Natively Trainable Sparse Attention | https://arxiv.org/abs/2502.11089 | 2025]`

For edge deployment specifically, the constraint set changes qualitatively: designs must jointly optimize for (a) quadratic attention cost at long context, (b) linear-growing KV-cache memory pressure, (c) narrow mobile DRAM/LPDDR bandwidth, and (d) strict power budgets (smartphone NPUs, wearables, embedded SoCs). This dossier surveys the state of the art across these dimensions.

---

## 2. Key Research Findings

### 2.1 Algorithmic Sparse Attention Foundations

**Native Sparse Attention (NSA) — DeepSeek-AI (2025).** NSA is arguably the most influential recent architecture bridging algorithm and hardware. It 
employs a dynamic hierarchical sparse strategy, combining coarse-grained token compression with fine-grained token selection to preserve both global context awareness and local precision
. Critically, the kernel design is hardware-native: 
"the kernel loads queries by GQA groups (Grid Loop), fetches corresponding sparse KV blocks (Inner Loop), and performs attention computation on SRAM"
, explicitly distinguishing data resident in fast SRAM versus slower HBM. NSA demonstrated that sparse attention can be natively trained rather than retrofitted post-hoc, with pretrained models 
maintaining or exceeding Full Attention models across general benchmarks, long-context tasks, and instruction-based reasoning
, while achieving substantial decode/prefill speedups at 64K-token sequence lengths.

`[SOURCE: Native Sparse Attention: Hardware-Aligned and Natively Trainable Sparse Attention | https://arxiv.org/abs/2502.11089 | 2025]`
`[SOURCE: Native Sparse Attention: Hardware-Aligned and Natively Trainable Sparse Attention (kernel design) | https://arxiv.org/html/2502.11089v1 | 2025]`

**MInference 1.0 (Microsoft Research, NeurIPS 2024).** This work targets the *pre-filling* phase specifically — the most expensive step for long-context prompts — using dynamic sparse attention patterns identified per-head at runtime. It has since been adopted downstream: Alibaba's **Qwen2.5-1M Technical Report** confirms production use, noting that at million-token input lengths 
"the time spent on the attention mechanism can account for over 90% of the total forward pass time"
, and that the team 
"implemente[d] a sparse attention mechanism based on MInference... to accelerate the prefill phase"
, layering in chunked prefill and a sparse refinement method to control accuracy loss.

`[SOURCE: MInference 1.0: Accelerating Pre-filling for Long-Context LLMs via Dynamic Sparse Attention | https://arxiv.org/pdf/2407.02490 | 2024]`
`[SOURCE: Qwen2.5-1M Technical Report | https://arxiv.org/pdf/2501.15383 | 2025]`

**Structured N:M and X:M Pruning.** A parallel line of work targets *fine-grained structured* sparsity compatible with hardware systolic execution rather than unstructured sparsity. The foundational **Dynamic N:M Fine-Grained Structured Sparse Attention** work (DFSS) established that attention weight matrices can be dynamically pruned to N:M patterns as a good empirical and theoretical approximation of full attention. Building on this, **DynaX** (ASPLOS 2025) generalizes the fixed-N constraint: 
"DynaX dynamically selects variable X (rather than a fixed N) important scores from a group via a 2-step pruning method, which results in high sparsity and less prediction memory overhead while maintaining pattern regularity"
. This targets the core tension in dynamic sparsity — irregular sparsity patterns are accuracy-friendly but hardware-hostile, while rigid structured patterns are hardware-friendly but can miss important tokens.

`[SOURCE: DynaX: Sparse Attention Acceleration with Dynamic X:M Fine-Grained Structured Pruning | https://dl.acm.org/doi/abs/10.1145/3676641.3715991 | 2025]`
`[SOURCE: Dynamic N:M Fine-grained Structured Sparse Attention Mechanism | https://arxiv.org/pdf/2203.00091 | 2022]`

**Row-wise Clustering (SpARC, DAC 2024).** Rather than pruning individual score entries, SpARC exploits redundancy *between* rows of the attention map: 
"By employing row-wise clustering, attention scores are calculated only once per cluster to achieve approximate attention without seriously compromising accuracy."
 The reported results show 
"attention map sparsity levels of 85-90% with negligible accuracy loss"
 and up to 4× core attention speedup versus prior sparse accelerators.

`[SOURCE: SpARC: Token Similarity-Aware Sparse Attention Transformer Accelerator via Row-wise Clustering | https://dl.acm.org/doi/10.1145/3649329.3655936 | 2024]`

### 2.2 Spatial and ASIC Accelerator Architectures

**SpAtten (HPCA 2021)** remains a foundational reference architecture. Where most prior sparse accelerators exploited *weight* sparsity, 
"SpAtten leverages activation (token/head) sparsity and employs specialized top-k engines to support on-the-fly cascade token/head pruning"
, and further supports adjustable bitwidth quantization driven by the attention probability distribution itself rather than a fixed compile-time setting.

`[SOURCE: SpAtten: Efficient Sparse Attention Architecture with Cascade Token and Head Pruning | https://arxiv.org/pdf/2012.09852 | 2021]`

**SALO (DAC 2022, Shanghai Jiao Tong University).** SALO targets long-sequence hybrid sparse attention patterns (e.g., Longformer-style local+global masks) with a dedicated data scheduler and spatial array: 
"SALO contains a data scheduler to map hybrid sparse attention patterns onto hardware and a spatial accelerator to perform the efficient attention computation."
 Reported results show 
"17.66x and 89.33x speedup on average compared to GPU and CPU implementations, respectively, on typical workloads, i.e., Longformer and ViL"
.

`[SOURCE: SALO: An Efficient Spatial Accelerator Enabling Hybrid Sparse Attention Mechanisms for Long Sequences | https://arxiv.org/pdf/2206.14550 | 2022]`

**STAR (Cross-Stage Tiling, 2025).** This more recent spatial-architecture paper explicitly frames the pruning rationale in linguistic terms — that 
"tokens like articles 'a' or 'the' contribute negligibly to semantics and yield near-zero attention values"
 after softmax suppression, meaning the corresponding K/V vectors can be pruned with minimal output degradation. STAR's contribution is a cross-stage tiling strategy that unifies the scheduling of the *prediction* (which tokens matter) and *execution* (compute only on selected tokens) stages, which prior dynamic-sparsity accelerators treated as separate pipeline phases with prediction overhead.

`[SOURCE: Designing Spatial Architectures for Sparse Attention: STAR Accelerator via Cross-Stage Tiling | https://arxiv.org/pdf/2512.20198 | 2025]`

**PADE (Predictor-Free Sparse Attention Accelerator, HPCA 2026).** PADE eliminates the separate lightweight-predictor stage that most dynamic sparse accelerators (SpAtten, STAR, DynaX-style designs) rely on to guess which blocks matter, instead fusing prediction and execution stages directly — reducing control overhead that otherwise erodes the benefits of sparsity at edge power budgets.

`[SOURCE: PADE: A Predictor-Free Sparse Attention Accelerator via Unified Execution and Stage Fusion | https://arxiv.org/pdf/2512.14322 | 2026]`

**FAMOUS (FPGA, UltraScale+).** On the reconfigurable-fabric side, FAMOUS demonstrates that flexible FPGA attention accelerators can approach ASIC-class throughput: it 
"achieves comparable throughput with state-of-the-art ASIC accelerators, which operate at higher frequencies and leverage sparsity to reduce computation and resources"
, reaching a measured 
"maximum throughput of 328 GOPS"
 while remaining runtime-programmable across different transformer topologies without new synthesis — a valuable property for edge devices that must support multiple model variants without re-fabricating silicon.

`[SOURCE: FAMOUS: Flexible Accelerator for the Attention Mechanism of Transformer on UltraScale+ FPGAs | https://arxiv.org/html/2409.14023v2 | 2024]`

### 2.3 Edge- and Mobile-Specific Systems

**H2EAL (2025) — Hybrid-Bonding Architecture.** This is one of the most directly on-topic papers: an edge accelerator combining hybrid (static + dynamic) sparse attention with a hybrid-bonding (advanced 3D packaging) memory-compute architecture. The authors report that 
"H2EAL proposes memory-compute co-placement and adaptive heterogeneous mapping"
 to resolve KV-cache management and workload imbalance, achieving 
"5.20 ∼ 48.21× speedup and 6.22 ∼ 73.48× energy efficiency improvement with negligible accuracy degradation"
 versus baseline edge inference.

`[SOURCE: H2EAL: Hybrid-Bonding Architecture with Hybrid Sparse Attention for Efficient Long-Context LLM Inference | https://arxiv.org/pdf/2508.16653 | 2025]`

**Salca (2026) — Sparsity-Aware Long-Context Decoding.** Salca is positioned specifically for the *decode* phase of long-context attention (as opposed to prefill), which is more memory-bandwidth-bound than compute-bound — a distinction highly relevant to edge NPUs where DRAM bandwidth, not FLOPs, is usually the binding constraint. Its bibliography situates it directly against SpAtten, MInference, SeerAttention, and TidalDecode as the comparison set for sparsity-aware decoding accelerators.

`[SOURCE: Salca: A Sparsity-Aware Hardware Accelerator for Efficient Long-Context Attention Decoding | https://arxiv.org/pdf/2604.24820 | 2026]`

**TENET — LUT-Centric Ternary LLM Architecture for Edge.** TENET targets extreme low-bit (ternary, BitNet-style) LLMs on edge hardware and specifically exploits *inter-token* sparsity via a 
"Linear-Projection-aware Sparse Attention dataflow that exploits inter-token sparsity in the attention mechanism to enable data reuse and computation fusion between QKV linear projections and attention"
, which eliminates redundant memory accesses during prefilling. The design was validated on both FPGA and ASIC platforms, and the authors report the resulting 
"optimized Sparse BitNet achieves comparable accuracy compared to the vanilla full precision Llama LLM with the same model size"
.

`[SOURCE: TENET: An Efficient Sparsity-Aware LUT-Centric Architecture for Ternary LLM Inference On Edge | https://arxiv.org/pdf/2509.13765 | 2025]`

**IntAttention — Fully Integer Attention Pipeline (Arm CPUs, 2025).** This paper attacks a different but complementary axis — datatype overhead rather than token-count sparsity. It 
"integrates sparsity-aware clipping, a 32-entry lookup table approximation, and direct integer normalization, thereby eliminating datatype conversion overhead along the attention path"
, reporting 
"up to 3.7x speedup and 61% energy reduction over FP16 baselines, and up to 2.0x speedup over conventional INT8 attention pipelines"
 on Armv8 CPU targets — directly relevant to smartphone-class edge inference without dedicated NPU silicon.

`[SOURCE: IntAttention: A Fully Integer Attention Pipeline for Efficient Edge Inference | https://arxiv.org/abs/2511.21513 | 2025]`

**Fast On-Device LLM Inference with NPUs (ASPLOS 2025).** This paper focuses on mapping LLM inference efficiently onto commercial mobile NPUs, addressing the mismatch between NPU-friendly dense/regular compute patterns and the dynamic, irregular nature of attention sparsity and token-level operations in real deployments (e.g., prefill/decode heterogeneity).

`[SOURCE: Fast On-device LLM Inference with NPUs | https://dl.acm.org/doi/10.1145/3669940.3707239 | 2025]`

**AccLLM — Algorithm-Hardware Co-Design for Edge Long-Context Inference (2025).** AccLLM combines structured pruning, shape-aware attention, and aggressive mixed-precision quantization: 
"we integrate (1) pruning, (2) Λ-shaped attention, and (3) an innovative W2A8KV4 (2-bit weights, 8-bit activations, and 4-bit KV cache) quantization scheme, thus effectively reducing memory and bandwidth requirements while facilitating LLMs' long-sequence generation"
 — directly targeting the memory/bandwidth bottleneck that dominates edge long-context inference cost.

`[SOURCE: AccLLM: Accelerating Long-Context LLM Inference Via Algorithm-Hardware Co-Design | https://arxiv.org/abs/2505.03745 | 2025]`

### 2.4 Memory-Centric and Storage-Offload Approaches (Adjacent but Important)

While not classical "accelerator" papers, several memory-hierarchy innovations are essential complements to sparse-attention silicon at the edge:

- **InstInfer** offloads KV-cache-heavy attention computation to Computational Storage Drives (CSDs), noting that increasing context and batch size 
"escalate the memory requirement of the key-value (KV) cache, which imposes a huge burden on the GPU VRAM, especially for resource-constraint scenarios (e.g., edge computing and personal devices)"
.
`[SOURCE: InstInfer: In-Storage Attention Offloading for Cost-Effective Long-Context LLM Inference | https://arxiv.org/html/2409.04992v1 | 2024]`

- **Compute-in-Memory (CIM) attention accelerators** are an increasingly important adjacent hardware class. Referenced designs in the survey literature include a 
"28nm 15.59 μj/token full-digital bitline-transpose cim-based sparse transformer accelerator with pipeline/parallel reconfigurable modes"
, indicating that fused sparsity + in-memory-compute is an active silicon research direction for ultra-low-energy edge inference.
`[SOURCE: Efficient memristor accelerator for transformer self-attention functionality | https://pmc.ncbi.nlm.nih.gov/articles/PMC11480463/ | 2024]`

---

## 3. Patent Landscape

**Important methodological note:** Direct programmatic access to Google Patents and the USPTO Full-Text Database was not available during this research session (the search tool encountered a persistent rate-limit/availability issue). The patent-landscape claims below are therefore drawn from a single secondary aggregator source (PatSnap) that itself references specific filed patents, rather than from direct primary-source patent document retrieval. **This entire section should be treated as lower-confidence pending direct USPTO/Google Patents verification in a follow-up research pass.**

- A patent attributed to Sogang University's industry-academic cooperation foundation describes a method that 
"first performs a full attention operation for an initial number of steps to compute baseline attention scores, then applies convolution patterns to detect diagonal and vertical structural regularities in those scores"
, subsequently encoding this into a reusable sparse mask to avoid expensive per-inference pattern search — notable because it is explicitly framed as 
"particularly well-suited to deployment scenarios where calibration data is unavailable"
, an edge-relevant constraint.
`[UNVERIFIED: Direct patent number, filing date, and claim scope for the Sogang University attention-mask patent — sourced only via secondary aggregator, not confirmed against USPTO/Google Patents primary record]`

- A separate filing attributed to Zhejiang Lab reportedly covers an 
"anchor compression method"
 for token compression in long-sequence attention, reducing effective sequence length presented to the attention mechanism rather than masking the full attention matrix.
`[UNVERIFIED: Zhejiang Lab anchor compression patent — number, jurisdiction, and full claims not independently confirmed in this session]`

- A filing attributed to a Shandong-based entity is referenced under the title "Attention mechanism improvement method for long sequences," but no further verifiable claim detail was retrievable in this session.
`[UNVERIFIED: Shandong entity long-sequence attention patent — title only, no claim text or patent number confirmed]`

**Assessment:** Based on the volume and geographic spread of academic/industrial patent activity referenced by the secondary source (South Korea, China — Zhejiang Lab and Shandong-affiliated filers), the patent landscape for sparse-attention mechanisms appears to be **globally distributed and actively contested**, particularly around (a) mask pre-computation/reuse strategies, (b) token/anchor compression, and (c) combined attention+FFN sparsification. This is consistent with the academic publication trend documented in Section 2, where similar architectural ideas (hierarchical compression, structured masking, row/token clustering) are being independently pursued by US, Chinese, and other Asia-Pacific research groups. However, **no claims in this section should be cited in the final blog article without direct primary-source patent verification**, per the Zero Hallucination Policy.

`[SOURCE: Sparse attention cuts transformer O(n²) complexity | https://www.patsnap.com/resources/blog/articles/sparse-attention-cuts-transformer-on%C2%B2-complexity/ | 2024]`

---

## 4. Future Implications (Fact-Based Speculation)

1. **Convergence of sparsity + extreme quantization + CIM will likely define the next generation of edge NPUs.** The TENET (ternary/LUT), IntAttention (integer pipeline), and CIM/memristor accelerator lines suggest that sparse attention will not be deployed in isolation but *stacked* with sub-8-bit quantization and near-memory compute. A plausible complementary patent-adjacent direction: accelerator designs that dynamically co-select bitwidth *and* sparsity pattern per attention head at runtime, extending SpAtten's adaptive-bitwidth concept to full mixed-precision/mixed-sparsity co-scheduling.
`[SOURCE: SpAtten: Efficient Sparse Attention Architecture with Cascade Token and Head Pruning | https://arxiv.org/pdf/2012.09852 | 2021]`

2. **Predictor-free, fused-stage architectures (PADE-style) are likely to become the dominant edge design pattern**, since edge power budgets cannot tolerate the control overhead of separate prediction/execution stages that datacenter accelerators can amortize. This has direct implications for future ASIC tape-outs targeting smartphone and wearable NPUs.
`[SOURCE: PADE: A Predictor-Free Sparse Attention Accelerator via Unified Execution and Stage Fusion | https://arxiv.org/pdf/2512.14322 | 2026]`

3. **Advanced 3D packaging (hybrid bonding) as seen in H2EAL is a strong candidate to migrate from research accelerators into commercial edge SoCs**, since it directly addresses the memory-compute co-placement problem that pure logic-scaling (Moore's Law) cannot solve for KV-cache-bound long-context workloads. This pattern mirrors trends already visible in HBM-adjacent datacenter accelerators, suggesting a "trickle-down" hardware roadmap from cloud to edge.
`[SOURCE: H2EAL: Hybrid-Bonding Architecture with Hybrid Sparse Attention for Efficient Long-Context LLM Inference | https://arxiv.org/pdf/2508.16653 | 2025]`

4. **In-storage and near-memory attention offload (InstInfer-style) is a speculative but logical fit for edge devices with NVMe/UFS storage** (e.g., laptops, high-end phones), potentially enabling multi-hundred-thousand-token contexts on devices without server-class VRAM, though this remains unproven at true edge power/latency budgets and should be flagged as a research extrapolation rather than a confirmed roadmap.
`[UNVERIFIED: Speculative extension of InstInfer's computational-storage-drive offload concept to consumer edge devices — no direct paper in this research pass validates this specific deployment scenario]`

5. **Structured N:M/X:M pruning is likely to become a first-class citizen in edge NPU instruction sets**, analogous to how NVIDIA's structured sparsity support became a first-class Tensor Core feature — DynaX's dynamic-X generalization suggests future NPU ISAs may need to support *variable-arity* structured sparsity rather than the fixed 2:4 patterns common in current commercial hardware.
`[SOURCE: DynaX: Sparse Attention Acceleration with Dynamic X:M Fine-Grained Structured Pruning | https://dl.acm.org/doi/abs/10.1145/3676641.3715991 | 2025]`

---

## 5. Continuity Hooks (Links to Prior/Future Articles)

- **→ KV-Cache Compression & Quantization (prior/adjacent thread):** AccLLM's W2A8KV4 scheme and the InstInfer storage-offload approach both connect directly to any prior Project Isocline coverage of KV-cache quantization or memory-hierarchy optimization for LLM serving — recommend cross-linking if such an article exists.
- **→ Mobile NPU Architecture Deep-Dives (future thread):** The "Fast On-Device LLM Inference with NPUs" and IntAttention papers set up a natural follow-on article specifically on *commercial* mobile NPU architecture (Qualcomm Hexagon, Apple ANE, MediaTek APU) and how each handles (or fails to handle) attention sparsity — a strong candidate for the next deep-dive in this series.
- **→ Compute-in-Memory (CIM) for AI Accelerators (future thread):** The memristor/CIM reference uncovered here is a narrow slice of a much larger CIM research space; recommend a dedicated future article on CIM accelerators for transformer inference broadly, using this dossier's Section 2.4 as the seed.
- **→ Advanced Packaging & Chiplets (future thread):** H2EAL's hybrid-bonding architecture is a strong entry point into a broader future piece on 3D packaging trends (CoWoS, hybrid bonding, chiplets) as they apply to edge AI silicon, distinct from the datacenter-packaging narrative most coverage currently focuses on.
- **→ DeepSeek Architecture Series (prior/adjacent thread, if exists):** NSA should be cross-referenced with any existing Project Isocline coverage of DeepSeek's MoE/MLA architecture choices, since NSA was designed as a complementary component to DeepSeek's broader efficient-training/inference stack.

---

## 6. Unverified Claims (Consolidated)

The following claims/data points could **not** be verified against primary sources in this research session and must be either independently confirmed or excluded before publication:

1. `[UNVERIFIED: Specific patent number, filing date, assignee legal entity, and full claim scope for the Sogang University attention-mask/convolution-pattern patent]`
2. `[UNVERIFIED: Zhejiang Lab "anchor compression" patent — number, jurisdiction, filing date, claim scope]`
3. `[UNVERIFIED: Shandong-affiliated "Attention mechanism improvement method for long sequences" patent — no number or claims retrieved]`
4. `[UNVERIFIED: Any USPTO-registered patents specifically covering sparse/structured attention accelerator hardware — direct USPTO Full-Text Database and Google Patents search could not be completed due to a tool availability issue during this session; this is a priority follow-up task]`
5. `[UNVERIFIED: Speculative claim that computational-storage-drive attention offload (InstInfer-style) will migrate to consumer edge devices — this is Mimir's forward-looking extrapolation, not a sourced claim from any retrieved paper]`
6. `[UNVERIFIED: Any IETF RFC or Internet-Draft directly addressing sparse-attention accelerator interconnect/networking standards — no relevant RFC was located in this session; it is possible none exists yet, as this appears to be a hardware/algorithm research area rather than a standardized-protocol area]`
7. `[UNVERIFIED: HAL Open Science (hal.science) specific papers — none were surfaced in this session's search results; a targeted follow-up search directly against the HAL repository is recommended]`

---

## Research Session Note for Brian

**Tooling limitation disclosure:** The web search tool became persistently rate-limited partway through this research session, which prevented direct queries against Google Patents, the USPTO Full-Text Database, and HAL Open Science — three of the eight mandated priority sources. The dossier above is built on strong coverage of arXiv (cs.AR/cs.AI) and ACM Digital Library results (11 primary technical papers cited directly, plus supporting survey material), which represent the algorithmic/architectural core of this topic. **I recommend a targeted follow-up research pass focused exclusively on the Patent Landscape section** once tool access is restored, to bring that section up to the same verification standard as the rest of the dossier.