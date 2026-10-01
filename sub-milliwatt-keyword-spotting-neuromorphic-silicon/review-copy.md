# REVIEW COPY — Sub-Milliwatt Keyword Spotting: Event-Driven Neuromorphic Silicon for Always-On Edge Sensing

## SEO Metadata
- Slug: sub-milliwatt-keyword-spotting-neuromorphic-silicon
- Meta Description: Explore how sub-milliwatt neuromorphic silicon enables always-on keyword spotting at the edge, cutting idle power to nanowatts while preserving accuracy.
- Focus Keyword: sub-milliwatt keyword spotting neuromorphic
---
## Monetization Notes
Affiliate links are contextual and low-friction; no upfront ad spend required. Potential revenue from qualifying purchases and sponsorships.
---

# Sub-Milliwatt Keyword Spotting: Event-Driven Neuromorphic Silicon for Always-On Edge Sensing

## At a Glance

- **The workload:** Always-on keyword spotting listens around the clock for a wake word. On a battery device, the power drawn while *nothing* is being said determines battery life more than peak throughput does.
- **Six years of one benchmark:** On the same "Aloha" keyword-spotting task, measured dynamic power fell from 22,860 mW on a GPU and 2,340 mW on an Nvidia Jetson to 81 mW on Intel's Loihi and 291 µW on SynSense's Xylo Audio 2 — about 6.6 µJ per inference.
- **Nanowatt silicon exists:** A 65 nm spiking classifier idles at 75 nW and draws 220 nW at full keyword-spotting activity, hitting 91.8% on four Google Speech Commands keywords and 95.8% on "Hey Snips."
- **The catch:** Those numbers describe the classifier, not the system. The Aloha results exclude audio preprocessing, and the nanowatt chip assumes an external analog front end. The measurement boundary is now the main story.
- **Spiking isn't the only way under a milliwatt:** Syntiant's non-spiking NDP120 wakes Google Assistant under 280 µW. Innatera's Pulsar, a shipping neuromorphic microcontroller, pairs a spiking fabric with CNN and FFT accelerators and a RISC-V core at as low as 400 µW for audio scene classification.

---

Our last piece, **"Spiking Neural Network Accelerators and the Neuromorphic Edge Inference Revival,"** surveyed spiking hardware broadly and noted that its advantage is conditional — strongest where the input is sparse and event-driven. Keyword spotting is the cleanest test of that condition. Audio arrives continuously, the target word is rare, and the device cannot afford to be awake in the conventional sense. This article follows that one workload down to the nanowatt level and examines where the reported numbers stop describing the device you would actually build.

## Why Idle Power Is the Metric That Matters

A smart speaker, earbud, or wearable running keyword spotting spends almost all of its life hearing nothing it needs to act on. Peak inference speed is nearly irrelevant. What matters is the power floor — what the chip burns while listening to silence, background noise, or conversation that does not contain the keyword.

That's why power reporting in this field splits into several numbers that aren't interchangeable:

- **Idle power:** the chip powered and configured, with no input.
- **Active power:** total power while processing input.
- **Dynamic power:** active minus idle — the extra cost of doing the work.
- **Energy per inference:** power divided by inference rate, reported against either dynamic or active power.

Dynamic energy per inference is the headline number in most neuromorphic benchmarks. It's also the one that flatters hardware with a high floor, because it subtracts that floor out. For an always-on device, the floor is the bill.

## The Aloha Benchmark: One Task Across Six Years of Hardware

