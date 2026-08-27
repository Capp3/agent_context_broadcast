---
report_id: audio-wordclock-broadcast-engineering
title: Audio Wordclock and Reference Synchronization in Broadcast Engineering
topic: Wordclock for Audio
report_version: 0.1.0
generated_date: 2026-08-27
source_access: mixed
source_cutoff: 2026-08-27
prompt_template: template-new-topic.md
prompt_template_version: 0.1.0
status: draft
---

## 1. Executive Summary

Audio wordclock in broadcast systems is the timing reference that ensures all digital audio devices sample and process audio at a common, phase‑aligned sampling frequency, preventing clicks, pops, and loss of audio at interconnection boundaries.[2][9]  

The primary formal standard governing digital audio reference signals is AES11, which defines the Digital Audio Reference Signal (DARS) carried over an AES3 interface and includes an Annex on baseband “word clock” practice.[1][5][8] However, the detailed normative text of AES11 is paywalled, and accessible material largely consists of summaries and secondary guidance that describe recommended practices for wordclock waveforms, jitter limits, cabling, and distribution architectures.[2][8][12][13]

In contemporary broadcast facilities, audio wordclock commonly exists in three forms:

1. AES11 DARS over AES3 for facility‑wide reference.[1][5][9]  
2. Baseband wordclock as a square‑wave at the sampling frequency on 75 Ω BNC.[2][4][12]  
3. Network‑derived clocks (e.g., IEEE 1588 PTP in AES67 systems), from which devices derive internal wordclock.[26][29]

Normatively, AES11 defines the use of AES3 signals as a reference (DARS) and specifies limits on jitter and wander for wordclock distribution; secondary materials quote a ±5% jitter/wander budget, corresponding to ±1 µs at 48 kHz.[5][13] Baseband wordclock waveform characteristics, 75 Ω BNC cabling, and star distribution are widely described as recommended or common practice rather than strict requirements.[2][4][12][24]

This report catalogues explicit normative requirements where they can be traced to AES11 or AES67, differentiates them from industry best practices, and highlights implementation‑dependent design decisions, especially around hybrid environments (AES3, MADI, and AES67) where multiple clock domains must be coherently managed.[5][9][21][25][29]

---

## 2. Scope and Boundaries

### 2.1 Topic Scope

This report covers:

- Facility‑wide and device‑to‑device synchronization of digital audio via:
  - AES11 Digital Audio Reference Signal (DARS) over AES3.[1][5][9]  
  - Baseband wordclock derived from or aligned with AES11 requirements.[1][2][8][12]  
  - Network‑based clocks (PTP) as used in AES67 and related audio‑over‑IP systems, insofar as they produce device wordclock.[26][29]

- Application context:
  - Broadcast plants and studios where multiple digital audio devices (AES3, MADI, audio‑over‑IP) must operate at a common sampling rate and phase.[9][21][25]

### 2.2 Out‑of‑Scope / Explicit Non‑Coverage

The following topics are out of scope except where they directly affect wordclock:

- Detailed AES3 physical and frame structure (covered by AES3, not retrieved here).[5]  
- Full text of AES11 clauses beyond snippets available in secondary sources (paywalled).[8][14]  
- Detailed SMPTE timing standards (e.g., SMPTE ST 2059) beyond their indirect use via vendor documentation.[13][26]  
- Vendor‑specific proprietary clocking enhancements beyond what is stated in publicly available manuals.[3][6][7][11][16][23]

### 2.3 Adjacent Standards and Dependencies

- **AES11 (Digital Audio Reference Signal)**:
  - Defines use of AES3 as Digital Audio Reference Signal (DARS) for clock distribution.[5]  
  - Includes Annex B on “Word Clock,” describing common practice for baseband wordclock signals.[1][8]  

- **AES3 (Serial digital audio interface)**:
  - Underlies DARS since AES11 distributes clocks via an AES3 signal.[5]  

- **AES67 (Audio over IP interoperability)**:
  - Requires IEEE 1588‑2008 Precision Time Protocol (PTP v2) for clock synchronisation in AoIP systems, from which local wordclocks are derived.[26][29]  

- **IEEE 1588‑2008 (PTP v2)**:
  - Time synchronization protocol used for media networks, providing a high‑precision time base from which devices derive an internal sampling clock.[26][29][30]  

- **MADI (AES10)**:
  - Multichannel digital audio interface whose transmission clock is decoupled from audio sample rate; a separate wordclock or reference is typically required for correct operation.[21][25]

### 2.4 Source Access Limitations

- AES11:2009 (R2014) and its Annex B are paywalled; available information comes from:
  - A standards listing summarising that Annex B covers wordclock.[8]  
  - Secondary descriptions in Wikipedia and professional organisation articles.[1][5]  
  - A timing whitepaper quoting AES11 jitter/wander limits.[13]  
- No clause‑by‑clause AES11 text was available; all AES11‑derived details in this report are therefore secondary or indirect.  
- AES3, AES10, and full AES67 texts are similarly paywalled or not directly accessed; only summary statements from public secondary sources and vendor documents are used.[5][21][23][29]  

All AES11‑based statements are explicitly marked as **Normative (from AES11, via secondary citation)** or **Unverified** where uncertainty remains.

---

## 3. Standards and Source Map

### 3.1 Standards and Source Map Table

