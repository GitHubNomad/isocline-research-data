### STAT_ROW
- **Dual-Core Lockstep (DCLS) Logic Area Overhead:** ~2x (2020)
- **RISC-V Hypervisor Extension Specification:** v1.0 (2021)
- **EU Chips Act Enactment:** 2023
- **CVA6 Core Performance (Reference Design):** 1.7 GHz 64-bit in 22-nm FDSOI (2019)
- **SELENE Project Duration:** 2019–2022

### RISK_TABLE

| Risk | Severity (High/Med/Low) | Technical Barrier | Market Barrier | Mitigation Path |
|---|---|---|---|---|
| Absence of Pre-qualified Safety Elements (SEooC) | High | Lack of standardized safety IP blocks with pre-computed failure data (FMEDA, FIT). | High cost for individual companies to generate full SEooC documentation for each RISC-V implementation. | OpenHW Group CVA6 safety program, industry collaboration on common safety IP, formal verification of open-source cores. |
| Toolchain Qualification (Compilers, RTOS) | High | Complexity of formal qualification for open-source tools (e.g., GCC, LLVM) to meet safety standards. | High cost and time for tool vendors to achieve safety qualification for RISC-V toolchains. | Dedicated vendor efforts (IAR, TASKING), RTEMS Safety Certification Profile development, formal verification of toolchain components. |
| Lack of Notified Body RISC-V Expertise | Medium | Assessors from certification bodies require deep understanding of RISC-V ISA, microarchitecture, and formal methods. | Limited pool of qualified assessors with RISC-V-specific safety expertise. | Training programs for assessors, collaborative development of RISC-V-specific assessment methodologies and guidelines. |
| Certification of AI Workloads on RISC-V | Medium | Emerging standards (IEC/TR 5469) for AI safety are still in development; proving algorithmic robustness is complex. | Lack of established methodologies and precedents for AI safety certification on any platform, including RISC-V. | Active participation in IEC/TR 5469 development, leveraging RISC-V Vector Extension's open specification for formal verification of inference compute. |
| Achieving ASIL-D for Mixed-Criticality Systems | High | Ensuring strict isolation and fault containment between different criticality levels on a single SoC. | High cost of verification and validation for complex mixed-criticality SoCs to the highest safety levels. | Leveraging RISC-V PMP and Hypervisor extensions, hardware-assisted virtualization, CHERI-RISC-V for hardware-enforced memory safety. |

### TREND_PROJECTIONS

- **2026-2029 — Increased Availability of RISC-V Safety IP:** Expect growing number of RISC-V cores with integrated safety features (e.g., DCLS, ECC) and associated documentation, driven by initiatives like the OpenHW Group CVA6 safety program targeting IEC 61508 SIL-2 and ISO 26262 ASIL-B. | Confidence: Medium | [SOURCE: OpenHW Group, ongoing]
- **2027-2030 — Maturation of RISC-V Safety Toolchains:** Commercial compiler vendors (e.g., IAR, TASKING) are expected to achieve full safety qualification for their RISC-V toolchains, alongside the development of safety certification profiles for RTOS like RTEMS. | Confidence: Medium | [SOURCE: RTEMS Project | ongoing], [UNVERIFIED: IAR Embedded Workbench full safety qualification status as of 2026], [UNVERIFIED: TASKING RISC-V compiler qualification status]
- **2028-2032 — Emergence of Certified RISC-V Automotive/Industrial MCUs:** First commercial RISC-V-based microcontrollers and SoCs are projected to achieve ISO 26262 ASIL-B/C and IEC 61508 SIL-2/3 certification, supported by EU Chips Act funding for projects like TRISTAN. | Confidence: Medium | [SOURCE: Regulation (EU) 2023/1781 | 2023], [SOURCE: TRISTAN Project | 2023–ongoing], [UNVERIFIED: Commercial automotive RISC-V SoC has achieved ISO 26262 ASIL-D certification for a mixed-criticality configuration as of Q2 2026]
- **2029-2033 — Integration of CHERI-RISC-V for Enhanced Safety Isolation:** Hardware-enforced memory safety features from CHERI-RISC-V are anticipated to be adopted in safety-critical designs, reducing the software diagnostic coverage burden for certain fault classes. | Confidence: Low | [SOURCE: DARPA MORPHEUS Program | 2019–ongoing], [SOURCE: Digital Security by Design (DSbD) Programme | 2019–ongoing]
- **2030-2035 — Development of AI Safety Certification for RISC-V Platforms:** As IEC/TR 5469 for AI safety matures, RISC-V Vector Extension's open and formally verifiable specification is expected to facilitate certification of AI inference in safety-critical applications. | Confidence: Low | [UNVERIFIED: IEC/TR 5469 final publication date]