In 2018, Peter Blouw, Xuan Choo, Eric Hunsberger, and Chris Eliasmith of Applied Brain Research built a two-layer keyword-spotting network with the Nengo DL toolkit and measured it on [Nvidia Jetson TX1 Module](https://www.amazon.com/s?k=Nvidia%20Jetson%20TX1&tag=isocline-20) and a Movidius Neural Compute Stick. Loihi beat every alternative on energy per inference at equivalent accuracy, and its lead grew as networks got larger. The dataset — built around the phrase "aloha" — became a recurring benchmark for neuromorphic keyword spotting.

In 2020, Yexin Yan and colleagues from TU Dresden, Applied Brain Research, and the University of Manchester ran keyword spotting on a SpiNNaker 2 prototype and compared it with Loihi. SpiNNaker 2's multiply-accumulate (MAC) array, normally a feature for conventional rate-based networks, paid off in the spiking context. The comparison split cleanly: Loihi was more efficient when the vector-matrix multiplication was simple, and SpiNNaker 2 when it was high-dimensional. The right chip depends on the network's shape, not just the label "neuromorphic."

In 2024, Hannah Bos and Dylan Muir of SynSense ran the same benchmark on the Xylo Audio 2 (SYNS61210) and collected the prior measurements into one table. All power figures were measured on physical devices:

| Hardware | Idle (mW) | Active (mW) | Dynamic (mW) | Dynamic energy (mJ/inf) | Active energy (mJ/inf) |
|---|---|---|---|---|---|
| GPU | 14,970 | 37,830 | 22,860 | 29.67 | 49.1 |
| CPU | 17,010 | 28,480 | 11,470 | 6.32 | 15.7 |
| Nvidia Jetson | 2,640 | 4,980 | 2,340 | 5.58 | 11.9 |
| Movidius NCS | 210 | 647 | 437 | 1.5 | 2.2 |
| Loihi (Blouw et al.) | 29 | 110 | 81 | 0.27 | 0.37 |
| Loihi (Yan et al.) | 29 | 40 | 11 | 0.037 | 0.13 |
| SpiNNaker 2 prototype | — | — | 7.1 | 0.0071 | — |
| Xylo Audio 2 | 0.216 | 0.507 | 0.291 | 0.0066 | 0.011 |

*Source: Bos & Muir, SynSense (2024), Table 2. Active power for SpiNNaker 2 wasn't reported in the benchmark paper; it's reported elsewhere as 390 mW.*

Read down the dynamic-energy column and the story is a steady march from tens of millijoules to single-digit microjoules. Xylo Audio 2 posts the lowest figure in every column, and the authors make a point worth taking seriously: active energy per inference, which includes the idle floor, is the more realistic system-level metric. On that measure Xylo leads the next-best device by an order of magnitude.

The SpiNNaker 2 row shows why the choice of metric matters. Its dynamic energy per inference (0.0071 mJ) is nearly identical to Xylo's (0.0066 mJ). But its active power, reported elsewhere at 390 mW, is roughly 770 times Xylo's 0.507 mW. The dynamic numbers make the two look like peers. For a device that listens all day, they aren't.

Xylo's own numbers make the same point from the other direction. Across every model the SynSense team trained, idle power was 216–217 µW against active power of 468–514 µW. More than 40% of the chip's working draw is spent before any audio arrives.

## Inside Xylo Audio 2

Xylo is a synchronous digital CMOS processor built from integer logic. It simulates leaky integrate-and-fire (LIF) spiking networks with 16-bit neuron and synapse state and 8-bit weights. SynSense designed it for real-time streaming — processing audio as it arrives — rather than for accelerated batch inference.

The more interesting design choice is the front end. Xylo Audio includes an audio encoder meant to connect directly to a microphone:

1. A low-noise amplifier with selectable 0, 6, or 12 dB gain.
2. A bank of 16 second-order Butterworth band-pass filters, center frequencies from 40 Hz to 16,940 Hz, each with a Q of 4.
3. Rectification of each filter output.
4. A LIF neuron per band that smooths the signal and converts it to events.

The result is 16 sparse event channels whose firing rate tracks the energy in each frequency band — a streaming, buffer-free alternative to a spectrogram.

On the Aloha task, the deployed quantized network reached 95% accuracy, above the benchmark standard of 93%, and lost less than 2% to quantization. It ran more than four times faster than real time with the master clock at 6.25 MHz, and the headline model drew 291 µW of dynamic power at 6.6 µJ per inference.

An earlier SynSense paper by the same authors showed the toolchain side: ambient audio classification on Xylo at 98% accuracy, 100 ms latency, and under 100 µW of inference power. It used a pyramid of synaptic time constants to pick up features at several temporal scales and went through the open-source Rockpool framework, which is designed to let ML engineers without a spiking-network background train and deploy these models.

## The Measurement Boundary Problem

Here's the caveat the Xylo authors state themselves: their benchmark numbers, and the numbers for every other device in the table, **exclude audio preprocessing**. Most Aloha implementations compute a mel-frequency cepstral coefficient (MFCC) spectrogram first, and the authors note that this can be computationally demanding. The table compares classifiers, not listening devices.

SynSense measured its own on-chip encoder separately, at under 50 µW. That's small, but it isn't negligible next to a core drawing a few hundred microwatts. It's also the only front-end power figure in the comparison.

The boundary gets sharper at the low end.

### A classifier that idles at 75 nanowatts

In 2020, Dewei Wang and colleagues presented a spiking classifier at the IEEE Asian Solid-State Circuits Conference, with a journal extension in *Frontiers in Neuroscience* in 2021. The chip is fabricated in 65 nm CMOS, occupies 1.99 mm², and runs at 0.52 V. Its network is five layers: 256-128-128-128-5 neurons.

The power figures are striking:

- **75 nW** with no input activity
- **220 nW** at full activity on keyword-spotting workloads
- Power that **scales linearly with input rate**, so a quiet room costs close to the idle floor

The low floor comes from event-driven circuit design all the way down. Clocks are generated by spikes and gated finely, and power is gated finely too, so circuitry that isn't handling an event is left unclocked or powered down.

Accuracy is respectable for the size: 91.8% on four Google Speech Commands keywords ("yes," "stop," "right," "off") plus filler, and 95.8% on the single "Hey Snips" wake phrase plus filler.

But the chip has no on-chip feature extraction. It assumes an external analog front end that produces spike-rate-coded audio features — 16 channels at 6-bit precision in 80 ms frames. The nanowatt figure is real, and it describes the classifier, not a microphone-to-decision system.

So the lowest-power core in this story leaves the front end off-chip, while the chip with the best system-level comparison measured its front end at tens of microwatts. Once the classifier falls into nanowatts, it stops being the most expensive part of the chain. The front end — amplifier, filtering, and spike encoding — becomes the part that sets the power floor.

### Cutting out the front end

That's why the sensor-to-spike boundary is getting research attention. Sidi Yaya Arnaud Yarga and Sean Wood proposed in 2024 connecting a pulse-density-modulation (PDM) MEMS microphone — the digital microphone type common in modern devices — directly to a spiking network. A PDM microphone already emits a dense one-bit pulse stream, so the intermediate conversion stages can be dropped, which substantially cuts computation. Their network reached 91.54% on Google Speech Commands and showed high sparsity.

They haven't reported a hardware power figure. Their result is an argument about where power can be removed, not a measurement of how much.

## Learning Without the Cloud

Keyword spotting is also a natural target for on-device learning, because wake words, accents, and acoustic environments vary from user to user.

**ReckOn**, from Charlotte Frenkel and Giacomo Indiveri, was presented at ISSCC 2022 and was the first spiking neuromorphic chip at that conference. It's a 0.45 mm² spiking recurrent network processor in 28 nm FDSOI that learns online over second-long timescales using a modified form of the e-prop algorithm. It was demonstrated on navigation, gesture recognition, and keyword spotting with 0.8% memory overhead and a training power budget under 150 µW. The design is open source.

At the commercial end, BrainChip's Akida AKD1000 — covered in our previous piece — has been measured at under 1 ms for keyword recognition inference and under 2 ms for on-chip learning, within a processing budget under 250 mW. That's a much higher power ceiling than Xylo or ReckOn, but it's a chip you can buy today with learning built in.

**Speculation:** A training budget under 150 µW is in the same power class as the always-on inference figures above. That points toward wake-word systems that adapt to a specific user or room on the device itself, without sending audio anywhere. None of the sources here show that in a shipping product.

## The Competition Isn't Only Spiking

It would be misleading to present neuromorphic silicon as the only route under a milliwatt.

Syntiant's **NDP120** is a deep-learning processor built on the Syntiant Core 2 architecture, which the company describes as at-memory compute running conventional CNNs, RNNs, and fully connected networks — not spiking networks. It activates Google Assistant on "Hey Google" or "Ok Google" at under 280 µW, and it runs echo cancellation, beamforming, noise suppression, speaker identification, and multiple wake words on the same chip. In the MLPerf Tiny v0.7 keyword-spotting benchmark in 2022, it posted 1.8 ms and 49.59 µJ per inference at 1.1 V and 98 MHz, and 4.3 ms and 35.29 µJ at 0.9 V and 30 MHz. Syntiant reported that as about 10 times faster and about 17 times more energy efficient than any other submission.

MLPerf Tiny and Aloha are different benchmarks with different networks, so NDP120's microjoules and Xylo's cannot be compared directly. What the numbers do show is that a well-designed non-spiking audio processor is already in the sub-milliwatt, tens-of-microjoules regime — and it already supports a major assistant's hotword.

Innatera's **Pulsar**, announced in May 2025 as available to developers, reads as the market's answer to that competition. It's a microcontroller, not a standalone accelerator: a spiking neural network fabric alongside a RISC-V CPU, a CNN accelerator, and an FFT accelerator. Innatera quotes power as low as 400 µW for audio scene classification and 600 µW for radar presence detection. It claims up to 100 times lower latency and 500 times lower energy than conventional AI processors but doesn't name a baseline for those figures. Development runs through Innatera's PyTorch-based Talamo SDK.

The FFT and CNN blocks are the telling part. Pulsar doesn't bet that every stage of the audio chain should be spiking. It puts the spiking fabric where event-driven processing pays off and keeps conventional accelerators for the rest — the same hybrid pattern our previous article found in research accelerators.

## What This Means for Anyone Building an Always-On Audio Product

- **Ask which number you're being shown.** Dynamic energy per inference hides the idle floor. For a device that listens all day, idle and active power are the figures that map to battery life.
- **Ask where the measurement boundary is.** Aloha results exclude preprocessing. The lowest-power spiking classifier leaves the analog front end off-chip. A core figure in nanowatts can sit inside a system that draws tens or hundreds of microwatts.
- **Treat the front end as a first-class design problem.** On-chip spike encoders like Xylo's and direct PDM-to-spike coupling are where the next reductions are likely to come from.
- **Don't assume spiking wins by default.** Non-spiking at-memory processors already wake assistants under 280 µW. The spiking case is strongest on idle floor, streaming operation, and low-power on-chip learning.

**Speculation:** The shipping direction is heterogeneous — a spiking block for event-driven work next to FFT, CNN, and CPU blocks on one microcontroller — rather than a pure spiking audio chip. The benchmark direction needs to follow, from classifier-only energy toward microphone-to-decision power. Until someone publishes that comparison across Xylo, NDP120, and Pulsar on the same task, claims of a single power winner in always-on audio should be read with the measurement boundary in mind.

---

*This article extends our coverage of neuromorphic hardware from "Neuromorphic Chips and the Future of Edge AI" through "Spiking Neural Network Accelerators and the Neuromorphic Edge Inference Revival." The same front-end question applies to vision, where event-based sensors play the role that spike encoders play here. That's the subject of a planned follow-up on event-based vision chips and the compiler toolchains that map spiking networks onto them.*

---

**Stay in the loop.**
Project Isocline publishes deep-dive technical analysis on AI infrastructure, energy systems, and the engineering decisions shaping the next decade of compute. No noise. No fluff.

[Subscribe to the newsletter →](https://isocline.kit.com)

*You can unsubscribe at any time.*