| Document | Version/date (as cited) | Role | Source status | Clause‑level visibility |
| --- | --- | --- | --- | --- |
| AES11:2009 (R2014) | Listing indicates 2009 with 2014 reaffirmation; Annex B Word Clock.[8] | Primary standard for digital audio reference signal (DARS) and wordclock practice.[5][8] | Paywalled AES standard; only summaries accessible. | Only high‑level description; Annex B existence and title known, but no clauses. |
| AES11-1991 (ANSI S4.44-1991) | 1991; described as earlier edition.[14] | Historical version of digital audio reference recommendations.[14] | Paywalled; summary only. | No clause text; only general description of genlock/masterclock concepts.[14] |
| AES67 | Article updated 2026-03-08.[29] | Audio over IP interoperability; specifies use of PTP for clock synchronisation.[29] | Public secondary summary; primary standard paywalled. | No clauses; only overall statement that AES67 uses IEEE 1588-2008 PTP.[29] |
| IEEE 1588-2008 (PTP v2) | 2008; version referenced in AES67 article.[29] | Time synchronisation standard used by AES67 and Dante‑like networks.[26][29][30] | Primary standard paywalled; referenced via secondary sources.[26][29][30] | No clause text used. |
| MADI (AES10) | Wikipedia article last updated 2026-03-09.[21] | Describes decoupling of MADI transmission clock from audio sample rate and need for separate wordclock.[21] | Public secondary summary. | No clauses; only descriptive text.[21] |
| IPS “Word Clock” article | Updated 2025.[2] | Professional guidance on wordclock; refers to AES11 timing requirements and baseband wordclock.[2] | Public; secondary but technically detailed. | Article‑level; no formal clauses but multiple specific statements.[2] |
| Ashly “Word Clock Basic Facts” | 2015 PDF; updated 2026 index.[12] | Vendor application note describing wordclock waveform, cabling, and lack of strict AES standard; references AES11 recommended practice.[12] | Public vendor document; secondary. | Document‑level visibility.[12] |
| Ravenna “AES67-SMPTE ST-2110 Timing Synchronization” | Whitepaper; updated 2025.[13] | Technical paper quoting AES11 jitter/wander distribution limits.[13] | Public; secondary; references AES11.[13] | Statement‑level; references AES11 jitter limits but not underlying clause.[13] |
| Audio/Video Company “AES” page | Updated 2026.[9] | Educational article summarizing AES11’s role in wordclock distribution and multi‑interface systems.[9] | Public secondary article. | Section‑level.[9] |
| MADI Interfacing Considerations (Wheatstone) | Support article; updated 2025.[25] | Implementation note on using DARS and wordclock with MADI; distinguishes DARS vs wordclock.[25] | Public vendor support article. | Paragraph‑level.[25] |
| DirectOut AES67 Stream Setup | PDF; updated 2026.[23] | Implementation guidance for using wordclock/MADI as external sync for an AES67 PTP network.[23] | Public vendor guidance. | Section‑level.[23] |
| Yamaha “Time is Precious” micro tutorial | 2017 article; updated index 2025.[26] | Educational note on PTP‑based audio networks and derivation of wordclock from PTP.[26] | Public secondary article.[26] | Article‑level.[26] |
| Sound On Sound “Is it best to synchronise…” | Article dated 2026-03-06.[24] | Best‑practice advice on clock distribution (star topology, master generator).[24] | Public editorial article.[24] | Article‑level.[24] |
| Audiodrome “Clocking (Digital Audio Sync)” | Article dated 2025-08-07.[27] | Step‑by‑step practice for choosing master, wiring, termination.[27] | Public secondary article.[27] |
| Product manuals (Evertz 520DARS‑W, Grass Valley 4404, Rosendahl Nanoclocks, Mutec clocks) | Various dates 2003–2026.[3][4][6][7][10][11][16][22] | Implementation examples: waveform levels, outputs, input types, AES11 Grade 1 reference clocks.[3][4][6][7][10][11][16][22] | Public vendor material. | Section‑ and spec‑table level.[3][4][6][7][10][11][16][22] |

---

## 4. Normative Requirements Catalog

### 4.1 Normative Requirements Table

**Key:**

- Normative status:
  - **Normative (AES11)** – inferred from AES11 via secondary sources.
  - **Normative (AES67)** – inferred from AES67 via secondary sources.
  - **Best practice** – industry or vendor recommendation.
  - **Assumed** – general engineering principle, not tied to a specific standard.
  - **Unverified** – suspected standard behaviour without adequate evidence.
- Confidence:
  - High – multiple independent sources or clear standard statement.
  - Medium – single source, or standard only referenced indirectly.
  - Low – single secondary source or inferred.

