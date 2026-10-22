# Technical Dossier: Prefill-Decode Disaggregation — The Network Energy of Moving KV Caches Between Nodes

**Project Isocline | Research Lead: Mimir**

---

## Research Note on Methodology (Transparency Disclosure)

Before the findings: this session's external search tool reached its usage cap after two successful queries, and all subsequent attempts (including the patent-landscape searches against Google Patents/USPTO and the energy-per-bit network literature) returned no results. In keeping with the Zero Hallucination Policy, this dossier treats only the two retrieved sources as verified and cited. Every other substantive technical claim common in the disaggregated-inference discourse — specific throughput numbers, named systems like Mooncake or Splitwise, RDMA energy-per-bit figures, and all patent numbers — is explicitly marked `[UNVERIFIED]` rather than stated as fact. This dossier should be treated as a **partial pass** requiring a follow-up research cycle with restored search access before publication. Do not let the downstream writer state any `[UNVERIFIED]` item as settled fact.

---

## Overview

Prefill-decode (PD) disaggregation is an LLM serving architecture that splits inference into two physically separate worker pools instead of running both phases on the same GPU. The rationale is rooted in a hardware-resource mismatch between the two phases of autoregressive generation.

LLM inference is commonly decomposed into two phases with distinct computational characteristics: a Prefill phase that processes the entire input sequence to generate the first output token, and a Decode phase that generates subsequent tokens auto-regressively. The Prefill phase can be fully parallelized using matrix-matrix multiplication operators, making it compute-bound, and during this process per-token key and value vectors are produced and stored as the key-value (KV) cache for reuse in later steps. In contrast, the Decode phase is inherently sequential at the token level due to its auto-regressive nature, and its frequent accesses to the KV cache make it memory-bound.

