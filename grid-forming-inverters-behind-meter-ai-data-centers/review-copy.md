# REVIEW COPY — Grid-Forming Inverters and the Behind-the-Meter AI Data Center

## SEO Metadata
- Slug: grid-forming-inverters-behind-meter-ai-data-centers
- Meta Description: How grid-forming inverters stabilize behind-the-meter AI data centers. Covers NERC Level 3 alerts, battery microgrid control, and grid-following vs grid-forming architecture.
- Focus Keyword: grid-forming inverter behind-the-meter data center
---
## Monetization Notes
No programmatic spend recommended. Tier 2 sponsorships should be pitched as native 'brought to you by' modules aligned to the NERC alert and microgrid case study sections. Affiliate links target senior engineers specifying GFM inverters and studying for NERC compliance.
---

---
title: "Grid-Forming Inverters for Behind-the-Meter AI Data Centers: NERC Standards and Microgrid Control"
slug: "grid-forming-inverters-behind-meter-ai-data-centers"
metaDescription: "How grid-forming inverters stabilize behind-the-meter AI data centers. Covers NERC Level 3 alerts, battery microgrid control, and grid-following vs grid-forming architecture."
focusKeyword: "grid-forming inverter behind-the-meter data center"
secondaryKeywords: ["nerc level 3 alert computational load", "grid-forming battery microgrid", "grid-following vs grid-forming inverter", "ai data center power oscillation"]
suggestedInternalLinks: ["/grid-problem-ai-data-centers-power-infrastructure", "/480v-hvdc-rack-power-distribution-ai-datacenters"]
---
# Grid-Forming Inverters and the Behind-the-Meter AI Data Center

## At a Glance

- **The pressure:** At the end of 2025, about 1,312 GW of U.S. generation and storage projects were waiting in interconnection queues. AI campuses that can't wait are building power behind the meter — solar, batteries, and on-site generation that they own and control.
- **The grid's side of the story:** On July 10, 2024, a series of faults on a single 230 kV line caused about 1,500 MW of data center load across 60 connection points to transfer to backup power at once. In May 2026, NERC issued a Level 3 alert telling grid planners to model computational loads separately, down to UPS settings and on-site batteries.
- **The control question:** Most inverters today are *grid-following* — they synchronize to a voltage someone else provides. A *grid-forming* inverter holds its own voltage magnitude and angle steady through a disturbance, the way a synchronous generator does, and can serve load with no generator in the system at all.
- **Batteries are the easy case:** NERC calls battery storage "low-hanging fruit" for grid-forming control. The energy buffer is already on the dc side, so the upgrade is essentially software, and NERC recommends commissioning every new bulk-system battery grid-forming-capable.
- **How much is enough:** There's no universal number. Studies NERC cites put the needed grid-forming share around 30% of inverter capacity in an inertia-free system, 37% in a European study, and 11–23.5% on one island depending on what the other inverters can do.

---

Our earlier coverage — **"The Grid Problem: Why AI Data Centers Are Forcing a Rethink of Power Infrastructure"** and **"480V and HVDC Rack Power Distribution for Gigawatt-Class AI Datacenters"** — followed power from the utility to the rack. This piece looks at what happens when a campus supplies part or all of that power itself. Once a data center owns its generation and storage, it also owns the control problem utilities have been working through for years: keeping voltage and frequency stable on a system run largely by power electronics.

## Why Campuses Are Going Behind the Meter

Lawrence Berkeley National Laboratory's *Queued Up* report tracks projects waiting for permission to connect to the U.S. transmission system. At the end of 2024 about 2,290 GW were active in those queues — nearly twice the capacity of the entire existing U.S. power plant fleet. By the end of 2025 the total had fallen to about 1,312 GW across roughly 8,200 projects. Within that, natural gas rose 86% to 253 GW, while solar (773 GW), storage (749 GW), and wind (220 GW) each fell by 16–19%.

Those numbers describe generators waiting to connect. A large new load faces its own interconnection studies on top of that. For an AI campus with hardware arriving on a fixed schedule, generating and storing power on site is one way around the wait.

One operating example: in June 2025, Redwood Materials and Crusoe brought up a microgrid at Redwood's campus in Sparks, Nevada. It pairs solar with 63 MWh of repurposed electric-vehicle battery packs, delivering 12 MW to Crusoe's modular Spark data centers. It was built in under four months. Redwood reports 99.2% operational availability, and in 2026 the companies announced an expansion from 4 to 24 Spark units, roughly seven times the original compute. Redwood names the mismatch between accelerating power demand and slow grid interconnection as the driver. Redwood's materials don't say whether the site's inverters run in grid-forming mode.

But going behind the meter doesn't take a campus off the grid's radar. Most stay connected, and that connection is where the next problem shows up.

## What 1,500 Megawatts Looks Like

NERC's incident review "Considering Simultaneous Voltage-Sensitive Load Reductions" describes what happened in the Eastern Interconnection on July 10, 2024.

