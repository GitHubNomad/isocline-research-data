I'll research this topic thoroughly, prioritizing the mandatory niche sources. Let me start with parallel searches across arXiv, patents, and IEEE for the core technical components.
Let me retry with better error handling.
Empty results, let's check what's happening with the raw response.
Excellent, good data. Now let me dig deeper into specific arXiv papers, patents, and IEEE sources.
It seems I've hit a rate limit on search tool usage. Let me pause and wait before retrying.
Still limited. Let me wait longer.
The rate limit seems persistent within this code_execution tool. Let me try using the web_search directly outside code_execution to see if that channel is separate.
Let me wait significantly longer since this appears to be a hard quota rather than a short-term throttle.
The search tool quota has been exceeded for this session and isn't recovering with short waits. I have already gathered substantial high-quality data from my initial successful search batch (10 results covering arXiv papers, patents context, and industry chips). Let me proceed to build the dossier using this rich data, and I'll clearly flag where additional verification would be needed rather than making further claims unsupported by what I've already retrieved.
The search quota exceeded is proving persistent. I'll wait one more extended interval and try a minimal single query — if it still fails, I will proceed to compile the dossier from the data already gathered, clearly flagging areas needing further verification (particularly the patent landscape, which I could not independently confirm this session).
The quota clearly isn't recovering within this session window. I'll proceed to compile the dossier using the substantive data already gathered from the successful search batch, which included several directly relevant arXiv papers and industry sources. I will be transparent about the patent landscape gap and flag it explicitly as requiring dedicated Google Patents/USPTO verification in a follow-up pass, per the zero-hallucination policy.
I have solid material from arXiv and adjacent industry sources. Let me attempt one more focused search batch for the patent landscape specifically, since that's a mandatory dossier section — trying now after a longer cool-down.
The search quota is exhausted for this session and not recovering. I will now compile the dossier using the verified data gathered, explicitly flagging the patent landscape as needing dedicated follow-up verification since I could not access Google Patents/USPTO directly this session — consistent with the Zero Hallucination Policy.
I've compiled the dossier. Before finalizing, I want to flag an important transparency issue and ask a clarifying question rather than assume.

## Summary & Important Flag

I've delivered the dossier below, but I need to be upfront about a research limitation: **the web search tool hit a rate limit partway through this session** and did not recover despite multiple extended wait periods (up to 5 minutes). This means I was **not able to complete dedicated Google Patents / USPTO searches** for this topic — a mandatory source category you specified. I did not fabricate patent numbers to compensate; instead I flagged this explicitly in Section 3 and in the Unverified Claims list, per the Zero-Hallucination Policy.

What I *did* successfully retrieve (from one productive search batch before the limit hit) gave solid arXiv-sourced technical grounding: benchmarking studies (Loihi vs. CPU/GPU/Jetson), a SpiNNaker-2 vs. Loihi comparison, a 2024 paper on direct PDM-microphone-to-SNN co-design, and a documented **300 nanowatt** always-on KWS chip — which is a genuinely useful finding since it's ~1000x below the "sub-milliwatt" framing in your topic title.

**Clarifying questions before this goes further into the pipeline:**
1. Do you want me to attempt the patent-landscape research again in a fresh session (the limit may reset), or should the pipeline route that specific sub-task elsewhere?
2. Can you supply the actual text/abstract of "Neuromorphic Chips and the Future of Edge AI" (2026-05-09)? I inferred the continuity hooks from topic conventions rather than the source article itself, since I don't have direct access to your vector memory content — I'd rather verify than assume.

---

# Technical Dossier: Sub-Milliwatt Keyword Spotting — Event-Driven Neuromorphic Silicon for Always-On Edge Sensing

## 1. Overview

Always-on keyword spotting (KWS) is one of the clearest commercial proving grounds for neuromorphic silicon, because the task runs continuously, making idle-state power — not peak throughput — the dominant cost metric. This is the regime where event-driven, spike-based computation has a structural advantage over clocked digital signal processing.

The core insight is sparsity exploitation: <cite index="1-3">the event-driven operation of neuromorphic chips is well-suited to processing spiking activity, and their energy efficiency addresses battery and heat constraints</cite> in embedded contexts. More specifically, <cite index="2-3,2-4">Intel's Loihi 2 has demonstrated pattern recognition tasks using less than one milliwatt, compared to hundreds of milliwatts or watts for equivalent GPU-based inference, an efficiency gain that comes from the event-driven, sparse activation model</cite>, and <cite index="2-2">when a neuromorphic system processes a static scene with no changes, almost no energy is consumed, since only new, relevant events trigger computation</cite>.

For audio specifically, <cite index="1-6">neuromorphic cochlea chips convert sound into spike trains that mirror the encoding performed by the human inner ear, enabling keyword spotting, speaker identification, and environmental sound classification at a fraction of the energy required by conventional digital signal processing</cite>.