| ID | Requirement or rule | Applies to | Normative citation | Normative / best practice / assumed / unverified | Implementation implication | Confidence |
| --- | --- | --- | --- | --- | --- | --- |
| WRD‑REQ‑001 | A Digital Audio Reference Signal (DARS) is carried using an AES3 interface signal and is recommended for distributing audio clocks within a facility. | Facility clock distribution, AES3 devices | AES11 description: AES11 recommends using an AES3 signal to distribute audio clocks; this application is called DARS.[5] | Normative (AES11, via secondary) | Use AES3‑format “blank” signals as reference clock feeds instead of separate baseband wordclock where practical. | High |
| WRD‑REQ‑002 | AES11 includes an Annex B on wordclock, describing common practice for transmitting and receiving a plain wordclock signal. | Equipment implementing baseband wordclock | AES11 listing (“Annex B – Word Clock”) and wordclock article describing Annex B common practice.[1][8] | Normative scope (existence), content not fully visible; practice described is secondary | Annex B is the primary normative context for baseband wordclock; implementers should treat its contents as the authoritative reference where accessible. | Medium |
| WRD‑REQ‑003 | A facility‑wide wordclock reference is required (by practice and AES11 intent) to keep multiple digital audio devices at the same sample rate and phase, avoiding sample rate conversion artifacts and failures. | Multi‑device digital audio systems | AES article: AES11 defines distribution of a wordclock reference signal; without a common reference, sample rate differences cause clicks, pops, or complete audio failure.[9] | Normative intent (AES11), described via secondary; requirement phrased as best practice | Systems must ensure all devices share a coherent reference; absence of such reference risks audible artifacts and unstable operation. | High |
| WRD‑REQ‑004 | Wordclock frequency equals the digital audio sampling frequency (basic rate) and can satisfy AES11 timing requirements when implemented as a square wave at this frequency. | Devices generating or consuming baseband wordclock | IPS: It is possible to meet all timing requirements of AES11 by means of a square wave at sampling frequency basic rate, commonly called wordclock.[2] | Best practice; AES11 compatibility claim via secondary source | Use a square wave at the audio sampling frequency as the baseband wordclock; ensure generator stability meets AES11 timing limits. | High |
| WRD‑REQ‑005 | AES11 calls for a maximum jitter/wander budget of ±5% for distributed wordclock reference signals, e.g., ±1 µs at 48 kHz, ±500 ns at 96 kHz, ±250 ns at 192 kHz. | Wordclock distribution networks | Timing paper quoting AES11 jitter/wander: ±5%; at 48 kHz: ±1 µs; 96 kHz: ±500 ns; 192 kHz: ±250 ns.[13] | Normative (AES11), quoted by secondary | Wordclock distribution must maintain timing within these limits relative to the nominal period to remain AES11‑compliant. | Medium |
| WRD‑REQ‑006 | AES67‑compliant systems must use IEEE 1588‑2008 Precision Time Protocol (PTP v2) for clock synchronisation. | AES67 network audio senders/receivers | AES67 article: AES67 uses IEEE 1588‑2008 PTP v2 for clock synchronisation.[29] | Normative (AES67, via secondary) | All AES67 devices must implement PTP v2; system wordclock is derived from the PTP time base. | High |
| WRD‑REQ‑007 | PTP‑based audio networks generate each device’s internal wordclock from the PTP time information rather than distributing a separate baseband wordclock. | Network audio devices (e.g., Dante, AES67) | Yamaha micro tutorial: PTP is used to synchronise network devices; each device generates an internal wordclock based on PTP information.[26] | Best practice / standard behaviour description | Implementations must synthesize local sampling clocks from the PTP reference, and not expect a separate baseband wordclock in a pure AoIP segment. | High |
| WRD‑REQ‑008 | MADI transmission clock is disassociated from the audio sample rate; a separate wordclock connection is required to maintain synchronisation, though some vendors allow deriving wordclock from MADI timing. | MADI (AES10) devices | MADI article: standard disassociates transmission clock from audio sample rate; requires separate wordclock; some vendors allow locking to transmission timing.[21] | Normative (AES10 behaviour via summary); vendor behaviour as implementation detail | Treat MADI and wordclock as separate clock domains; do not assume a MADI signal alone provides a sufficient wordclock reference unless explicitly supported. | High |
| WRD‑REQ‑009 | MADI‑based systems in TV installations commonly use a DARS (AES11) signal from the house master clock to synchronise systems; wordclock alone may not be acceptable as a Wheatstone system reference. | Broadcast MADI installations | Wheatstone: DARS from house master is used to sync MADI; wordclock is not the same as DARS and cannot be used to synchronise their system.[25] | Best practice (vendor) | Use AES11 DARS as primary sync for MADI equipment when required; verify whether plain wordclock is allowed by specific systems. | High |
| WRD‑REQ‑010 | There is no official AES standard mandating the exact baseband wordclock waveform; AES11 provides a recommended practice, and industry practice has converged on a 0–5 V square wave at sampling frequency over 75 Ω BNC. | Baseband wordclock generators/receivers | Ashly: No official AES standard mandates wordclock signal; a recommended practice exists as part of AES11; industry has generally settled on 0–5 V square wave at sampling frequency over 75 Ω BNC.[12] | Best practice (AES11 recommended practice + industry convergence) | Implementations should adopt the de‑facto 0–5 V, 75 Ω, sampling‑rate square wave unless there is a compelling reason to deviate. | High |
| WRD‑REQ‑011 | Wordclock lines should present approximately 75 Ω source impedance and be terminated with 75 Ω at the far end of the line. | Baseband wordclock cabling | Ashly: Wordclock lines should have a 75 Ω source impedance and a 75 Ω load termination at the farthest end.[12] | Best practice | Design drivers and receivers with appropriate impedance; use terminators or built‑in termination only at the final load. | High |
| WRD‑REQ‑012 | Wordclock connectors are typically 75 Ω BNC per IEC 61169‑8 Annex A; some hardware outputs a 5 V p‑p square wave with allowable tolerance ±0.5 V. | Hardware wordclock I/O | Evertz 520DARS‑W spec: Wordclock outputs on BNC per IEC 61169‑8 Annex A; level 5 V p‑p (0–5 V) ±0.5 V.[4] | Best practice / product‑specific | Use 75 Ω BNC connectors and design for 0–5 V p‑p levels when matching common hardware; verify compatibility for lower‑level signals. | High |
| WRD‑REQ‑013 | The rising edge of the wordclock is a recommended timing reference point for sampling alignment in new designs. | Devices consuming wordclock | IPS: Where new equipment is designed to use a wordclock signal, it is recommended that the rising edge is treated as the timing reference point.[2] | Best practice | Align internal sampling or PLL phase so that the rising edge is the primary timing reference; document this for interoperability. | High |
| WRD‑REQ‑014 | The jitter and wander limits specified by AES11 for wordclock distribution may be expressed as timing deviations (e.g., µs or ns) corresponding to ±5% of the wordclock period. | Wordclock design and verification | Timing paper equates ±5% jitter/wander to ±1 µs at 48 kHz, ±500 ns at 96 kHz, ±250 ns at 192 kHz.[13] | Normative (AES11 values), derived expression | System timing measurement and verification should check these absolute deviations at relevant sampling rates. | Medium |
| WRD‑REQ‑015 | For AES67 networks clocked from external signals such as wordclock or MADI, the device receiving the external sync must be configured as PTP grandmaster; others should run in auto mode. | AES67 gateways and bridges | DirectOut: For networks clocked from external sync such as wordclock or MADI, the externally synced device must become grandmaster, others set to auto.[23] | Best practice / implementation guidance | Configure AoIP bridges to propagate house sync into PTP as grandmaster; misconfiguration can create multiple masters and unstable clocks. | High |
| WRD‑REQ‑016 | In PTP‑based networks, external wordclock selection only affects low‑frequency timing error below a “corner frequency” of a few hertz; high‑frequency jitter is governed by PTP behaviour and PLL design. | Network audio systems | Yamaha: PTP updates a few times per second; devices generate wordclock from this, and an external sync master only influences timing inaccuracy below a corner frequency of a few hertz.[26] | Best practice / explanatory | Do not expect external wordclock to override PTP for high‑frequency jitter; focus on PLL design and network quality. | Medium |

