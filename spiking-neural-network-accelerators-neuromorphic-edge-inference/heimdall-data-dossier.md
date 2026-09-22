### STAT_ROW

- **Intel Loihi 2 capacity per chip:** up to 1 million neurons, 120 million synapses, 128 neuromorphic cores (2021)
- **Intel Hala Point system:** 1,152 Loihi 2 chips, 1.15 billion neurons, 2,600 W maximum (2024)
- **BrainChip Akida (AKD1000) keyword recognition:** <1 ms inference, <2 ms on-chip learning, <250 mW (2025)
- **OpenSpike open-source SNN ASIC:** 48,262 images/s at 56.8 GOPS/W, SkyWater 130 nm (2023)
- **Bishop spiking-transformer accelerator:** 5.91× speedup, 6.11× energy efficiency vs. prior SNN accelerators (2025)

### RISK_TABLE

- **Static-data efficiency gap** — Severity: High · Technical barrier: ANNs currently process static data more efficiently than SNNs · Market barrier: most published SNN benchmarks use static datasets, weakening buyer comparisons · Mitigation: target event-camera, audio, and biosignal workloads; hybrid SNN/ANN designs [SOURCE: To Spike or Not To Spike: A Digital Hardware Perspective on Deep Learning Acceleration | arXiv:2306.15749 | 2023]
- **Research-chip availability** — Severity: Med · Technical barrier: Loihi 2 is a research processor, not a commercial product · Market barrier: product teams cannot design in a chip they cannot buy · Mitigation: commercially sold neuromorphic parts such as Akida AKD1000 [SOURCE: Intel Advances Neuromorphic with Loihi 2, New Lava Software Framework and New Partners | https://www.intc.com/news-events/press-releases/detail/1502/intel-advances-neuromorphic-with-loihi-2-new-lava-software | 2021]
- **Mature-node silicon** — Severity: Med · Technical barrier: the benchmarked Akida v1.0 is built on TSMC 28 nm · Market barrier: competes against edge NPUs on newer nodes · Mitigation: architectural sparsity gains rather than process gains [SOURCE: Enabling Efficient Processing of Spiking Neural Networks with On-Chip Learning on Commodity Neuromorphic Processors for Edge AI Systems | arXiv:2504.00957 | 2025]
- **Self-reported benchmarks** — Severity: Med · Technical barrier: few third-party head-to-head measurements against edge AI accelerators · Market barrier: efficiency claims are hard to verify during procurement · Mitigation: hardware-probe comparisons on identical tasks [SOURCE: Realtime Facial Expression Recognition: Neuromorphic Hardware vs. Edge AI Accelerators | arXiv:2403.08792 | 2024]

### TREND_PROJECTIONS

- **2025–2028 — Hybrid CNN-SNN accelerators convert only energy-intensive layers to spiking form** | Confidence: Medium | [SOURCE: A Power-Efficient Reconfigurable Hybrid CNN-SNN Accelerator for High Performance AI Applications | IEEE Xplore | 2025]
- **2025–2028 — Spiking-transformer accelerators move from FPGA prototypes toward language-model workloads** | Confidence: Low | [SOURCE: Neuromorphic Principles for Efficient Large Language Models on Intel Loihi 2 | arXiv:2503.18002 | 2025]

*Note: no Tier 1 or Tier 2 market-size data for neuromorphic silicon was located. [UNVERIFIED: no Tier 1 or 2 source located for neuromorphic chip market size]*

### VERIFIED_SOURCES

[SOURCE: Intel Advances Neuromorphic with Loihi 2, New Lava Software Framework and New Partners | https://www.intc.com/news-events/press-releases/detail/1502/intel-advances-neuromorphic-with-loihi-2-new-lava-software | 2021]
[SOURCE: Intel Builds World's Largest Neuromorphic System to Enable More Sustainable AI | https://www.intc.com/news-events/press-releases/detail/1691/intel-builds-worlds-largest-neuromorphic-system-to | 2024]
[SOURCE: Enabling Efficient Processing of Spiking Neural Networks with On-Chip Learning on Commodity Neuromorphic Processors for Edge AI Systems | arXiv:2504.00957 | 2025]
[SOURCE: OpenSpike: An OpenRAM SNN Accelerator | arXiv:2302.01015 | 2023]
[SOURCE: Bishop: Sparsified Bundling Spiking Transformers on Heterogeneous Cores with Error-Constrained Pruning | arXiv:2505.12281 | 2025]
[SOURCE: To Spike or Not To Spike: A Digital Hardware Perspective on Deep Learning Acceleration | arXiv:2306.15749 | 2023]
[SOURCE: Realtime Facial Expression Recognition: Neuromorphic Hardware vs. Edge AI Accelerators | arXiv:2403.08792 | 2024]
