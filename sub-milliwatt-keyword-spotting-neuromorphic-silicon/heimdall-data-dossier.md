### STAT_ROW

- **Xylo Audio 2 Aloha KWS dynamic power:** 291 µW, 6.6 µJ/inference (2024)
- **Nvidia Jetson on the same benchmark:** 2,340 mW dynamic, 5.58 mJ/inference (2018 measurement, tabulated 2024)
- **65 nm SNN classifier idle power:** 75 nW; 220 nW at full KWS activity (2021)
- **Syntiant NDP120 MLPerf Tiny KWS:** 35.29 µJ/inference at 4.3 ms (2022)
- **Innatera Pulsar audio scene classification:** as low as 400 µW (2025)

### RISK_TABLE

- **Front-end power excluded from benchmarks** — Severity: High · Technical barrier: published Aloha results omit audio preprocessing; MFCC computation can be demanding · Market barrier: system battery life can't be read from core numbers · Mitigation: on-chip spike encoders (<50 µW on Xylo Audio 2) [SOURCE: Micro-power spoken keyword spotting on Xylo Audio 2 | https://www.synsense.ai/wp-content/uploads/2024/08/Micro-power-spoken-keyword-spotting-on-XyloA2.pdf | 2024]
- **External analog front end required** — Severity: High · Technical barrier: nanowatt SNN classifier assumes off-chip spike-rate feature generation · Market barrier: integrators must source or design the AFE · Mitigation: direct PDM-microphone-to-SNN coupling [SOURCE: Always-On Sub-Microwatt Spiking Neural Network Based on Spike-Driven Clock- and Power-Gating for an Ultra-Low-Power Intelligent Device | Frontiers in Neuroscience | 2021]
- **Non-comparable metrics** — Severity: Med · Technical barrier: idle, active, dynamic power and per-inference energy are reported inconsistently · Market barrier: vendor claims (e.g., "500× lower energy") lack common baselines · Mitigation: report active energy per inference [SOURCE: Micro-power spoken keyword spotting on Xylo Audio 2 | https://www.synsense.ai/wp-content/uploads/2024/08/Micro-power-spoken-keyword-spotting-on-XyloA2.pdf | 2024]
- **Strong non-spiking competition** — Severity: Med · Technical barrier: none for incumbents; at-memory DNN processors already run sub-mW wake words · Market barrier: NDP120 runs "Hey Google" under 280 µW · Mitigation: SNN niches with lower idle power or on-chip learning [SOURCE: Edge AI Chip Company Syntiant Announces Support for 'Hey Google' and 'Ok Google' Hotwords | https://www.syntiant.com/press-release/edge-ai-chip-company-syntiant-announces-support-for-hey-google-and-ok-google-hotwords/ | 2021]

### TREND_PROJECTIONS

- **2025–2028 — Spiking cores ship as blocks inside heterogeneous MCUs (SNN + CNN + FFT + RISC-V)** | Confidence: Medium | [SOURCE: Innatera unveils Pulsar | https://innatera.com/press-releases/innatera-unveils-pulsar-the-worlds-first-mass-market-neuromorphic-microcontroller-for-the-sensor-edge | 2025]
- **2025–2030 — On-device wake-word adaptation within ~150 µW training budgets** | Confidence: Low | [SOURCE: ReckOn | arXiv:2208.09759 | 2022]

### VERIFIED_SOURCES

[SOURCE: Benchmarking Keyword Spotting Efficiency on Neuromorphic Hardware | arXiv:1812.01739 | 2018]
[SOURCE: Low-Power Low-Latency Keyword Spotting and Adaptive Control with a SpiNNaker 2 Prototype and Comparison with Loihi | arXiv:2009.08921 | 2020]
[SOURCE: Micro-power spoken keyword spotting on Xylo Audio 2 | https://www.synsense.ai/wp-content/uploads/2024/08/Micro-power-spoken-keyword-spotting-on-XyloA2.pdf | 2024]
[SOURCE: Always-On Sub-Microwatt Spiking Neural Network Based on Spike-Driven Clock- and Power-Gating for an Ultra-Low-Power Intelligent Device | Frontiers in Neuroscience, PMC8329666 | 2021]
[SOURCE: ReckOn | arXiv:2208.09759 | 2022]
[SOURCE: Syntiant NDP120 MLPerf Tiny v0.7 | https://www.syntiant.com/blog/syntiant-ndp120-achieves-outstanding-results-in-latest-mlperf-tiny-v07-benchmark-suite/ | 2022]
[SOURCE: Innatera Pulsar | https://innatera.com/press-releases/innatera-unveils-pulsar-the-worlds-first-mass-market-neuromorphic-microcontroller-for-the-sensor-edge | 2025]