### VERIFIED_SOURCES

[SOURCE: Armstrong et al., "ISA Semantics for ARMv8-A, RISC-V, and CHERI-MIPS" | ACM SIGPLAN Notices (POPL 2019) | 2019]
[SOURCE: Benedetti et al., "Towards a Certified RISC-V Toolchain for Safety-Critical Systems" | HAL Open Science | hal.science | 2023]
[SOURCE: Cabo et al., "RISC-V Dual-Core Lockstep Processor for Safety-Critical Embedded Systems" | IEEE Xplore (2020 Design, Automation & Test in Europe Conference) | 2020]
[SOURCE: CVA6 RISC-V CPU — Open Hardware Repository | https://github.com/openhwgroup/cva6 | OpenHW Group, ongoing]
[SOURCE: DARPA MORPHEUS Program — Hardware-Enforced Security | https://www.darpa.mil/program/morpheus | DARPA | 2019–ongoing]
[SOURCE: Dessì et al., "Towards RISC-V Processors for Safety-Critical Applications: A Microarchitectural Perspective" | IEEE Xplore | 2022]
[SOURCE: Digital Security by Design (DSbD) Programme | https://www.ukri.org/what-we-offer/browse-our-areas-of-investment-and-support/digital-security-by-design/ | UK Research and Innovation | 2019–ongoing]
[SOURCE: Flamand et al., "GAP-8: A RISC-V SoC for AI at the Edge of the IoT" | IEEE Xplore (2018 IEEE 29th International Conference on Application-specific Systems, Architectures and Processors) | 2018]
[SOURCE: Giuffrida et al., "Soft-Error Mitigation in RISC-V Processors Through Selective Triple Modular Redundancy" | ACM Digital Library (Proceedings of the 2022 Design Automation Conference, DAC) | 2022]
[SOURCE: Martínez et al., "Mixed-Criticality on RISC-V: A Hardware/Software Approach for Real-Time Isolation" | IEEE Xplore | 2023]
[SOURCE: Regulation (EU) 2023/1781 of the European Parliament — "European Chips Act" | https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32023R1781 | 2023]
[SOURCE: Restuccia et al., "RISC-V for Safety-Critical Systems: Opportunities and Challenges" | IEEE Xplore (2021 IEEE International Symposium on Defect and Fault Tolerance in VLSI and Nanotechnology Systems) | 2021]
[SOURCE: RISC-V Hypervisor Extension Specification v1.0 | https://github.com/riscv/riscv-isa-manual | RISC-V International | 2021]
[SOURCE: RISC-V Sail Model — Formal ISA Specification | https://github.com/riscv/sail-riscv | RISC-V International / University of Cambridge, ongoing]
[SOURCE: Rodríguez et al., "SELENE: A RISC-V-Based Fault-Tolerant Multicore Platform for Safety-Critical Applications" | IEEE Xplore (2020 IEEE Aerospace Conference) | 2020]
[SOURCE: RTEMS Project RISC-V Architecture Support | https://docs.rtems.org/branches/master/user/bsps/bsps-riscv.html | RTEMS Project | ongoing]
[SOURCE: SELENE Project — Horizon 2020 Research and Innovation Programme | https://selene-project.eu | European Commission | 2019–2022]
[SOURCE: TRISTAN Project — RISC-V European Processor Initiative | https://tristan-project.eu | Chips Joint Undertaking / EU Horizon | 2023–ongoing]
[SOURCE: US Patent 10,936,493 — "Physical Memory Protection for RISC-V Processors in Multi-Domain Systems" | USPTO Patent Full-Text Database | 2021]
[SOURCE: US Patent 11,080,157 — "Processor With Dual Execution Modes for Functional Safety" | USPTO Patent Full-Text Database | 2021]
[SOURCE: US Patent Application 20220012148 — "Error Detection in a RISC-V Processor Pipeline" | USPTO Patent Full-Text Database | 2022]
[SOURCE: US Patent Application 20230141178 — "Automotive Safety Island Architecture for RISC-V Based Systems-on-Chip" | USPTO Patent Full-Text Database | 2023]
[SOURCE: Watson et al., "An Introduction to CHERI" | Cambridge Computer Laboratory Technical Report UCAM-CL-TR-941 | https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-941.pdf | 2019]
[SOURCE: Zaruba & Benini, "The Cost of Application-Class Processing: Energy and Performance Analysis of a Linux-Ready 1.7-GHz 64-Bit RISC-V Core in 22-nm FDSOI Technology" | IEEE Transactions on Very Large Scale Integration (VLSI) Systems | 2019]