---

## 5. Engineering Model

### 5.1 Core Objects and Relationships

The engineering model for audio wordclock in broadcast environments can be organised around the following objects:

- **Master reference clock / sync generator**:
  - Generates AES11‑compliant DARS and/or baseband wordclock and may lock to analog video syncs (black burst) or tri‑level sync for AV coherence.[6][11][25]  
  - Example devices provide multiple AES11 DARS outputs and wordclock outputs, often with genlock inputs to video references.[6][11]

- **DARS (Digital Audio Reference Signal)**:
  - An AES3‑format signal carrying no useful audio but serving as a reference for sample clock distribution.[5][9]  
  - Recommended by AES11 as the preferred reference within facilities.[5][9]  

- **Baseband wordclock signal**:
  - Square‑wave signal nominally at the digital audio sampling frequency; used as a basic timing reference between devices.[2][12]  
  - Distributed via 75 Ω BNC connectors and coaxial cable.[4][12]  

- **Distribution amplifiers (DA) / clock distribution units**:
  - Receive one DARS or wordclock input and provide multiple isolated outputs (e.g., four wordclock outputs from one AES11 DARS input).[4][6]  
  - May also regenerate AES3 and provide reclocked outputs.[4]  

- **Digital audio devices (consoles, routers, converters, MADI interfaces, AoIP endpoints)**:
  - Expose clock inputs (DARS, wordclock, video sync) and internally generate or lock their sampling clock to these references.[6][19][21][25]  
  - In AES67/Dante devices, PTP inputs replace or supplement wordclock inputs; each device derives internal wordclock.[17][26][29]

- **AoIP PTP network**:
  - Provides IEEE 1588‑based time distribution; one grandmaster node (possibly an external‑sync gateway) drives others.[23][26][29]  

- **Bridges/gateways (MADI ↔ AES67, AES3 ↔ AES67, etc.)**:
  - Interfaces that lock to external wordclock or MADI and act as PTP grandmasters on the network, or vice versa.[23][25]

### 5.2 Data‑ and Timing‑Flow Semantics

#### 5.2.1 Baseband Wordclock / DARS Distribution

- A central sync generator outputs DARS (AES3) and/or baseband wordclock.[6][11][25]  
- Distribution amplifiers fan out these references to multiple destinations using either:
  - AES3‑formatted DARS connections, or  
  - 0–5 V wordclock square‑waves on BNC.[4][9][12]  

- Devices configured as “clock slaves” lock their internal sampling clocks to the incoming reference (DARS or wordclock).[2][19][27]  
- In mixed systems:
  - MADI devices typically require an external reference (DARS or wordclock) because the MADI stream’s transmission clock does not guarantee phase alignment with the audio sample rate.[21][25]  

#### 5.2.2 AoIP and PTP‑Derived Wordclock

- AES67 and similar AoIP systems use PTP to distribute a precise time base.[26][29][30]  
- Each device derives its local sampling clock (wordclock) by phase‑locking a local oscillator to the PTP time information.[26][29]  
- When an AoIP gateway is locked to an external signal (e.g., wordclock or MADI), that gateway should be configured as PTP grandmaster so that PTP time follows the external reference.[23][26]  