At about 7:00 p.m. Eastern, a lightning arrester failed on a 230 kV transmission line, creating a permanent fault. Over 82 seconds the line saw six sequential faults, each lasting 42–66 ms and each depressing voltage in the area to 0.25–0.40 per unit.

Data center protection systems saw those dips and did what they were designed to do: they moved their facilities to backup power. About **1,500 MW** of load, all associated with data centers, dropped from the grid across 60 connection points and 25 substations.

Losing load sounds harmless compared with losing a generator. It isn't. With 1,500 MW of demand suddenly gone, supply exceeded consumption. Frequency climbed to 60.047 Hz and took about four minutes to settle, and voltage rose to 1.07 per unit, forcing operators to act to bring it back down. NERC noted that the grid had not historically seen simultaneous load losses of this size in response to a fault, and that operators hadn't anticipated it.

Each facility behaved correctly on its own. The problem came from 60 of them behaving the same way at the same moment.

## NERC's Level 3 Alert

On May 4, 2026, NERC issued a Level 3 alert — its highest level, an "Essential Action" — on computational load. It went to transmission planners, planning coordinators, transmission owners, balancing authorities, reliability coordinators, and transmission operators, with acknowledgment due May 11 and reporting due August 3, 2026.

The alert followed an earlier Level 2 alert. NERC found in the responses that entities "generally did not have sufficient processes, procedures, or methods" to handle the risks from computational loads, which NERC defines to include AI training, cryptocurrency mining, and traditional data centers. NERC is also working to register a new "Computational Load Entity" category, currently loads of 20 MW or more connected at 60 kV with more than 1 MW of IT load. Draft registry criteria and a Standard Authorization Request were posted April 1, 2026.

The Essential Actions read like a specification for how the grid wants to see a data center:

- **Model it as its own kind of load.** Use the PERC1 (Power Electronic Reconnecting and Ceasing) model or better, and separate IT load from cooling and other non-IT load.
- **Collect the dynamic details.** That includes expected ramp rates in MW per minute, protection settings, the voltage and timing at which the facility reconnects, and uninterruptible power supply (UPS) configuration.
- **Include on-site generation.** Model when the facility transfers to it, and fully represent any generation or battery storage that will run in parallel with the grid.
- **Record what the facility is for.** Traditional compute, AI training, inference, or crypto mining, with percentages when mixed.
- **Test it at commissioning.** For example, switch from a UPS "eco" mode to supply through on-site battery storage.
- **Ride through where possible.** Encourage facility-level ride-through up to equipment limits.
- **Install fault recording.** Capture the facility's electrical behavior during disturbances.

The alert isn't a mandatory reliability standard, and NERC states that failing to implement it carries no penalty. But its direction is clear. The batteries, UPS systems, and transfer logic inside a data center are now part of how grid planners model the bulk power system.

## A Second Stress: AI Training Oscillations

Sudden disconnection is one risk. Periodic fluctuation is another.

In a 2025 study, Min-Seung Ko and Hao Zhu profiled the power draw of large-scale AI workloads. They found periodic, stochastic power fluctuations during training and fine-tuning, tied to how GPU systems are architected. Modeling those fluctuations on the WECC 179-bus and NPCC 140-bus test systems, they found that narrower fluctuation frequency bands, larger single-site capacity, or dispersed siting can intensify wide-area power system oscillations.

A training cluster's power swings aren't only a facility-level concern. At scale, they can reach the grid as a forcing signal.

## Grid-Following and Grid-Forming

Everything so far describes power electronics interacting with a grid. The control mode of those power electronics is where the fix lies.

Solar farms, batteries, and most other inverter-based resources today are **grid-following**. They measure the voltage an existing grid provides, synchronize to it, and inject current accordingly. NERC notes that modern grid-following equipment can already provide real services: regulation, primary and fast frequency response, and dynamic voltage support. But it depends on something else setting the voltage waveform.

A **grid-forming** inverter sets the waveform itself. NERC's September 2023 white paper on grid-forming specifications gives the core behavior most definitions agree on: grid-forming controls "tend to hold their voltage magnitude and angle at the device terminals constant in the period immediately following a system event." That resistance to sudden change is the stabilizing property rotating generators provide by physics, reproduced in control software.

### How NERC proposes to define it

The white paper considers four ways to specify grid-forming behavior:

1. **By control topology**, such as virtual synchronous machine or droop-based control. NERC recommends *against* this, to leave room for innovation.
2. **By quantitative response metrics**, such as active and reactive power rise time after a network event.
3. **By frequency-domain characterization**, measuring the device's response to perturbations across a range of frequencies.
4. **By a general testing definition**, which NERC recommends.

The recommended tests are demanding on purpose. A grid-forming device, like a synchronous generator, should be able to serve load with no other synchronous machines in service, stay in synchronism with other machines, and keep operating stably both when those machines disconnect and when they reconnect. Grid-following inverters generally can't do all of those things.