[SOURCE: Prefill/Decode-Aware Evaluation of LLM Inference on Emerging AI Accelerators | https://arxiv.org/pdf/2606.17104 | 2025]

Because these two phases stress entirely different parts of the accelerator (FLOPs vs. memory bandwidth), co-locating them on one GPU forces a scheduling compromise: batching decode requests alongside a prefill request causes head-of-line blocking and unpredictable latency, while running them in strict alternation wastes compute. The disaggregation approach resolves this by giving each phase its own hardware pool.

Recent serving systems exploit this phase asymmetry through Prefill/Decode disaggregation; Zhong et al. formalize this approach in DistServe by separating Prefill and Decode workers that scale independently.

[SOURCE: Prefill/Decode-Aware Evaluation of LLM Inference on Emerging AI Accelerators | https://arxiv.org/pdf/2606.17104 | 2025]

The architectural consequence of this split is the specific subject of this dossier: once prefill finishes on Node A, the KV cache it produced — potentially gigabytes of tensor state for a long-context request — must be transferred to Node B before decode can begin. That transfer crosses a physical network fabric (NVLink, InfiniBand, RoCE, or plain Ethernet depending on deployment tier), and every byte moved has a joule cost and a latency cost that did not exist in a co-located (aggregated) serving design. This is the "network energy" trade that PD disaggregation implicitly accepts in exchange for independent scaling of compute-bound and memory-bound resources.

`[UNVERIFIED: Specific figures for KV cache size per token per layer (e.g., bytes = 2 × num_layers × num_kv_heads × head_dim × precision_bytes), and resulting total cache sizes for long-context requests on specific model families, could not be confirmed against a primary source this session.]`

`[UNVERIFIED: Named systems commonly associated with this problem space — including Mooncake (Moonshot AI), Splitwise (Microsoft), NVIDIA Dynamo/TensorRT-LLM disaggregated serving, and vLLM's disaggregated prefill implementation — are widely discussed in the field, but their specific architectural claims, benchmark numbers, and publication details could not be re-verified against primary sources in this session due to search tool exhaustion. They should not be cited as fact until confirmed in a follow-up pass.]`

---

## Key Research Findings

### 1. The phase-asymmetry premise is the field's load-bearing assumption

Every disaggregation architecture — and therefore every claim about the network energy cost of moving KV caches — rests on the same premise verified above: the Prefill phase is compute-bound while the Decode phase's frequent accesses to the KV cache make it memory-bound [SOURCE: Prefill/Decode-Aware Evaluation of LLM Inference on Emerging AI Accelerators | https://arxiv.org/pdf/2606.17104 | 2025]. This is the reason the industry has converged on splitting the two phases onto heterogeneous or independently-scaled hardware pools rather than treating the transfer cost as a reason to avoid disaggregation altogether — the throughput and SLO gains from independent scaling are evidently judged to outweigh the added network hop in the systems that adopt this pattern.

### 2. A body of follow-on work is optimizing resource allocation *given* disaggregation, not reverting from it

The existence of a dedicated line of research on compute allocation *within* the disaggregated paradigm — rather than papers arguing to abandon it — is itself informative. One paper identified in this research cycle addresses exactly this follow-on problem:

[SOURCE: SLO-Aware Compute Resource Allocation for Prefill-Decode Disaggregated LLM Inference | https://arxiv.org/pdf/2603.04716 | 2025]

Its reference list independently confirms two adjacent and relevant threads of work: a chunked-prefill scheduling technique designed to reduce the cost of interleaving phases on shared hardware, and ongoing research into disaggregated serving across heterogeneous ("multi-vendor") GPU fleets — a detail with direct relevance to network energy, since cross-vendor deployments are less likely to share a homogeneous high-bandwidth fabric like NVLink. Specifically, the bibliography lists Agrawal et al.'s Sarathi work on chunked prefills [SOURCE: Sarathi: Efficient LLM inference by piggybacking decodes with chunked prefills (arXiv:2308.16369) | referenced in arXiv:2603.04716 bibliography | 2023] and a 2025 paper on disaggregated prefill/decode inference spanning multi-vendor GPUs [SOURCE: Disaggregated prefill and decoding inference system for large language model serving on multi-vendor gpus (arXiv:2509.17542) | referenced in arXiv:2603.04716 bibliography | 2025].

`[UNVERIFIED: I was not able to open or directly verify the content of arXiv:2308.16369 or arXiv:2509.17542 in this session — they are cited here only as titles/IDs appearing in another paper's confirmed bibliography, not as independently fetched and read sources. Their specific claims about chunked-prefill savings or multi-vendor GPU interconnect behavior must be verified directly before being used as factual support in the article.]`

### 3. The network-energy mechanism itself (physical/engineering reasoning, not yet source-verified this session)

The following mechanism is the standard systems-engineering explanation for why PD disaggregation has a network energy cost, but because I could not complete corroborating searches this session, it is presented as reasoned inference rather than cited fact:

- KV cache transfer between a prefill node and a decode node must move tensor data proportional to sequence length × number of layers × number of attention heads × head dimension × element precision, for every request that crosses the disaggregation boundary.
- The energy cost of this transfer is a function of the physical fabric used: on-package/NVLink-class interconnects have fundamentally lower joules-per-bit than switched Ethernet/RDMA over longer cable runs, because signal integrity and SerDes power scale with reach.
- Energy spent moving the KV cache is "pure overhead" relative to an aggregated (non-disaggregated) design — it performs no useful FLOPs, it exists only to relocate state, and it sits directly on the critical path of time-to-first-decode-token.

`[UNVERIFIED: All bandwidth, joules-per-bit, and latency figures for NVLink, InfiniBand/RoCEv2, and PCIe that would normally ground this section in hard numbers are withheld pending verification — I will not state vendor spec-sheet numbers from memory without a confirmed primary source this session.]`

---

## Patent Landscape

`[UNVERIFIED: Google Patents and USPTO Full-Text Database searches for "KV cache transfer," "disaggregated inference," "prefill decode separation," and adjacent claim language could not be executed this session due to search tool exhaustion. No patent numbers, assignees, or claim scopes can be reported as verified fact. This entire section requires a dedicated follow-up research pass before the article's patent-landscape section is drafted. Known industry players likely to hold relevant filings — major GPU/accelerator vendors, hyperscale cloud providers running LLM inference fleets, and LLM-serving infrastructure startups — are a reasonable starting point for that follow-up, but no specific patent should be attributed to any of them without direct verification.]`

---

## Future Implications

The following are framed explicitly as reasoned speculation per the Fact-Based Speculation mandate, built on the one solidly verified architectural fact (phase asymmetry drives disaggregation) rather than on unverified numeric claims:

1. **Topology-aware schedulers as the next layer.** If moving KV cache between nodes carries a real latency and energy cost — a direct consequence of the architectural split confirmed in DistServe's separation of Prefill and Decode workers that scale independently [SOURCE: Prefill/Decode-Aware Evaluation of LLM Inference on Emerging AI Accelerators | https://arxiv.org/pdf/2606.17104 | 2025] — then the logical next optimization target is a scheduler that places prefill and decode workers for a given request as close together on the network fabric as load permits, trading some scheduling flexibility for reduced transfer energy. This is a natural complement to the SLO-aware allocation research already underway for the compute side of the problem.

2. **Multi-vendor heterogeneity will make the energy question harder, not easier.** The confirmed existence of research specifically targeting disaggregated prefill/decode serving across multi-vendor GPU fleets suggests the field anticipates deployments where prefill and decode nodes may not share a common high-speed interconnect generation or vendor stack, which would push more KV cache traffic onto general-purpose data-center Ethernet rather than purpose-built accelerator fabrics — raising, not lowering, the per-byte energy cost relative to a single-vendor NVLink-class deployment. `[UNVERIFIED: the quantitative magnitude of this gap pending direct access to arXiv:2509.17542.]`

3. **Compression and cache-reduction techniques as an orthogonal mitigation.** Chunked-prefill scheduling, referenced in the confirmed bibliography of the SLO-aware allocation paper, represents one existing strategy for reducing the cost of phase interleaving; a parallel and complementary research direction would be KV cache compression or quantization applied specifically at the transfer boundary (rather than for memory savings alone), since a smaller cache is cheaper to move regardless of which fabric carries it. `[UNVERIFIED: I have not confirmed a specific paper proposing compression explicitly motivated by inter-node transfer energy; this is speculative extrapolation, not a cited existing result.]`

---

## Continuity Hooks

- **Backward link candidate:** Any prior Project Isocline article covering autoregressive decoding mechanics, KV cache fundamentals, or GPU memory-bandwidth bottlenecks in LLM serving would be the natural prerequisite piece — the compute-bound/memory-bound split between prefill and decode [SOURCE: Prefill/Decode-Aware Evaluation of LLM Inference on Emerging AI Accelerators | https://arxiv.org/pdf/2606.17104 | 2025] is the foundational concept this entire article builds on, and should be assumed-known or linked rather than re-derived.
- **Forward link candidate:** The SLO-aware compute allocation research identified here [SOURCE: SLO-Aware Compute Resource Allocation for Prefill-Decode Disaggregated LLM Inference | https://arxiv.org/pdf/2603.04716 | 2025] opens a direct sequel topic — "Scheduling Across the Disaggregation Boundary: SLO-Aware Resource Allocation for Prefill and Decode Pools" — as does the multi-vendor GPU disaggregation thread (arXiv:2509.17542) for a piece on heterogeneous-fabric inference economics.
- **Adjacent topic flag:** A dedicated article on RDMA/RoCEv2 fabric energy characteristics (grounded in IEEE Xplore / IETF RFC sources) would materially strengthen this piece's weakest section (network-energy mechanism) and is recommended as a prerequisite or companion deep-dive before this article's numeric claims are finalized.

---

## Unverified Claims (Consolidated)

All items below require a follow-up research session with restored search tool access before this dossier can support publication-grade numeric or attributional claims:

1. `[UNVERIFIED: Exact byte-size formulas and representative KV cache sizes for long-context requests across specific model families.]`
2. `[UNVERIFIED: Existence, architecture, and specific claims of Mooncake, Splitwise, NVIDIA Dynamo/TensorRT-LLM disaggregated serving, and vLLM disaggregated prefill — these are well-known in the field from training data but were not re-confirmed via primary source this session.]`
3. `[UNVERIFIED: Content and findings of Sarathi (arXiv:2308.16369) and the multi-vendor GPU disaggregation paper (arXiv:2509.17542) — titles only confirmed via a third paper's bibliography, not independently fetched.]`
4. `[UNVERIFIED: Any bandwidth, latency, or joules-per-bit figures for NVLink, InfiniBand/RoCEv2, PCIe, or Ethernet fabrics as applied to KV cache transfer.]`
5. `[UNVERIFIED: All patent landscape claims — no Google Patents or USPTO search could be executed this session.]`
6. `[UNVERIFIED: Any IETF RFC numbers for RDMA transport protocols (e.g., RoCEv2, iWARP) relevant to KV cache transfer implementations.]`
7. `[UNVERIFIED: Full content and authorship detail behind the truncated "Cunchen Hu, Heya..." bibliography entry in arXiv:2603.04716 — likely relevant prior work, but identity and claims unconfirmed.]`

**Recommendation to Hestia orchestrator:** Re-queue this topic for a second research pass with search tool access restored before handing to the writing stage. The architectural core (Section: Overview, Finding 1) is solid and citation-complete; the energy-quantification and patent sections are not yet publication-ready.