#### 5.2.3 Device Clock Modes

Vendor and support material describe typical device clock modes:

- **Internal clock**: Device uses its own oscillator as master; other devices may lock to its outputs.[27][28]  
- **External clock (wordclock/DARS)**: Device acts as slave, locking its PLL to a received wordclock or DARS.[2][19][27]  
- **Network clock (PTP)**: Device locks to PTP grandmaster and derives internal wordclock.[17][26][29]  
- **Asynchronous / SRC mode**: Device uses sample rate converters to adapt incoming audio streams to its own internal wordclock when not locked, usually at the cost of added latency and complexity.[17]

### 5.3 Control‑Flow / Configuration Semantics

Best‑practice guidance outlines a typical control model:

- Select a single master clock device (often highest‑quality A/D converter or dedicated clock unit).[20][27][28]  
- Configure all other devices to use “external” clock mode and select appropriate input (wordclock, DARS, PTP).[2][19][27]  
- For AES67 setups using external sync:
  - Configure the externally clocked device as preferred PTP master or grandmaster.[23][26]  
  - Set other devices to auto mode to avoid multiple master conflicts.[23]  

### 5.4 Boundary Between Standards and Implementation Policy

- **Standards‑derived behaviour**:
  - Use of AES3‑based DARS as digital audio reference (AES11).[5][9]  
  - Use of PTP v2 in AES67 for clock synchronisation (AES67).[29]  
  - Jitter/wander limits for wordclock distribution (AES11 via quoted values).[13]  

- **Implementation policy / best practice**:
  - Choice between DARS vs baseband wordclock for specific links.[2][9][12][25]  
  - Specific waveform amplitude (1 V or 5 V p‑p) and connector usage beyond 75 Ω BNC.[4][6][11][12]  
  - Distribution topology (star vs daisy chain).[24][27]  
  - Selection of master device in a studio or broadcast plant.[20][24][27][28]  

---

## 6. Formulas, Calculations, and Worked Examples

### 6.1 Formula and Assumption Register

| Calculation name | Formula or method | Inputs and units | Source citation | Normative or assumed | Worked example available | Confidence |
| --- | --- | --- | --- | --- | --- | --- |
| Wordclock frequency vs sample rate | \( f_{\text{WC}} = f_s \) (wordclock frequency equals sampling frequency basic rate). | \( f_s \): sampling frequency in Hz. | IPS: square wave at sampling frequency basic rate (wordclock).[2]; Ashly: wordclock specified as 5 V p‑p square wave with frequency matching sampling frequency.[12] | Best practice (AES11‑compatible) | Yes | High |
| AES11 jitter/wander limit (absolute) | Allowed timing deviation \(\Delta t = 0.05 \times T\), where \(T\) is the wordclock period; quoted directly as ±1 µs at 48 kHz, ±500 ns at 96 kHz, ±250 ns at 192 kHz. | \( T \): wordclock period in seconds; derived from sampling rate. | Ravenna timing paper quoting AES11: ±5% jitter/wander, with specific numeric values.[13] | Normative (AES11 values), formula inferred | Yes | Medium |
| Jitter as percentage of period | \( \%J = \frac{|\Delta t|}{T} \times 100 \) (percentage of nominal period). | \(\Delta t\): timing deviation; \(T\): period. | Derived from general definition; numeric examples cross‑checked to the ±5% assertion.[13] | Assumed | Yes | Medium |
| Cable propagation delay estimation | Assumed: coax propagation delay \( t_d \approx \frac{L}{v} \), with velocity factor based on cable type. | \(L\): length (m); \(v\): propagation speed (m/s). | General transmission‑line principle; used qualitatively when discussing long wordclock runs in Word Clock Explained.[18] | Assumed; Unverified vs AES11 | Yes | Low |

### 6.2 Worked Examples

#### 6.2.1 Jitter Limit Example at 48 kHz

Given:

- Sampling frequency: \( f_s = 48\ \text{kHz} \).  
- Wordclock frequency equals sampling frequency.[2][12]  

The Ravenna timing paper states:

- AES11 calls for ±5% maximum jitter/wander.  
- At 48 kHz, this corresponds to ±1 µs.[13]  

This implies:

- Nominal period \(T = 1 / f_s \approx 20.83\ \mu\text{s}\) (assumed general relation).  
- ±1 µs ≈ ±4.8% of 20.83 µs, consistent with the ±5% assertion.[13]  

**Normative status:**

- The ±1 µs value is directly quoted from a document citing AES11; thus treated as **Normative (AES11)**.[13]  
- The explicit calculation of percentage is an **Assumed** verification, not an independent requirement.

#### 6.2.2 Jitter Limit Example at 96 kHz

Given:

- Sampling frequency: \( f_s = 96\ \text{kHz} \).  
- Wordclock frequency equals sampling frequency.[2][12]  

Quoted values:

- AES11 jitter/wander: ±5%.  
- At 96 kHz, ±500 ns.[13]  

Check:

- Nominal period \(T \approx 10.42\ \mu\text{s}\) (assumed).  
- ±500 ns ≈ ±4.8% of 10.42 µs, coherent with ±5% jitter.[13]  

Again, numerical values (±500 ns) should be taken as **Normative**, with the percentage check as **Assumed** support.[13]

#### 6.2.3 Wordclock Level Compatibility Example

Given an Evertz 520DARS‑W wordclock output:

