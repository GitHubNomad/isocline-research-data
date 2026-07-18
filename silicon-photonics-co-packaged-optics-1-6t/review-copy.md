# REVIEW COPY — Silicon Photonics Co-Packaged Optics and the Death of the Pluggable Transceiver at 1.6T

## SEO Metadata
- Slug: silicon-photonics-co-packaged-optics-1-6t
- Meta Description: Explore the shift from pluggable transceivers to Co-Packaged Optics at 1.6T, focusing on DSP retimer removal and thermal challenges. 160 characters.
- Focus Keyword: silicon photonics co-packaged optics
---
## Monetization Notes
Focus on high-value partners like NVIDIA and Broadcom that align with the article's technical depth. Affiliate links are limited due to the niche nature of the content.
---

# The DSP Is Dying. The Argument Is About Where to Put the Optics.

## At a Glance

The framing you've probably encountered—"the pluggable transceiver dies at 1.6T"—is wrong in a specific and instructive way. What's actually dying is a component buried inside the transceiver: the digital signal processing retimer that has, until now, cleaned up the electrical signal on its journey from the switch ASIC to the optics. The physics forcing that component out of the link is real, dated, and non-negotiable. But that same physics admits *two* valid architectural answers, not one.

The first answer is Co-Packaged Optics (CPO): move the optical engine off the front panel and onto the switch package itself, millimeters from the ASIC. The second is Linear Pluggable Optics (LPO): keep the optic at the front panel, in its familiar cage, but strip the retimer out anyway. Both are shipping or commercialized at the 1.6-terabit-per-second generation. Both claim the pluggable's power problem as their justification.

This article builds the causal chain the existing coverage leaves broken: the signal-integrity mechanism that forces the change, the CPO architecture that results, the thermal wall that dictates its most important design decision, and the reason the pluggable form factor survives—transformed—rather than dying.

---

## The Copper Wall

Start with the constraint, because everything downstream follows from it.

A pluggable optical transceiver sits at the front panel of a switch, connected to the switch ASIC by centimeters of copper trace on a printed circuit board. For most of Ethernet's history, this was fine. It stopped being fine at 200 gigabits per second per lane.

The 1.6T port is built from eight lanes of 200 Gb/s PAM4 signaling. (PAM4—four-level pulse amplitude modulation—encodes two bits per symbol, doubling throughput over simple on-off signaling at the cost of much tighter noise margins.) This is a genuine architectural shift, not merely a faster version of the old thing. As the IEEE 802.3dj work makes explicit, earlier 800G designs aggregated eight 100G lanes; 802.3dj reaches 800G with four 200G lanes and 1.6T with eight, halving the lane count at each capacity by doubling per-lane speed.

Here's the problem: PCB insertion loss—the signal energy a copper trace absorbs and dissipates before it reaches the far end—grows with frequency. Dominated by skin effect and dielectric absorption, it scales roughly with the square root of frequency and linearly with trace length. Push the per-lane rate to 200 Gb/s and the loss over centimeters of PCB becomes untenable. The industry's consensus is blunt: the traditional model of pluggable transceivers separated from the switch ASIC by centimeters of PCB trace cannot continue to scale.

The historical fix has been to fight loss with silicon. A DSP retimer inside the transceiver receives the degraded electrical signal, equalizes it, and regenerates a clean copy to drive the optics. That works—but it costs power and adds latency, because the retimer's equalization and retiming impose real overhead. And it's precisely this component that the 1.6T generation renders optional.

*Backward link: readers who followed our earlier treatment of the electrical-interface limits—CEI-224G and the SerDes wall—will recognize this as the optical consequence of the same copper problem. This is where that story arrives.*

---

## What Removing the DSP Actually Buys You

Here's the causal core, stated plainly. Co-Packaged Optics integrates the optical engine—the photonics plus its driver and transimpedance-amplifier electronics—directly onto the switch package substrate, adjacent to the ASIC, rather than out at the front panel.

Shortening the electrical path does two things at once: it shrinks the link budget the electrical channel must satisfy, and it eliminates the need for the DSP retimer entirely. With the optics sitting millimeters from the ASIC, the SerDes lanes can drive the optical engine directly at 200 Gb/s without an intervening retiming stage. Remove the retimer and you remove its power draw and its latency overhead in one move.

The power evidence is the strongest quantitative material available, and it converges from both vendor and operator sources. An 800G DR4 pluggable transceiver consumes roughly 16–17 W. By contrast, the optical engine plus external laser in the NVIDIA Q3450 CPO switch delivers 4–5 W per 800G of bandwidth—about a 73% reduction. This isn't a single-source claim. Meta, presenting at ECOC 2025, reported figures very close to these, showing an 800G 2×FR4 pluggable at roughly 15 W against a substantially lower figure for the optical engine and laser inside Broadcom's Bailly 51.2T CPO switch.