A concrete silicon benchmark: <cite index="4-2">"Always-On, Sub-300-nW, Event-Driven Spiking Neural Network based on Spike-Driven Clock-Generation and Clock- and Power-Gating for an Ultra-Low-Power Intelligent Device" (Dewei Wang et al., 2020) proposes a synchronous architecture operating at Near Threshold Voltage, using clock gating and power gating to minimize idle power consumption to 300nW, targeted at keyword spotting and prototyped on a 65nm CMOS process as an inference-only design</cite>.
[SOURCE: Always-On, Sub-300-nW, Event-Driven Spiking Neural Network based on Spike-Driven Clock-Generation and Clock- and Power-Gating for an Ultra-Low-Power Intelligent Device (Dewei Wang et al.) | https://open-neuromorphic.org/blog/digital-neuromorphic-hardware-read-list/ | 2020]

This 300 nW idle figure should anchor the article: it is roughly three orders of magnitude below the "sub-milliwatt" framing in the topic title, indicating the true frontier of always-on KWS silicon has moved into the **nanowatt** regime for idle listening, reserving milliwatt-scale budgets for active spike-processing bursts.

---

## 2. Key Research Findings

### 2.1 Benchmarking neuromorphic KWS against conventional compute
<cite index="5-1,5-2">Using Intel's Loihi neuromorphic research chip and ABR's Nengo Deep Learning toolkit, researchers analyzed inference speed, dynamic power consumption, and energy cost per inference of a two-layer neural network keyword spotter, comparing against a CPU, GPU, Nvidia's Jetson TX1, and the Movidius Neural Compute Stick, finding that Loihi outperforms all of these alternatives on an energy-cost-per-inference basis while maintaining equivalent inference accuracy</cite>. Notably, <cite index="5-3">an analysis of tradeoffs between network size, inference speed, and energy cost indicates that Loihi's comparative advantage over other low-power computing devices improves for larger networks</cite> — relevant to whether neuromorphic KWS stays competitive as vocabularies scale beyond single wake-words.
[SOURCE: Benchmarking Keyword Spotting Efficiency on Neuromorphic Hardware (Blouw, Choo, Hunsberger, Eliasmith) | https://arxiv.org/pdf/1812.01739 | 2018]

### 2.2 Second-generation many-core comparison (SpiNNaker 2 vs. Loihi)
<cite index="6-1">Researchers implemented two neural-network-based benchmark tasks on a prototype chip of the second-generation SpiNNaker (SpiNNaker 2) system: keyword spotting and adaptive robotic control</cite>. <cite index="6-2">The study highlights the benefit of a multiply-accumulate (MAC) array in the SpiNNaker 2 prototype, ordinarily used in rate-based machine learning networks, when employed in a neuromorphic spiking context</cite>, finding that <cite index="6-3">while Loihi shows better efficiency when less complicated vector-matrix multiplication is involved, the SpiNNaker 2 prototype shows better efficiency when high-dimensional vector-matrix multiplication is involved</cite>. This establishes that chip selection for KWS is workload-dependent, not universal.
[SOURCE: Low-Power Low-Latency Keyword Spotting and Adaptive Control with a SpiNNaker 2 Prototype and Comparison with Loihi (Yan et al.) | https://arxiv.org/pdf/2009.08921 | 2020]

### 2.3 Sensor-to-spike co-design: eliminating the analog front-end bottleneck
<cite index="7-1,7-2">The KWS task requires low-energy devices for continuous processing; neuromorphic devices address this energy challenge, but the general neuromorphic KWS pipeline from microphone to SNN entails multiple processing stages</cite>. A 2024 paper proposes a fix: <cite index="7-2,7-3">leveraging the popularity of Pulse Density Modulation (PDM) microphones in modern devices and their similarity to spiking neurons, researchers proposed a direct microphone-to-SNN connection that eliminates intermediate stages, notably reducing computational costs</cite>. <cite index="7-4">The system achieved 91.54% accuracy on the Google Speech Command dataset, surpassing the state-of-the-art for the Spiking Speech Command dataset</cite>, and <cite index="7-5">the observed sparsity in network activity and connectivity indicates potential for remarkably low energy consumption in a neuromorphic device implementation</cite>.

This is a key differentiator finding: the power bottleneck in sub-milliwatt KWS is shifting from the SNN compute core toward the analog-to-spike conversion stage.
[SOURCE: Neuromorphic Keyword Spotting with Pulse Density Modulation MEMS Microphones (Yarga, Wood) | https://arxiv.org/pdf/2408.05156 | 2024]

### 2.4 On-chip learning and commercial deployment (BrainChip Akida)
<cite index="8-1,8-2">The BrainChip Akida AKD1000 ships as a standard PCIe peripheral and supports on-chip STDP-based learning alongside inference, differing from research platforms like Intel Loihi and IBM TrueNorth; it has been applied to single-device keyword spotting and visual classification, though no multi-device or federated deployments have been reported</cite>. More broadly, <cite index="8-3">spiking neural networks communicate via discrete spikes rather than continuous activations, enabling event-driven computation at milliwatt-scale power</cite>.
[SOURCE: Federated Few-Shot Learning on Neuromorphic Hardware: An Empirical Study Across Physical Edge Nodes | https://arxiv.org/html/2603.13037v1 | 2026]

### 2.5 Historical lineage: TrueNorth's mixed sync/async design
<cite index="4-3">TrueNorth is a fully digital chip embedding 4096 cores with 1 million neurons and 256 million synapses, adopting a mixed design methodology in which local computational cores are synchronous while the interconnecting infrastructure is asynchronous (event-driven)</cite>, with <cite index="4-4">each core using time-multiplexing to compute neuron states and minimize area, at 256 neurons per core</cite>. Today's nanowatt-class KWS chips are direct architectural descendants of this event-driven interconnect philosophy, scaled down from 65mW research silicon to sub-µW always-on sensing.
[SOURCE: TrueNorth: Design and Tool Flow of a 65 mW 1 Million Neuron Programmable Neurosynaptic Chip (Akopyan et al.) | https://open-neuromorphic.org/blog/digital-neuromorphic-hardware-read-list/ | 2015]

### 2.6 Adjacent bio-signal validation
<cite index="3-1">Researchers built ultra-low-power SNN-based classifiers that can detect epileptic seizures from intracranial EEG signals in real time, running on neuromorphic hardware with sub-milliwatt power consumption — a crucial step toward implantable seizure suppression devices</cite>. This validates that the sub-milliwatt event-driven paradigm generalizes beyond audio, and is a strong bridge to future implantable-biosensing content.
[SOURCE: Neuromorphic Computing 2025: Current SotA landscape review | https://humanunsupervised.com/papers/neuromorphic_landscape.html | 2025]

---

## 3. Patent Landscape

**⚠️ FLAGGED GAP:** Direct Google Patents / USPTO queries could not be completed this session due to a search-tool rate limit that did not clear after multiple extended retries. No patent numbers or claim language are fabricated below.

- The Dewei Wang et al. (2020) 300nW design is a strong prior-art candidate for a follow-up patent search, describing a specific patentable mechanism (spike-driven clock generation + clock/power gating at Near-Threshold Voltage). [SOURCE: same as above | 2020]
- BrainChip's Akida AKD1000, as a shipped commercial product with on-chip STDP learning, implies an active patent portfolio, though specific numbers are unverified this session.

**[UNVERIFIED: Specific patent numbers/assignees/claims for event-driven KWS silicon — requires dedicated Google Patents/USPTO pass.]**
**[UNVERIFIED: Patent status of Innatera T1 or Syntiant NDP-series PDM-to-spike front ends.]**

Recommended next step: a dedicated patent-only pass targeting BrainChip, Intel, Syntiant, Innatera, and Applied Brain Research under classification codes G10L15/00 and G06N3/063.

---

## 4. Future Implications

1. **Bottleneck migration to sensing:** <cite index="7-3">Eliminating intermediate stages between microphone and SNN notably reduces computational costs</cite>, suggesting future gains will come from monolithic transducer/spike-encoder co-design rather than smarter neuron models alone.
2. **Workload-dependent chip selection will professionalize**, per the Loihi/SpiNNaker-2 divergence in Section 2.2 — a candidate topic for a future "selection matrix" article.
3. **Cross-domain transfer to implantable biosensing:** given <cite index="3-1">sub-milliwatt SNN seizure classifiers already exist for intracranial EEG</cite>, KWS power-gating techniques are plausible candidates for adaptation into implantable wake-up circuits.
4. **On-chip learning narrows cloud dependency**, per <cite index="8-1">Akida's on-chip STDP learning alongside inference</cite>, supporting local wake-word personalization without cloud round-trips.

**[UNVERIFIED: Any specific vendor roadmap commitment to ship sub-100nW KWS silicon by a stated date.]**

---

## 5. Continuity Hooks (to "Neuromorphic Chips and the Future of Edge AI," 2026-05-09)

- **Extends:** This piece narrows a landscape-level prior article into one killer application (KWS), going deep on nanowatt-to-milliwatt power engineering the prior piece likely couldn't cover in depth.
- **Challenges:** If the prior article framed "sub-milliwatt" as the efficiency frontier, this dossier's 300nW finding suggests an update: the idle-state frontier has moved ~1000x lower.
- **Complements:** <cite index="3-2">The pattern of embedding a neuromorphic chip to handle always-on tasks like keyword spotting, waking the main processor only when necessary, is analogous to how some phones use dedicated DSPs for low-power audio processing</cite> — directly substantiating a likely theme of the prior article.
- **Sets up future piece:** The implantable-biosensing angle (seizure detection via SNN) is a natural next article extending edge-AI into bio-tech.

**Note:** These hooks are inferred from topic conventions, not verified against the actual prior article text — please supply it for precise mapping.

---

## 6. Unverified Claims (Consolidated)

1. Patent numbers/assignees/claims for event-driven KWS ASICs (BrainChip, Intel, Syntiant, Innatera, ABR).
2. Vendor roadmap commitments for sub-100nW KWS silicon shipping dates.
3. Detailed architectural/patent specifics of Innatera T1 and Syntiant NDP-series beyond general field-guide characterizations.
4. Exact content of the prior article "Neuromorphic Chips and the Future of Edge AI" — continuity hooks are inferential pending source text.