- Level: 5 V p‑p (0–5 V) ±0.5 V.[4]  
- Connector: BNC per IEC 61169‑8 Annex A, 75 Ω.[4]  

Given a Rosendahl nanoclocks input:

- Audio (AES11 or wordclock) minimum level > 250 mV.[11]  

Conclusion:

- Any level from 4.5 V to 5.5 V p‑p (per Evertz tolerance) exceeds the >250 mV minimum of Rosendahl, leaving ample margin.[4][11]  
- This demonstrates interoperability in terms of amplitude between typical 5 V wordclock generators and receivers designed for much lower minimum amplitude.[4][11]  

This is an **implementation example**, not a normative requirement.

---

## 7. Interoperability Risks and Ambiguity Register

### 7.1 Risk and Ambiguity Table

| Risk or ambiguity | Evidence | Normative status | Failure symptom | Mitigation or modeling rule |
| --- | --- | --- | --- | --- |
| Confusion between DARS and baseband wordclock | AES11 defines DARS over AES3 as the preferred distribution method; Annex B covers baseband wordclock; Wheatstone states wordclock is not the same as DARS and cannot synchronise some systems.[1][5][8][25] | DARS definition is Normative (AES11); relationship to wordclock is clear; vendor requirement is best practice. | Using baseband wordclock where DARS is required may lead to unsynchronised systems or refusal to lock. | Treat DARS and baseband wordclock as separate reference types; implement explicit configuration and documentation of which reference is required per device. |
| Assumed equivalence of all wordclock waveforms (levels, polarity) | Manufacturer outputs vary (1 V p‑p wordclock outputs, 5 V p‑p outputs); Ashly notes no official AES standard mandates wordclock waveform, only a recommended practice; IPS recommends rising edge as timing reference.[2][4][6][11][12] | No single normative waveform; AES11 Annex B provides a recommended practice; actual implementation is best‑practice. | Devices may fail to lock or exhibit jitter when driven with incompatible level or edge polarity expectations. | Design receivers with wide level tolerance and configurable edge sensitivity; document expected waveform characteristics and test with common generator types. |
| Over‑reliance on daisy‑chained wordclock with improper termination | Ashly recommends 75 Ω source impedance and far‑end termination; Word Clock Explained warns of reflections and jitter with long cables; Audiodrome emphasises termination at last device.[12][18][27] | Best practice, not a formal standard requirement. | Reflections causing PLL instability, increased jitter, or loss of lock, especially with long chains. | Use star distribution where possible; if daisy‑chaining, ensure only the last device is terminated at 75 Ω and cable lengths are controlled. |
| Misinterpretation of AES11 jitter limits | Ravenna paper cites AES11 ±5% jitter/wander limits; detailed AES11 text is unavailable, leaving ambiguity about how jitter is measured (e.g., measurement bandwidth, averaging).[13] | Normative limits known; measurement methodology is Unverified. | Implementers may under‑ or over‑engineer jitter performance; conformance testing may be inconsistent. | Treat ±5% values as target design limits; explicitly document measurement method and bandwidth; recognise that full AES11 compliance cannot be asserted without accessing the standard. |
| MADI systems clocked only from MADI stream | MADI article states standard disassociates transmission clock from audio sample rate; Wheatstone advises synchronising both ends with DARS and warns that syncing from MADI should be a last resort.[21][25] | Standard behaviour of MADI is Normative; vendor guidance is best practice. | Possible clicks, pops, or loss of audio when MADI endpoints are not co‑timed at the sample level; unstable system operation. | Provide external reference (DARS or wordclock) to both MADI endpoints; use MADI‑derived clock only where explicitly supported and documented as last resort. |
| Bridging external references into PTP networks | DirectOut and Yamaha explain that an externally synced device must act as PTP grandmaster; misconfiguration leads to multiple masters or incorrect hierarchy.[23][26] | Best practice; AES67 behaviour is Normative but not fully visible. | PTP instability, wandering clocks, lost audio, or packet slips in AES67 streams. | Ensure exactly one grandmaster when using external sync; implement clear configuration models for preferred grandmaster and fallback in AoIP controllers. |
| Terminology ambiguity: “house clock” vs “wordclock” vs “sync” | Multiple sources use these terms loosely; AES11 distinguishes DARS from wordclock; vendor docs speak of “sync pulse generator”, “master clock”, and “wordclock” interchangeably.[5][9][25] | Normative distinction exists (DARS vs wordclock), but broader terms are informal. | Mis‑wiring and misconfiguration, e.g., feeding composite video sync into a wordclock input or vice versa. | In documentation and configuration, separate: (1) video sync references, (2) AES11 DARS, (3) baseband wordclock, and (4) PTP; model these as distinct reference types. |
| Cable length limits for wordclock | Word Clock Explained suggests keeping runs short (e.g., under ~15 ft) to avoid reflection problems; other articles suggest longer limits (hundreds of feet) are possible; no AES11 limit is cited.[18][20] | Unverified; purely best practice. | Long runs may produce reflections and jitter, depending on cable quality and termination. | Treat length limits as implementation‑dependent; use 75 Ω broadcast‑grade coax, proper termination, and measure jitter at the far end rather than relying on generic length rules. |

---

## 8. Implementation Guidance

This section consolidates **best practices** and **implementation policies** suitable for engineering design and AI‑assisted modelling. None of the following are strict “shall” requirements unless specifically tied to AES11 or AES67 references.