A word of discipline about that ~73% number. It's a set of point estimates for specific 800G products with specific baselines—not a validated average for the 1.6T generation. Treat it as an attributed estimate of what shipping hardware achieves, not as a universal constant. The more durable framing comes from peer-reviewed modeling: a 2021 IET study, grounded in a working CPO demonstrator, estimated that overall energy efficiency below 10 pJ/bit (picojoules per bit—the energy cost of moving a single bit across the link) is attainable. For comparison, a separate energy study places front-panel pluggable optics at roughly 20 pJ/bit. The direction is unambiguous, even where the exact multiplier is soft.

---

## The Modulator Question: Rings Versus Interferometers

Below the architecture sits a component choice that materially shapes the energy claims: how you actually imprint data onto light.

Two modulator families dominate silicon photonics. The Mach-Zehnder modulator (MZM) splits light down two arms, shifts the phase in one, and recombines them so they interfere—a robust, broadband, but physically large device. The micro-ring modulator (MRM) uses a resonant ring only microns across, tuned so that a small change in refractive index swings the ring in and out of resonance with the signal wavelength.

NVIDIA's production optical engines take the MRM path, built on TSMC's COUPE process with a 3D-stacked electronic IC and photonic IC. The rationale is energy and density: micro-rings reduce drive voltage and energy per bit relative to MZM alternatives, and they're dramatically smaller. The patent literature underscores the size gap—a Mach-Zehnder modulator runs about 1 mm, enormous next to other devices in a high-density photonic integrated circuit.

That density comes with a liability. A micro-ring's resonance is exquisitely temperature-sensitive; hold it on-resonance and any thermal drift detunes it. Which brings us to the single most important design decision in production CPO.

---

## The Thermal Wall and the External Laser

This is where the academic literature and the shipping hardware finally meet—and where the "clean death of the pluggable" narrative breaks entirely.

The problem is heat. A modern switch ASIC at 51.2T or 102.4T switching capacity dissipates 300–500 W or more. CPO deliberately places temperature-sensitive optical components—above all, lasers—into that thermal environment. The foundational modeling here is Buscaino et al. in the IEEE *Journal of Lightwave Technology* (2021), which addressed the question directly: co-packaging near high-temperature electronics can diminish optical component performance and reliability. Their analysis extended to 102.4 Tb/s switching and found that coherent detection architectures could support it with over 13 dB of link budget—but the thermal caveat is the load-bearing finding.

The reliability data makes the stakes concrete. Field data from hyperscalers place laser sources among the top three failure modes in optical systems. Integrated lasers—fabricated onto the photonic die itself—carry known reliability challenges, and those challenges worsen next to a 400 W ASIC. This is why production CPO overwhelmingly uses an **External Laser Source (ELS)**: pull the laser out of the hot package and pipe its continuous-wave light in. Because integrated sources are the reliability liability, the industry is moving the laser out, not in.

The thermal trade-off doesn't vanish at ELS. It relocates. A 2025 IEEE JSTQE study by Coenen and colleagues built an experimentally validated thermo-optic laser model for a 64-channel transceiver and identified a clean trade-off between laser-array area and overall thermal resistance—spread the lasers out to shed heat, or pack them densely and fight the thermal penalty. There's no free configuration.

The consequence is the crux of this piece: **the pluggable laser survives even as the pluggable transceiver recedes.** The electro-optic conversion moves onto the package, but a serviceable, external, often pluggable laser module persists—because the alternative, a laser cooking beside the ASIC, fails in the field.

*Backward link: if you followed our earlier discussion of External Laser Source form factors and the ELSFP module, this is the architectural payoff that justified them.*

---

## Pluggability Refuses to Die

Even the transceiver's pluggability—the ability to pull and replace a module by hand—doesn't disappear cleanly. NVIDIA's answer to the obvious objection ("you can't hot-swap a soldered optic") is the detachable optical sub-assembly (OSA): three optical engines clustered into a replaceable 4.8 Tbps module that mates with the switch substrate rather than being permanently bonded, as in Broadcom's approach. The fiber interfaces travel with the replaceable module.

This matters because CPO changes the operational failure model in ways operators can't ignore. If an optical engine in a fully bonded CPO package fails, repair may mean replacing the entire assembly—or an entire line card. That's a different spares-inventory and maintenance regime than swapping a per-unit pluggable at the front panel. Which is exactly why pluggable optics are still projected to dominate deployed port counts near-term: they're flexible, mature, and straightforward to operate. CPO earns its place specifically where bandwidth and port density are pushed so hard that long high-speed electrical traces and front-panel cages become the binding constraint—the highest-radix tier, not the whole data center.

