# Technical Dossier: Confidential Computing on AI Accelerators — GPU Trusted Execution and the Cost of Encrypted Memory

## Overview

Confidential computing extends the Trusted Execution Environment (TEE) model — established on CPUs through Intel SGX/TDX, AMD SEV-SNP, and Arm CCA — onto GPUs and AI accelerators, so model weights, prompts, activations, and intermediate results remain encrypted and attested even while actively computed on, not just at rest or in transit. NVIDIA's Hopper-generation GPUs were the first mass-market accelerators to ship a native confidential computing mode, and Arm has published a complementary architecture (ACAI) for extending its Confidential Compute Architecture to generic accelerators sharing a coherent interconnect with a CPU.

[SOURCE: ACAI: Protecting Accelerator Execution with Arm Confidential Computing Architecture | https://arxiv.org/pdf/2305.15986 | 2023]

The central engineering tension of the field is that encrypting and integrity-protecting memory traffic between HBM and compute dies introduces cryptographic engines into a path that, on GPUs, must sustain terabyte-per-second bandwidth. Whether this tax is negligible or workload-defining has become the subject of a growing body of 2024–2026 benchmark literature spanning Hopper, Blackwell, and heterogeneous CPU-GPU confidential deployments.

[SOURCE: Confidential Computing on nVIDIA Hopper GPUs: A Performance Benchmark Study | https://arxiv.org/abs/2409.03992 | 2024]
[SOURCE: Benchmarking Confidential Computing Performance on NVIDIA Blackwell GPUs | https://arxiv.org/pdf/2608.26575 | n.d.]
[SOURCE: Confidential Computing on Heterogeneous CPU-GPU Systems: Survey and Future Directions | https://arxiv.org/html/2408.11601v3 | 2024]

## Key Research Findings

### Hopper-generation benchmark baseline
The earliest rigorous third-party benchmark of NVIDIA's shipping confidential computing mode measured H100 GPUs with CC mode enabled against the same hardware with CC disabled, across representative AI workloads, establishing the first public overhead baseline for the technology rather than relying on vendor marketing figures.

[SOURCE: Confidential Computing on nVIDIA Hopper GPUs: A Performance Benchmark Study | https://arxiv.org/html/2409.03992v3 | 2024]
[SOURCE: (PDF) Confidential Computing on nVIDIA H100 GPU: A Performance Benchmark Study | https://www.researchgate.net/publication/383864014_Confidential_Computing_on_nVIDIA_H100_GPU_A_Performance_Benchmark_Study | 2024]

A related independent analysis specifically targeted optimization strategies for H100 confidential mode in AI inference/training pipelines, implying that the default overhead profile is large enough to motivate dedicated tuning work rather than being a rounding error.

[SOURCE: Performance Analysis and Optimization of Nvidia H100 Confidential Computing for AI Workloads | https://www.researchgate.net/publication/389188106_Performance_Analysis_and_Optimization_of_Nvidia_H100_Confidential_Computing_for_AI_Workloads | 2024]

Phala Network, operating a confidential GPU inference marketplace, published its own account of how NVIDIA's confidential computing mode is structured and how it is deployed in a production inference setting (OpenRouter integration), corroborating that the encrypted-mode pathway is being used commercially, not only in lab benchmarks.

[SOURCE: Confidential Computing on Nvidia H100 GPU: A Performance Benchmark Study | https://phala.com/posts/confidential-computing-on-nvidia-h100-gpu-a-performance-benchmark-study | 2024]
[SOURCE: How Nvidia Enable Confidential AI | https://phala.com/learn/How-Nvidia-Enable-Confidential-AI | 2025]
[SOURCE: Secure, Privacy & Verifiable LLMs with GPU TEEs is Alive on OpenRouter! | https://phala.com/posts/GPU-TEEs-is-Alive-on-OpenRouter | 2024]

### Distributed training overhead characterization
A dedicated study characterized GPU TEE overheads specifically in the context of distributed data-parallel ML training — the regime where gradient/activation synchronization across devices compounds any per-device encryption tax, as opposed to single-GPU inference where overhead is more contained.

[SOURCE: Characterization of GPU TEE Overheads in Distributed Data Parallel ML Training | https://arxiv.org/html/2501.11771v1 | 2025]

### Blackwell-generation and next-gen benchmarking
As NVIDIA's Blackwell generation reached deployment, a follow-up benchmark study repeated the confidential-computing performance characterization specifically on Blackwell silicon, suggesting the research community treats each GPU generation's confidential computing implementation as architecturally distinct enough to require independent measurement rather than assuming the Hopper overhead figures carry forward.

[SOURCE: Benchmarking Confidential Computing Performance on NVIDIA Blackwell GPUs | https://arxiv.org/pdf/2608.26575 | n.d.]
[SOURCE: Benchmarking Confidential Computing Performance on NVIDIA Blackwell GPUs · Pith Review | https://pith.science/paper/2608.26575 | n.d.]

A separate, more general performance study of "Confidential Computing GPUs" appears to broaden the measurement scope beyond a single vendor generation.

[SOURCE: Performance of Confidential Computing GPUs | https://arxiv.org/pdf/2505.16501 | 2025]

### Cross-vendor TEE pairing: GPU confidentiality under a non-NVIDIA CPU root of trust
A 2026-dated study benchmarked confidential GPU inference on NVIDIA H100 specifically when the host CPU trust root is Intel TDX rather than AMD SEV-SNP, which is the pairing most of NVIDIA's own reference architecture documentation assumes. This indicates the field is actively testing whether GPU CC mode's performance and security guarantees are portable across different CPU-side TEE implementations, not bound to one vendor's stack.

[SOURCE: Benchmarking Confidential GPU Inference on NVIDIA H100 under Intel TDX | https://arxiv.org/pdf/2607.19353 | n.d.]

### End-to-end and inference-economics framing
"EnclaveX" proposes an end-to-end confidential AI architecture spanning both CPU and GPU TEEs jointly, rather than treating the GPU enclave as an isolated add-on to an otherwise untrusted pipeline — addressing the gap where data is protected on the GPU but exposed during CPU-side preprocessing, tokenization, or postprocessing stages.

[SOURCE: EnclaveX: End-to-End Confidential AI with CPU/GPU TEEs | https://arxiv.org/html/2606.31408v1 | n.d.]

A companion economics-focused study specifically quantifies the performance *and cost* of confidential LLM inference across CPU and GPU TEE configurations, which directly operationalizes the "cost of encrypted memory" framing of this article into dollar and latency terms for a deployable inference service rather than raw microbenchmark numbers.

[SOURCE: Confidential LLM Inference: Performance and Cost Across CPU and GPU TEEs | https://arxiv.org/pdf/2509.18886 | n.d.]

A production-facing explainer from Phala Network frames the same tradeoff for practitioners deploying regulated workloads, describing confidential GPU computing with "encrypted VRAM" as a 2026-relevant deployment pattern for regulated industries.

[SOURCE: Confidential GPU Computing on Cloud: Deploy LLMs with NVIDIA TEE and Encrypted VRAM for Regulated Workloads (2026 Guide) | https://www.spheron.network/blog/confidential-gpu-computing-nvidia-tee-encrypted-vram/ | 2026]

A vendor-neutral documentation source states the performance-overhead framing explicitly as a named concept in confidential AI deployment guidance, treating "performance overhead of confidential computing" as a first-class deployment consideration rather than an edge case.

[SOURCE: Confidential AI - Performance overhead of confidential computing | https://confidential.ai/docs/concepts/performance-overhead | n.d.]

### Adjacent cryptographic-memory architecture work
Pre-dating the native GPU CC wave, "MemShield" explored GPU-assisted software memory encryption as a mechanism to protect memory contents using the GPU as a cryptographic accelerator for a separate trust problem (protecting CPU-side memory), illustrating that the GPU-as-cryptographic-engine and GPU-as-protected-trust-domain problems have historically been studied somewhat independently before converging in current CC-GPU designs.

[SOURCE: MemShield: GPU-assisted software memory encryption | https://arxiv.org/pdf/2004.09252 | 2020]

"BOLT" addresses a closely related bandwidth problem — building a bandwidth-optimized oblivious map structure powered by secure HBM accelerators — which is directly relevant to the encrypted-memory-bandwidth-tax discussion because oblivious access patterns (hiding *which* addresses are touched, not just their contents) impose an additional, typically larger, bandwidth penalty on top of basic confidentiality/integrity protection.

[SOURCE: BOLT: Bandwidth-Optimized Lightning-Fast Oblivious Map powered by Secure HBM Accelerators | https://arxiv.org/pdf/2509.01742 | n.d.]

A related oblivious-access technique, "OTRO," proposes an oblivious tokenization path using square-root ORAM, which is relevant to confidential LLM serving specifically at the tokenization/embedding-lookup stage — a workload pattern (small, data-dependent, high-frequency memory accesses) that is known in the broader ORAM literature to be disproportionately sensitive to memory-protection overhead compared to large sequential tensor operations.

[SOURCE: OTRO: Oblivious Tokenization Path with Square-Root ORAM | https://arxiv.org/pdf/2606.17358 | n.d.]

### Firmware attestation as a prerequisite
"FAARM" addresses firmware attestation and authentication specifically for Mali GPUs, establishing that the attestation-chain problem (proving the accelerator is running known-good, unmodified firmware before trusting it with encrypted data) is being tackled across GPU vendors beyond NVIDIA, including mobile/embedded GPU vendors where confidential computing requirements are emerging from different pressures (on-device inference privacy) than datacenter AI training.

[SOURCE: FAARM: Firmware Attestation and Authentication Framework for Mali GPUs | https://arxiv.org/pdf/2510.22566 | n.d.]

### Confidential orchestration layer
"C8s" proposes a confidential Kubernetes architecture, which is the orchestration-layer counterpart to GPU-level confidential computing — relevant because a confidential GPU enclave deployed inside an untrusted orchestration layer (standard Kubernetes) only closes part of the trust gap; C8s addresses securing the scheduling/orchestration control plane itself.

[SOURCE: C8s: A Confidential Kubernetes Architecture | https://arxiv.org/pdf/2604.26974 | n.d.]

### Broader survey context
A survey of confidential computing on heterogeneous CPU-GPU systems maps out the current design space and open problems across the field, serving as a useful framing document for where single-GPU CC, multi-GPU CC, and CPU-GPU joint attestation each stand relative to one another.

[SOURCE: Confidential Computing on Heterogeneous CPU-GPU Systems: Survey and Future Directions | https://arxiv.org/html/2408.11601v3 | 2024]

A broader survey at the intersection of cryptography/AI more generally touches on confidential computing as one of several mechanisms (alongside zero-knowledge proofs and secure multiparty computation) for trust-minimized AI systems, useful for situating GPU TEEs within the wider landscape of privacy-preserving AI techniques rather than treating it as an isolated mechanism.

[SOURCE: Crypto x AI, AI x Crypto: A Survey | https://arxiv.org/pdf/2606.13892 | n.d.]

### General confidential computing / cloud security grounding
A broader hardware-based confidential computing survey for cloud security provides general grounding for TEE architecture principles (sealed memory regions, remote attestation, hardware root of trust) that the GPU-specific papers above build upon.

[SOURCE: Confidential Computing for Cloud Security: Exploring Hardware based | https://arxiv.org/pdf/2511.04550 | 2025]

## Patent Landscape

No USPTO or Google Patents filings were retrieved or verified in this research pass — the mandatory patent databases listed in the sourcing requirements were not queried successfully this session. The architecture papers above reference concrete implemented mechanisms (NVIDIA's Hopper/Blackwell CC mode, Arm's ACAI extension of its Confidential Compute Architecture to accelerators, Mali GPU firmware attestation) that strongly imply underlying patent filings exist for:

- On-die/in-controller memory encryption engines mediating HBM traffic on NVIDIA Hopper and Blackwell GPUs.
- Arm's accelerator-extension mechanism for Confidential Compute Architecture (CCA), as described architecturally in ACAI.
- Firmware measurement and attestation chains for GPU microcontrollers (parallel to the Mali GPU attestation framework described in FAARM).

`[UNVERIFIED: No specific patent numbers, assignees, or filing dates can be cited — Google Patents and USPTO Full-Text Database were not successfully queried in this research session]`

`[UNVERIFIED: Whether NVIDIA's GPU System Processor (GSP) security role in CC mode is covered by specific issued patents rather than only the architecture descriptions found in the benchmark papers above]`

This section should be treated as incomplete and re-run against Google Patents / USPTO directly before publication, searching assignee names "NVIDIA Corporation," "Arm Limited," and "Advanced Micro Devices" combined with claim terms "memory encryption engine," "accelerator attestation," and "confidential virtual machine GPU passthrough."

## Future Implications

**Multi-GPU and interconnect-level encryption as the next bottleneck.** The distributed data-parallel training overhead study indicates that overhead compounds once multiple GPUs must synchronize under encryption, rather than remaining confined to a single device's memory controller.

[SOURCE: Characterization of GPU TEE Overheads in Distributed Data Parallel ML Training | https://arxiv.org/html/2501.11771v1 | 2025]

A reasonable extrapolation — grounded in this finding but not itself directly stated in any single source reviewed — is that as model-parallel and tensor-parallel execution across many GPUs becomes standard for frontier-scale training, encryption of inter-GPU fabric traffic (not just HBM traffic) will become the dominant overhead term, since fabric links must sustain synchronized throughput across many devices simultaneously rather than one device's local bandwidth budget. `[UNVERIFIED: No source reviewed explicitly measures NVLink/fabric-level encryption overhead as distinct from HBM-level overhead; this is inferential extrapolation from the distributed-training overhead finding above]`

**Oblivious access as a compounding cost layer.** The BOLT and OTRO papers indicate that for workloads requiring access-pattern privacy (not just data-content privacy) — such as private information retrieval over embeddings or tokenization — the additional ORAM-style obliviousness layer imposes bandwidth costs on top of baseline encryption/integrity overhead. This suggests a two-tier future cost model for confidential AI: a baseline encryption tax (now reasonably well characterized by the H100/Blackwell benchmark papers) plus a much steeper, workload-specific obliviousness tax for the subset of operations where access patterns themselves are sensitive.

[SOURCE: BOLT: Bandwidth-Optimized Lightning-Fast Oblivious Map powered by Secure HBM Accelerators | https://arxiv.org/pdf/2509.01742 | n.d.]
[SOURCE: OTRO: Oblivious Tokenization Path with Square-Root ORAM | https://arxiv.org/pdf/2606.17358 | n.d.]

**Cross-vendor TEE portability.** The existence of a dedicated study benchmarking NVIDIA H100 confidential GPU inference specifically under Intel TDX (rather than AMD SEV-SNP) suggests the industry is actively probing whether GPU confidential computing can be decoupled from a single CPU-vendor trust root, which is a prerequisite for confidential computing becoming a portable, cloud-agnostic capability rather than a vendor-specific stack.

[SOURCE: Benchmarking Confidential GPU Inference on NVIDIA H100 under Intel TDX | https://arxiv.org/pdf/2607.19353 | n.d.]

**Orchestration-layer confidentiality as the remaining gap.** Even with GPU-level and CPU-level TEEs solved, the C8s proposal for confidential Kubernetes indicates that the control/scheduling plane around confidential workloads remains a distinct, only partially addressed attack surface — implying that end-to-end confidential AI deployment requires at minimum three cooperating trust layers: CPU TEE, GPU TEE, and orchestration-layer confidentiality.

[SOURCE: C8s: A Confidential Kubernetes Architecture | https://arxiv.org/pdf/2604.26974 | 2026]

**Economic framing will likely drive adoption more than raw overhead percentage.** The dedicated cost-and-performance study of confidential LLM inference suggests the industry conversation is shifting from "how much slower is it" to "what is the dollar cost premium per token/request," which is the framing more likely to determine actual enterprise adoption for regulated industries (healthcare, finance) than raw throughput benchmarks alone.

[SOURCE: Confidential LLM Inference: Performance and Cost Across CPU and GPU TEEs | https://arxiv.org/pdf/2509.18886 | n.d.]

## Continuity Hooks

- **Backward link candidate:** If Project Isocline has previously covered CPU-side confidential computing fundamentals (SGX/TDX/SEV-SNP), this article should open by contrasting CPU TEE memory-bandwidth needs against the terabyte-per-second HBM bandwidth that GPU TEEs must protect — using the Hopper and Blackwell benchmark studies as the concrete before/after data points.

[SOURCE: Confidential Computing on nVIDIA Hopper GPUs: A Performance Benchmark Study | https://arxiv.org/abs/2409.03992 | 2024]
[SOURCE: Benchmarking Confidential Computing Performance on NVIDIA Blackwell GPUs | https://arxiv.org/pdf/2608.26575 | n.d.]

- **Forward link candidate — multi-tenant cloud GPU isolation.** A natural follow-up piece covers MIG (Multi-Instance GPU) partitioning combined with confidential computing for cloud GPU rental to mutually distrusting tenants, extending the cross-vendor TDX/SEV-SNP portability thread raised by the TDX benchmark study.

[SOURCE: Benchmarking Confidential GPU Inference on NVIDIA H100 under Intel TDX | https://arxiv.org/pdf/2607.19353 | n.d.]

- **Forward link candidate — oblivious memory access for embeddings/retrieval.** The BOLT and OTRO papers point toward a dedicated future article on oblivious RAM techniques for private retrieval-augmented generation and embedding search, since this represents a cost layer beyond basic confidential computing that current blog coverage has not yet addressed.

[SOURCE: BOLT: Bandwidth-Optimized Lightning-Fast Oblivious Map powered by Secure HBM Accelerators | https://arxiv.org/pdf/2509.01742 | n.d.]
[SOURCE: OTRO: Oblivious Tokenization Path with Square-Root ORAM | https://arxiv.org/pdf/2606.17358 | n.d.]

- **Forward link candidate — confidential orchestration.** A follow-up on confidential Kubernetes architecture (C8s) would close the loop on end-to-end confidential AI deployment, moving from chip-level to datacenter-orchestration-level trust.

[SOURCE: C8s: A Confidential Kubernetes Architecture | https://arxiv.org/pdf/2604.26974 | 2026]

`[UNVERIFIED: Specific titles of prior Project Isocline articles — no blog catalog or orchestrator context was provided alongside this task, so these hooks are structural recommendations based on logical topic adjacency, not confirmed existing cross-references]`

## Unverified Claims

1. `[UNVERIFIED: Exact quantitative overhead percentages (e.g., "X% slower") for NVIDIA H100/Blackwell confidential computing mode — the benchmark papers above establish that such measurements exist and were published, but the specific numeric results were not extracted from full paper text in this pass and should be pulled directly from the cited arXiv PDFs before publication]`
2. `[UNVERIFIED: AMD's specific mechanism and shipping status for extending SEV-SNP protections to Instinct MI300-series accelerators — no AMD-specific GPU TEE paper or document was found in this research pass]`
3. `[UNVERIFIED: Any specific patent numbers or assignee filings covering GPU memory encryption engines, CPU-GPU attestation chains, or confidential MIG partitioning — USPTO and Google Patents were not successfully queried]`
4. `[UNVERIFIED: Any IETF RATS (Remote ATtestation ProcedureS) working group draft explicitly addressing heterogeneous CPU+GPU attestation chains — no IETF document was retrieved in this research pass]`
5. `[UNVERIFIED: NVLink/NVSwitch fabric-level encryption overhead as distinct from HBM-controller-level overhead — inferred as a likely future bottleneck but not directly measured in any source reviewed]`
6. `[UNVERIFIED: Specific role of the NVIDIA GPU System Processor (GSP) in mediating confidential computing mode — referenced in general industry knowledge of Hopper architecture but not confirmed against a specific source in this research pass]`
7. `[UNVERIFIED: Whether Google TPUs implement an analogous confidential computing / encrypted-memory mode — no TPU-specific confidential computing source was found in this research pass]`
8. `[UNVERIFIED: Any public cloud provider (AWS/Azure/GCP) technical documentation detailing their specific confidential-GPU-instance attestation and key-management flow — not retrieved in this pass]`

**Recommended next step:** Before publication, pull full text (not just abstracts) of the two core benchmark papers — the Hopper study (arXiv:2409.03992) and the Blackwell study (arXiv:2608.26575) — to extract the specific percentage-overhead figures and workload breakdowns needed to make the "cost of encrypted memory" section quantitatively precise rather than qualitative. A direct USPTO/Google Patents query for NVIDIA and Arm assignee filings on accelerator memory encryption should also be run to populate the Patent Landscape section with real filing numbers.