### 8.1 Clock Hierarchy and Topology

- **Designate a single master clock**:
  - Choose a device with a high‑stability clock (often a dedicated clock generator or the principal A/D converter) as the master.[20][27][28]  
  - Configure all other devices as clock slaves (external clock mode) with matching sample rates.[20][27]  

- **Prefer star distribution**:
  - Sound On Sound and other guidance recommend distributing clock from a central hub or master in a star topology, using wordclock, AES (DARS), or a combination.[24][27]  
  - This reduces cumulative jitter compared to daisy‑chaining and simplifies termination.  

- **Use DARS where appropriate**:
  - AES11 recommends DARS (AES3‑based) for facility clocking; it is often more robust over long distances than baseband wordclock.[5][9]  
  - For systems mixing AES3, MADI, and other digital interfaces, align all devices to the same DARS reference where possible.[9][25]  

### 8.2 Wordclock Signal Design

- **Waveform and amplitude**:
  - Implement a square‑wave wordclock at the sampling frequency; 0–5 V p‑p into 75 Ω is widely used and recommended as practice.[2][4][12]  
  - Ensure the rising edge is clean and has sufficient slew rate; IPS suggests using the rising edge as the timing reference.[2]  

- **Connectors and cabling**:
  - Use 75 Ω BNC connectors and 75 Ω coaxial cable (e.g., broadcast video coax) for wordclock.[4][12]  
  - Avoid using shielded microphone cable for wordclock, as noted in Ashly’s guidance.[12]  

- **Impedance and termination**:
  - Provide a 75 Ω source impedance and ensure only the last device in a chain is terminated with 75 Ω, either internally or via an external terminator.[12][27]  

### 8.3 System Integration Scenarios

#### 8.3.1 AES3 + Wordclock Systems

- For small systems, direct AES3 connections can operate without a separate wordclock as AES3 carries embedded clock; however, larger systems benefit from a central AES11‑style reference.[9]  
- In multi‑device AES3 setups:
  - Distribute DARS via AES3 where possible.  
  - Use baseband wordclock only when device inputs require it or where AES3 reference distribution is impractical.[2][5][9]  

#### 8.3.2 MADI Systems

- Synchronise both sides of a MADI link using a common reference (DARS or wordclock) from the house master clock.[21][25]  
- Follow Wheatstone’s guidance:
  - Use DARS from a master sync pulse generator to feed MADI interfaces via AES distribution amplifiers.[25]  
  - Only derive clock from MADI as a last resort and only if the equipment explicitly supports it.[21][25]  

#### 8.3.3 AES67 / AoIP Systems

- Ensure all AES67 devices implement PTP v2; configure one device (often a bridge locked to wordclock or MADI) as grandmaster.[23][29]  
- When bridging external wordclock into a PTP network:
  - Use the device receiving wordclock as grandmaster (preferred master).[23][26]  
  - Set other devices to auto mode to prevent multiple grandmasters.[23]  

- Recognise that:
  - PTP packets occur at a low rate (a few per second), and each device’s PLL generates wordclock from that; external wordclock affects only low‑frequency error below the corner frequency.[26]  

### 8.4 Operational Practices

- **Configuration and monitoring**:
  - Confirm all devices report “locked” or equivalent sync indicators.[27]  
  - Check digital audio paths for clicks, pops, or dropouts when changing clock configuration.[9][25][27]  

- **Document reference types**:
  - Clearly distinguish between:
    - House video sync,  
    - AES11 DARS,  
    - Baseband wordclock, and  
    - PTP time in AoIP networks.[5][9][25][26][29]  

- **Upgrade and migration planning**:
  - When migrating from baseband wordclock to DARS or PTP, ensure transitional architectures maintain stable references across both domains, possibly using hybrid clock generators that output wordclock, DARS, and PTP‑aligned signals.[7][16][22][23]  

---

## 9. Validation Checklist

This checklist is intended for design reviews, system commissioning, or automated validation logic.

1. **Clock architecture**
   1. Exactly one master clock is configured (or one PTP grandmaster) in each clock domain.[23][24][27][29]  
   2. All devices’ configured sample rates match (e.g., all 48 kHz).[9][21][27]  

2. **Reference distribution**
   1. DARS is used for facility‑wide reference where devices support AES3 reference inputs.[5][9][25]  
   2. Baseband wordclock is present only where required and its distribution maintains 75 Ω topology and appropriate termination.[4][12][27]  

3. **Signal characteristics**
   1. Wordclock amplitude and polarity are compatible between generators and receivers (e.g., 0–5 V p‑p, rising edge reference).[2][4][11][12]  
   2. Wordclock jitter/wander at endpoints is within ±1 µs (48 kHz), ±500 ns (96 kHz), or ±250 ns (192 kHz), following AES11 limits.[13]  

4. **MADI integration**
   1. Both ends of each MADI link are synchronised to the same reference (DARS or wordclock).[21][25]  
   2. MADI‑derived clocking is used only where explicitly supported and documented.[21][25]  

5. **AoIP / AES67 integration**
   1. All AES67 devices have functioning PTP v2 stacks and show lock to the correct grandmaster.[23][26][29]  
   2. Where external wordclock or MADI is the master, the attached node is configured as PTP grandmaster and others as auto.[23]  