And then there's the competing answer to the same physics. Linear Pluggable Optics removes the DSP too—it simply leaves the optic at the front panel and drives it linearly. As the OIF framing puts it, in both CPO and LPO one of the two DSPs is removed from the link; CPO moves the optics toward the ASIC, while LPO keeps them in the front-panel cage. This isn't a fringe hedge. Marvell has commercialized a 1.6T LPO chipset, and Eoptolink demonstrated 200G-per-lane LPO at OFC 2024. At the exact 1.6T node where the pluggable is declared dead, the pluggable is fighting back.

*Forward link: the CPO-versus-LPO schism deserves its own treatment—two architectures answering one physical constraint, with different bets on serviceability, interoperability, and reach. We'll take it up separately.*

---

## The Standards Scaffolding

None of this is speculative at the standards layer. The IEEE P802.3dj Task Force is defining Ethernet at 200G, 400G, 800G, and 1.6T using 200 Gb/s-per-lane signaling for both electrical and optical interfaces, with completion timed for July 2026. That's the PHY-layer foundation for the 1.6T port.

The packaging side is less settled. The confirmed CPO packaging standard is the OIF 3.2T Co-Packaged Module Implementation Agreement (2023)—an industry first—defining a 3.2T module on 100G electrical lanes that enables a 51.2 Tb/s aggregate-bandwidth switch. Note the mismatch, and note it honestly: there's a ratified CPO *module* agreement, but it targets the 51.2T switch on 100G lanes, not a 1.6T-port CPO packaging agreement on 200G lanes. The verified 1.6T standard is the IEEE PHY, not a CPO packaging IA.

---

## Future Speculation

**Speculation:** The natural next step in the OIF packaging roadmap is a 200G-lane CPO module—a 6.4T building block dimensioned for the 102.4T switch generation, mirroring the way the existing 3.2T/100G-lane IA was dimensioned for 51.2T. This is a grounded extrapolation from the published trajectory (a ratified 3.2T IA on 100G lanes, plus an IEEE move to 200G lanes by mid-2026), not a claim of an existing document. If and when such an IA publishes, it becomes the connective standard for the tier above 1.6T.

**Speculation:** The modulator debate doesn't end with silicon micro-rings. Heterogeneous integration of thin-film lithium niobate onto active silicon photonics—an active arXiv research vector—points toward higher-linearity single-chip transceivers that could, in a later generation, displace silicon MRMs where linearity dominates. This is a research trajectory with published experimental basis, not a product timeline.

**Speculation:** The thermal trade-off Coenen and colleagues quantified—laser-array area against thermal resistance—is likely to become a first-class co-design constraint rather than an afterthought. As switch ASICs climb from 300–500 W toward the next capacity tier, the laser's refusal to sit beside the ASIC will shape package floorplans as much as the electrical channel does.

*Forward link: the 102.4T generation and its 6.4T engine—tracking the next OIF packaging agreement and the roadmap toward 204.8T switching—is the sequel to this piece. So is a dedicated treatment of thermal co-design, built directly on the laser-array modeling referenced here.*

---

## The Verdict

The pluggable transceiver isn't dead at 1.6T, and saying so obscures the more interesting truth. The DSP retimer is what the physics evicts, forced out by PCB insertion loss that no longer tolerates centimeters of copper at 200 Gb/s per lane. That single constraint has two legitimate solutions. CPO pulls the optics onto the package and, to survive the 300–500 W thermal environment, pushes the laser back out via External Laser Sources and detachable sub-assemblies. LPO leaves the optics at the front panel and removes the retimer anyway.

What ends at 1.6T is the front-panel DSP transceiver's monopoly at the highest-radix tier. What survives is nearly everything else about the pluggable model—its serviceability, its spares logic, even its laser—reassembled around the one component that physics would no longer permit. The honest headline isn't a funeral. It's a bifurcation, and the argument is about where to put the optics.

---

*Sources referenced in this article include the IEEE 802.3dj standards work; the OIF 3.2T Co-Packaged Module Implementation Agreement (2023) and OIF CEI electrical-interface framing; Buscaino et al., IEEE Journal of Lightwave Technology (2021); Coenen et al., IEEE JSTQE (2025); the IET Optoelectronics CPO energy modeling study (2021); Meta's ECOC 2025 presentation; and public technical analyses of NVIDIA's Q3450 and Broadcom's Bailly CPO switches. Power and reliability figures attributed to specific vendors and operators are point estimates for specific products and are cited as such.*

---

**Stay in the loop.**
Project Isocline publishes deep-dive technical analysis on AI infrastructure, energy systems, and the engineering decisions shaping the next decade of compute. No noise. No fluff.

[Subscribe to the newsletter →](https://isocline.kit.com)

*You can unsubscribe at any time.*