The first test is, in effect, the islanded microgrid. A behind-the-meter campus that separates from the utility with only inverter-based sources — solar and batteries — needs at least one source that can create voltage and frequency on its own. That's the grid-forming role.

The DOE-supported UNIFI Consortium is working on the same problem from the equipment side. Its *Specifications for Grid-Forming Inverter-Based Resources, Version 2*, published through NREL in March 2024, sets system-level and inverter-level requirements meant to let grid-forming equipment from different vendors operate together at any scale, alongside rotating machines and other inverters. A third version has since been published.

### What grid-forming adds

Per NERC, grid-forming control enables capabilities beyond grid-following:

- Operation in low system-strength conditions
- Improved overall system stability
- Help arresting frequency change after a large generator trips
- Potentially, black start — restarting a de-energized system from inverter-based resources

### Why batteries come first

NERC singles out battery energy storage as "particularly low-hanging fruit." A battery already has the energy buffer on its dc side, so enabling grid-forming behavior is "purely software-based," avoiding more expensive hardware changes or the curtailment other resources might need to hold reserve. New batteries can be built grid-forming-capable at a relatively low incremental controller and hardware cost.

Retrofitting an existing grid-following battery may also be only a controls change. But on a bulk-system plant it counts as a material modification that requires new studies. NERC's recommendation follows from that: commission all new bulk-system batteries with grid-forming capability, and enable it once the responsible entity has studied it.

The technology is already in service. NERC's 2023 list of grid-forming battery projects runs from a 13 MW system on Kauai in 2018 and Germany's 15 MW Bordesholm project in 2019 to Hawaii's 185 MW Kapolei Energy Storage in 2023 and the 300 MW Blackhillock project in Great Britain in 2024, and, in Australia, the 150 MW Hornsdale Power Reserve. Tests on Maui's system, NERC reports, found that with no rotating inertia, about 30% of inverter capacity needed to be grid-forming.

## How Much Grid-Forming Is Enough

NERC is direct that there's no rule of thumb. The needed share is system-specific and has to be studied. The references it cites give a sense of scale:

- **Maui (HECO):** Power-hardware-in-the-loop tests found that as rotating inertia fell toward zero, the needed grid-forming share rose roughly linearly, to about **30%** of inverter capacity with no rotating inertia. The amount of rotating machinery online predicted the need better than the share of inverter-based generation did, which was above 95% in every case studied.
- **EU MIGRATE project:** At least **37%** [Grid-Forming Inverter Control Textbook](https://www.amazon.com/s?k=grid-forming%20inverter%20control%20book&tag=isocline-20)s as a share of all inverter-based resources.
- **An island network study:** **11%** grid-forming was enough if the other inverters provided frequency and voltage support per IEEE 2800-2022, but **23.5%** was needed if grid-following inverters offered no such support.

The last result matters for data center microgrids. How much grid-forming capacity a system needs depends on what everything else in it does, including the loads.

## What This Means for an AI Campus

**Speculation:** The behind-the-meter campus and the NERC alert describe the same physical problem from two sides. A training cluster's power swings, which Ko and Zhu showed can excite oscillations across a regional grid, land first on whatever sources supply the campus. If those sources are grid-following, something else has to hold voltage and frequency steady. If a grid-forming battery sits between the GPUs and the rest of the system, it can absorb and smooth those swings before they propagate, whether "the rest of the system" is the utility or on-site gas turbines.

**Speculation:** The July 2024 event points to a matching role at the grid connection. The load loss happened because many facilities made the same protective decision at once. A campus that can ride through a disturbance on its own grid-forming storage has less reason to disconnect abruptly. That's the ride-through behavior NERC's alert encourages, provided with equipment the campus may already need for islanded operation.

**Speculation:** Owning generation doesn't exempt a campus from grid planning. NERC's Essential Actions ask for UPS modes, battery storage operated in parallel with the grid, transfer logic, and ramp rates. A behind-the-meter microgrid that stays connected is exactly what that request describes. The campuses that treat grid-forming control, ride-through, and published models as design requirements — rather than as utility paperwork — are likely to find interconnection easier.

The technical base is already in place. Grid-forming batteries have operated at scale since 2018, NERC has proposed a testable definition, and UNIFI has published vendor-neutral specifications. What's new is the load: gigawatt-scale computing that can disappear in seconds or swing in periodic patterns. That makes grid-forming control a data center design decision, not just a utility one.

---

*This article continues our AI power-infrastructure coverage from "The Grid Problem" and "480V and HVDC Rack Power Distribution for Gigawatt-Class AI Datacenters." The next step down the chain is medium-voltage conversion: solid-state transformers and whether power electronics can replace the substation transformer between the grid and the rack.*

---

**Stay in the loop.**
Project Isocline publishes deep-dive technical analysis on AI infrastructure, energy systems, and the engineering decisions shaping the next decade of compute. No noise. No fluff.

[Subscribe to the newsletter →](https://isocline.kit.com)

*You can unsubscribe at any time.*