6. **Operational indicators**
   1. Devices report “lock” to external clock without frequent relocking or error logs.[19][27]  
   2. Audio paths are free from clicks, pops, or dropouts during normal operation and after clock reconfiguration.[9][25]  

---

## 10. Open Questions / Unverified Items

These items require access to primary standards or further evidence.

1. **Exact AES11 Annex B wordclock waveform specification**  
   - Accessible sources state that Annex B describes common practice and that industry has settled on a 0–5 V square wave, but the precise normative recommendations (e.g., rise time, duty cycle, level tolerances) are not visible.[1][2][8][12]  
   - Status: **Unverified**.

2. **Formal AES11 jitter measurement conditions**  
   - The Ravenna paper quotes ±5% jitter/wander and provides equivalent timing values; the measurement bandwidth, test setup, and compliance definitions from AES11 are not available.[13]  
   - Status: **Unverified**.

3. **AES11 Grade 1 clock definition**  
   - Several products advertise “AES11 Grade 1” with 0.5 ppm internal reference clocks.[3][7]  
   - Without the AES11 text, the formal definition of Grade 1 (including ppm limit and any additional constraints) is unknown.  
   - Status: **Unverified**.

4. **Compatibility requirements between 1 V and 5 V wordclock implementations**  
   - Vendor manuals show 1 V p‑p and 5 V p‑p wordclock outputs and inputs with minimum 250 mV sensitivity,[4][6][11] but no standard explicitly defines interoperability requirements.  
   - Status: **Unverified**.

5. **Maximum recommended cable length for wordclock under AES11**  
   - Various secondary sources suggest different maximum lengths; no AES11 clause specifying length has been identified.[18][20]  
   - Status: **Unverified**.

6. **Detailed AES67/PTR profiles for media clock derivation**  
   - AES67 requires PTP v2, but specific profiles (default, media) and their impact on wordclock recovery are only partially documented in vendor guidance.[23][29][30]  
   - Status: **Unverified** vs primary standard.

---

## 11. Sources

This section lists the numbered sources referenced in the report. No URLs are provided; identifiers correspond to the inline citations.

1. “Word clock” – Encyclopedic article describing wordclock, AES11 Annex B, and DARS terminology.  
2. “Word Clock – IPS” – Institute of Professional Sound article on wordclock signals and AES11 timing requirements.  
3. Mutec MC‑7 / MC‑3.* product specifications noting AES11 Grade 1 internal reference clocks (0.5 ppm).  
4. Evertz 520DARS‑W product specification for unbalanced AES wordclock extractor (BNC, 5 V p‑p wordclock outputs).  
5. “AES11” – Encyclopedic article describing the AES11 digital audio reference signal standard and its use of AES3 (DARS).  
6. Grass Valley 4404 Digital Audio Reference Signal Generator manual (AES11 DARS generator with wordclock outputs).  
7. Mutec MC‑3.2 Smart Clock HD product description (universal reference generator for audio/video, AES11 Grade 1 reference).  
8. AES11:2009 (R2014) standard listing indicating Annex B “Word Clock” and scope of synchronization approach.  
9. Audio/Video Company AES standards overview, explaining AES11’s role in wordclock distribution and multi‑interface systems.  
10. APC IMS‑SCG wordclock / AES11 signal generator product description (frequency ranges, outputs).  
11. Rosendahl Nanoclocks GL manual (wordclock / AES11 inputs and outputs, minimum input levels).  
12. Ashly “Word Clock Basic Facts” application note (waveform, 0–5 V p‑p, 75 Ω BNC, termination, AES11 recommended practice).  
13. Ravenna “AES67–SMPTE ST‑2110 Timing Synchronization” whitepaper (AES11 jitter/wander limits and numeric equivalents).  
14. AES11‑1991 (ANSI S4.44‑1991) summary (earlier version of digital audio reference recommendations).  
15. [Not used substantively.]  
16. Studio Technologies Model 5401 Dante master clock documentation (PTP v2 support for AES67).  
17. DHD AES67 synchronisation support page (wordclock sources and asynchronous SRC mode).  
18. “Word Clock Explained” article (practical cable length advice and termination considerations).  
19. Avid HD MADI Guide (wordclock ports and synchronization).  
20. Perfect Circuit “Studio Concepts: What is Word Clock?” article (master/slave clock concepts and cable length recommendations).  
21. “MADI” – Encyclopedic article describing MADI’s separation of transmission and sample clocks and requirement for separate wordclock.  
22. Studio Technologies / related vendor documentation for AoIP master clocks (general PTP master roles).  
23. DirectOut “Info AES67 Stream Setup” PDF (configuring PTP grandmaster from external sync such as wordclock or MADI).  
24. Sound On Sound “Is it best to synchronise all my digital gear using a word clock generator?” (best practice for star clock distribution).  
25. Wheatstone “MADI Interfacing Considerations” support article (use of DARS vs wordclock in broadcast MADI systems).  
26. Yamaha “Time is Precious – Where did the external word clock go?” micro tutorial (PTP‑based network audio and internal wordclock).  
27. Audiodrome “Clocking (Digital Audio Sync)” article (step‑by‑step guide to master selection, wiring, termination).  
28. Reddit / audioengineering discussion (historical commentary on external clocking; not used normatively).  
29. “AES67” – Encyclopedic article noting use of IEEE 1588‑2008 PTP v2 for clock synchronisation.  
30. AIMS Alliance “Networking Fundamentals” document (PTP usage in Dante and similar systems).