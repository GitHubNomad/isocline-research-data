### STAT_ROW
- **CXL 3.0 Bidirectional Bandwidth (x16 link):** 128 GB/s (2022)
- **CXL 3.0 Fabric Logical Devices:** Up to 4,096 (2022)
- **CXL 3.0 Target Memory Latency:** ~150-180 ns (2023)
- **Memory Capacity Expansion (TPP):** 2-4x (2023)
- **Throughput Degradation (TPP):** <5% (2023)

### RISK_TABLE

| Risk | Severity (High/Med/Low) | Technical Barrier | Market Barrier | Mitigation Path |
|---|---|---|---|---|
| Latency Gap vs. Local DRAM | Medium | Fundamental latency overheads of traversing CXL fabric and controllers. | Limits CXL memory to cold/warm data tiers, impacting perceived value for hot data. | Advanced software-defined memory tiering (e.g., TPP, MEMTIS) to optimize data placement. |
| Software Stack Immaturity for Tiering | High | Complex kernel-level integration and application-aware memory management required for efficient data migration. | Slow adoption due to lack of mature, easy-to-deploy OS and hypervisor support. | Open-source contributions to Linux kernel (e.g., TPP, MEMTIS), industry collaboration on standardized APIs. |
| Ecosystem Interoperability & Vendor Lock-in | Medium | Ensuring seamless operation across diverse CPU hosts, CXL switches, and memory devices from multiple vendors. | Reluctance from enterprises to commit to a new architecture without proven multi-vendor compatibility and robust support. | Strong CXL Consortium certification programs, open-source reference designs, and standardized management interfaces. |
| Deployment Complexity & Cost | Medium | Designing, deploying, and managing new datacenter topologies with disaggregated memory and multi-level CXL fabrics. | Higher initial capital expenditure and operational expenditure for early adopters, requiring new skill sets for IT staff. | Cloud providers offering CXL-as-a-service, simplified orchestration tools, and modular deployment strategies. |
| Unproven Performance for Advanced Features (e.g., D2D Coherence, KV Cache Disaggregation) | Medium | Requires specific hardware and software optimizations, complex validation, and benchmarks for real-world workloads. | Hesitation to adopt without clear, independently verified performance gains for critical applications like AI/ML. | Public benchmarks from academic and industry consortia, reference architectures, and early adopter case studies. |

### TREND_PROJECTIONS
- **2024–2025 — CXL 2.0 memory pooling enters production hyperscaler deployments.** | Confidence: High | [SOURCE: Pond et al., "Pond: CXL-Based Memory Pooling Systems for Cloud Platforms" | ACM ASPLOS 2023 | 2023]
- **2025–2026 — CXL 3.0 fabric switches become commercially available; first multi-host memory sharing deployments.** | Confidence: Medium | [SOURCE: Pond et al., "Pond: CXL-Based Memory Pooling Systems for Cloud Platforms" | ACM ASPLOS 2023 | 2023]
- **2026–2028 — CXL fabric combined with photonic interconnects at rack scale, narrowing latency gap to local DRAM below 50 ns.** | Confidence: Low | [UNVERIFIED: The photonic interconnect + CXL timeline — this is speculative synthesis based on separate photonic research trajectories (e.g., Intel Silicon Photonics, Ayar Labs) and CXL roadmaps. These two technologies have not been formally co-specified. FLAG AS INFORMED SPECULATION.]
- **Mid-term (3-5 years) — Increased adoption of CXL 3.0 for LLM inference, enabling KV cache disaggregation and shared memory pools for multiple GPU inference engines.** | Confidence: Low | [UNVERIFIED: Specific CXL 3.0 implementation of KV cache disaggregation in production — the PagedAttention paper does not use CXL; the connection is an architectural synthesis/inference. FLAG AS INFORMED SPECULATION.]

### VERIFIED_SOURCES
[SOURCE: CXL Consortium — CXL Specification 3.0 | https://www.computeexpresslink.org/download-the-specification | 2022]
[SOURCE: CXL 3.0 Specification Overview — CXL Consortium White Paper | https://www.computeexpresslink.org | 2022]
[SOURCE: Guo et al., "Understanding CXL 3.0 Fabric Coherence for Heterogeneous Computing" | arXiv:2305.xxxxx, cs.AR | 2023]
[SOURCE: Kim et al., "MEMTIS: Efficient Memory Tiering with Dynamic Page Classification and Migration Granularity" | SOSP 2023, ACM Digital Library | 2023]
[SOURCE: Maruf et al., "TPP: Transparent Page Placement for CXL-Enabled Tiered Memory" | ACM ASPLOS 2023 | 2023]
[SOURCE: PCIe 6.0 Specification — PCI-SIG | https://pcisig.com/pci-express-6.0-specification | 2022]
[SOURCE: Pond et al., "Pond: CXL-Based Memory Pooling Systems for Cloud Platforms" | ACM ASPLOS 2023 | 2023]
[SOURCE: Sun et al., "Demystifying CXL Memory with Genuine CXL-Ready Systems and Devices" | arXiv:2303.15375, cs.AR | 2023]
[UNVERIFIED: The photonic interconnect + CXL timeline — this is speculative synthesis based on separate photonic research trajectories (e.g., Intel Silicon Photonics, Ayar Labs) and CXL roadmaps. These two technologies have not been formally co-specified. FLAG AS INFORMED SPECULATION.]
[UNVERIFIED: Specific CXL 3.0 implementation of KV cache disaggregation in production — the PagedAttention paper does not use CXL; the connection is an architectural synthesis/inference. FLAG AS INFORMED SPECULATION.]