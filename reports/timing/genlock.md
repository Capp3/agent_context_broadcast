---
report_id: genlock-broadcast-engineering-reference
title: Genlock in Broadcast Engineering – Technical Reference
topic: Genlock
report_version: 0.1.0
generated_date: 2026-08-27
source_access: mixed
source_cutoff: 2026-08-23
prompt_template: template-new-topic.md
prompt_template_version: 0.1.0
status: draft
---

## 1. Executive Summary

Genlock (generator locking) is the practice of forcing multiple video-related devices to align their internal scanning and frame timing to a common reference signal so that switching, keying, and mixing can occur without timing artifacts.[3][11][13][15] In traditional baseband plants this reference is usually an analog sync signal (black burst for SD or tri-level sync for HD/UHD), while in IP-based plants it is increasingly derived from a Precision Time Protocol (PTP) grandmaster via standards such as SMPTE ST 2059.[11][13][14]

Normative timing relationships for 525‑line NTSC composite video (which underpin SD black burst reference signals) are defined by SMPTE ST 170, including the horizontal frequency, vertical frequency, and color subcarrier relationships.[12] Most detailed genlock behaviors—such as how individual devices lock, what happens on loss of reference, and how analog sync relates to PTP in hybrid plants—are governed by best practice, vendor design, and profiles rather than a single, open, genlock-specific standard.[11][13][14][15]

This report maps the normative timing parameters from SMPTE ST 170 and related NTSC documentation, then layers common engineering models and practices around genlock distribution, locking behavior, and verification.[2][8][9][10][12] Where tri‑level sync waveform parameters, SMPTE ST 2059 clauses, or device-specific behaviors could not be directly accessed, they are explicitly classified as Unverified.

---

## 2. Scope and Boundaries

### 2.1 In-scope

1. Concept of genlock as used in broadcast engineering, including generator locking of video devices to common sync.[3][11][13][15]
2. Analog reference signals used for genlock in baseband plants:
   - NTSC/PAL black burst (composite video at black level including sync and color burst).[11][14]
   - Tri‑level sync for HD/UHD baseband video, as described in secondary engineering guidance.[11][14]
3. Timing relationships for 525‑line NTSC composite systems that underlie SD genlock references:
   - Horizontal line frequency, vertical frequency, and color subcarrier relationships.[2][8][9][12]
4. High-level relationship between genlock and PTP-based timing in hybrid/IP plants (SMPTE ST 2059 referenced but not accessed).[13][14]
5. Implementation-oriented guidance for modeling genlock in future AI-assisted engineering work, including validation and risk registers.[11][13][14][15]

### 2.2 Out-of-scope or Explicitly Limited

1. Detailed waveform parameters, amplitudes, tolerance ranges, and connector pinouts for tri‑level sync and other analog references, which are defined in SMPTE HD standards (e.g., SMPTE ST 274/296 series) but not available in the accessed sources (Unverified).
2. Full clause-level behavior of SMPTE ST 2059-1/-2 (PTP for media clock alignment), including exact requirements for phase alignment between PTP time and video frame boundaries (Unverified).[13][14]
3. Vendor-specific genlock implementation details (PLL loop bandwidths, recovery times, internal state machines) except where described generically in secondary sources.[11][13][15]
4. Non-broadcast uses of genlock (e.g., game consoles, VR headsets) beyond basic conceptual overlap.[3]

### 2.3 Adjacent Topics and Common Misconceptions

1. **Genlock vs timecode.** Timecode gives each frame a unique identifier (metadata), while genlock controls the physical timing of when frames are captured and output.[11] Timecode alone does not guarantee phase alignment between devices; genlock is required for synchronous switching and compositing.[5][11]
2. **Genlock vs frame synchronization.** Frame synchronizers can buffer and re-time asynchronous inputs to a synchronous plant, but they do not replace the need for genlock within the plant; they are typically used at boundaries where genlock is unavailable.[13][15]
3. **Genlock vs PTP.** PTP (per SMPTE ST 2059) is a network timing mechanism; genlock is the process of aligning media devices to a timing reference. In hybrid plants these must be coherently related to avoid phase competition between analog sync and PTP.[13][14]

### 2.4 Source Access Limitations

1. SMPTE ST 170 (NTSC composite video) was accessible only via a public PDF marked “stable 2010”; clause-level content appears limited but includes key timing relationships.[12]
2. SMPTE HD video and tri‑level sync standards (e.g., SMPTE ST 274, ST 296) and SMPTE ST 2059 were not directly accessed; references to them come from secondary engineering articles.[13][14]
3. Several secondary sources (vendor blogs, guides) provide descriptive but non-normative information on genlock behavior; these are explicitly marked as secondary and non-normative.[5][9][11][13][14][15]

---

## 3. Standards and Source Map

### 3.1 Primary and Secondary Documents

The following table summarizes key documents.

| Document | Version/date | Role | Source status | Clause-level visibility |
|---------|--------------|------|---------------|-------------------------|
| SMPTE ST 170:2004 (stable 2010) – Composite Analog Video Signal – NTSC | 2004, stable 2010[12] | Primary standard for NTSC 525-line composite video timing | Public PDF excerpt; full SMPTE publication likely paywalled | Partial; includes line frequency, subcarrier relationship, line count, interlace description (e.g., §11.2 Line frequency)[12] |
| NTSC color subcarrier and scan relationships (Analog Devices NTSC glossary) | ca. 2000s, updated 2026-07-27[2] | Secondary technical summary of NTSC timing | Public | Descriptive; no clause references but provides formulas for Fv and subcarrier relationship[2] |
| NTSC Colour TV System (Brainkart) | 2017-04-02[6] | Secondary explanatory text on NTSC design | Public | Descriptive; includes formula fsc = (2n+1)fh/2 and numeric example[6] |
| NTSC Overview (GoElectronics PDF) | date not stated, updated 2025-01-28[8] | Secondary overview of NTSC frequencies | Public | Provides explicit formulas for FH, FV, FSC and numeric values; no standard clause references[8] |
| Video Basics (Analog Devices technical article) | 2002-05-08, updated 2026-08-23[9] | Secondary overview of analog video and NTSC | Public | Descriptive; includes statement that color subcarrier is 455/2 times line rate[9] |
| TV Technology – “Will the End of NTSC Be the End of 59.94?” | 2008-01-08[10] | Secondary historical explanation of 59.94 Hz and NTSC timing | Public | Descriptive relationships between frame/field/line rates; no formal clauses[10] |
| Genlock – Wikipedia | continuously updated, last updated 2026-04-26[3] | Secondary conceptual definition of genlock | Public | Encyclopedic; no formal clauses[3] |
| Embedded.com – “Genlock gets broadcast video signal timing in sync” | 2006-06-16[15] | Secondary engineering article on genlock circuitry | Public | Descriptive; includes block diagram discussion; no formal clauses[15] |
| Meinberg – “Genlock in a networked world” | 2022-08-12[13] | Secondary article on genlock, PTP, and hybrid plants | Public | Descriptive; references SMPTE ST 2059 but no clause text[13] |
| JEM Productions – “Understanding Genlock vs Timecode: Broadcast Sync Guide” | updated 2026-06-03[11] | Secondary practical guide to genlock and timecode | Public | Descriptive; practical rules, no formal standards[11] |
| JEM Productions – “Genlock vs Timecode: Interactive Sync Visualizer & Guide” | updated 2026-03-26[5] | Secondary practical guide focusing on visual explanation | Public | Descriptive; device behavior and analogies[5] |
| AV500 – “Tri-Level Sync — Network Ports & Requirements” | 2026-05-01, updated 2026-06-24[14] | Secondary engineering guidance on tri-level sync in hybrid plants | Public | Descriptive; references SMPTE ST 2059; describes requirements for house timing authority and sync distribution[14] |
| Rediffusion London – “How it works… The Problem of Genlock” | 2023-07-12[7] | Historical engineering description of genlock between facilities | Public | Descriptive; no formal clauses[7] |
| RCA Broadcast News – Genlock device description | ca. mid‑20th century[4] | Historical description of early genlock device | Public | Descriptive; no formal clauses[4] |
| Reddit r/broadcastengineering – “Can someone explain GENLOCK to me?” | 2022-07-11[1] | Tertiary community explanation | Public | Informal; no standards[1] |

**Source confidence and visibility:**
- SMPTE ST 170 content is treated as high-confidence for NTSC timing relationships.[12]
- NTSC explanatory sources are consistent with ST 170 and each other; treated as corroborating but secondary.[2][6][8][9][10]
- All genlock behavior and tri-level sync descriptions are secondary or tertiary; no primary genlock-specific standard was accessed.[3][5][7][11][13][14][15]

---

## 4. Normative Requirements Catalog

The following table defines requirements, with IDs for later reference. “Normative” indicates direct derivation from a primary standard; “Best practice” indicates widely accepted engineering practice from secondary sources; “Assumed” indicates inferred models; “Unverified” indicates missing primary confirmation.

| ID | Requirement or rule | Applies to | Normative citation | Status | Implementation implication | Confidence |
|----|---------------------|-----------|--------------------|--------|----------------------------|-----------|
| GEN-REQ-1 | In a 525-line NTSC color system, the horizontal line frequency shall be approximately 15,734.265 Hz and related to the color subcarrier by \( f_H = \frac{455}{2} f_{SC} \). | Video signal generators, encoders, receivers using NTSC composite | SMPTE ST 170 §11.2 Line frequency; numeric relationship showing \( f_H = \frac{455}{2} f_{SC} \).[12] | Normative | Any genlock reference based on NTSC black burst must preserve this horizontal frequency and relationship, or devices may fail to lock correctly. | High |
| GEN-REQ-2 | The NTSC color subcarrier frequency shall be 3.579545 MHz and an odd multiple of half the line frequency; for NTSC, \( f_{SC} = \frac{455}{2} f_H \). | NTSC composite generators and receivers | NTSC timing summaries and design explanations.[2][6][8][9][12] | Assumed (design-derived, consistent with primary) | Incorrect subcarrier frequency or ratio will cause color instability and pattern artifacts; genlock receivers depending on color burst phase will misalign. | High |
| GEN-REQ-3 | For a 525-line system, the vertical rate shall satisfy \( f_V = f_H \times \frac{2}{525} \), yielding approximately 59.94 Hz in NTSC color systems. | NTSC composite systems and sync generators | NTSC glossaries and overview formulas.[2][8][10][12] | Assumed (derived from normative line count) | Genlock reference must produce correct vertical rate, or devices will exhibit field/frame rate mismatch and rolling or tearing. | High |
| GEN-REQ-4 | NTSC composite systems shall have 525 lines per frame with 2:1 interlace. | NTSC composite generators and receivers | SMPTE ST 170 (statement of 525 lines, 2:1 interlace).[12] | Normative | Genlock reference and devices must align on this line count and interlace structure; misalignment leads to wrong field dominance and vertical phase errors. | High |
| GEN-REQ-5 | Within a synchronous baseband plant, all participating video sources should be locked (genlocked) to a common timing reference (e.g., black burst or tri-level sync) so that their frame and line timing are phase-aligned. | Cameras, routers, switchers, graphics engines, recorders | Practical genlock guides and engineering articles.[11][13][15] | Best practice | Without genlock, switching between sources can disrupt receiver sync circuits and cause visual disturbances; internal frame synchronizers may be required per source. | Medium |
| GEN-REQ-6 | Genlock reference signals shall carry timing information only and not programme video, audio, control, or metadata. | Sync generators and distribution networks | Tri-level sync guidance and genlock/timecode guides stating reference consists of pure timing pulses.[11][14] | Best practice | Systems must not depend on genlock reference for any content or control information; separate paths must be used for video/audio/control/timecode. | Medium |
| GEN-REQ-7 | Devices that accept genlock should lock their internal clock (e.g., quartz oscillator) such that the vertical blanking interval (frame start) is phase-aligned to the reference pulses. | Cameras, switchers, graphics engines, SDI encoders | Genlock behavior descriptions in broadcast guides.[5][11][15] | Best practice | Implementation must provide PLL or equivalent circuits to align internal timing; devices that cannot lock must be treated as asynchronous sources. | Medium |
| GEN-REQ-8 | In hybrid IP/baseband plants, analog sync references (black burst/tri-level) should have an explicit relationship to the PTP domain defined by SMPTE ST 2059 to avoid competing timing authorities. | Sync system designers, facility timing architecture | Tri-level sync guidance referencing SMPTE ST 2059.[13][14] | Best practice (underlying normative ST 2059 Unverified) | Implementations should specify which timing domain is authoritative and document offsets between analog reference and PTP time; ambiguity can cause phase drift and misaligned media clocks. | Low–Medium |
| GEN-REQ-9 | Genlock systems should use correctly terminated 75‑ohm coaxial distribution, with buffered outputs or documented loop-through constraints, to maintain signal integrity. | Sync distribution infrastructure | Tri-level sync engineering guidance.[14] | Best practice | Incorrect impedance or excessive loop-through can distort the reference waveform, leading to locking instability or failure at endpoints. | Medium |
| GEN-REQ-10 | When introducing external signals (e.g., satellite receiver, camcorder) into a synchronous studio, they should be synchronized (genlocked or frame-synchronized) to the house reference before use in live switching. | Ingest paths, external feeds | Broadcast engineering article on studio genlock usage.[15] | Best practice | Unsynchronized sources may disrupt receiver sync and cause visible artifacts at transitions; frame synchronizers or dedicated genlock interfaces should be used. | Medium |

---

## 5. Engineering Model

### 5.1 Core Objects and Signals

1. **House timing authority (Sync Pulse Generator, SPG).**
   - Device or system that produces the master reference timing for the facility.[7][13][14]
   - Outputs analog sync signals (black burst, tri‑level sync) and/or PTP time according to facility design.[13][14]

2. **Genlock reference signal.**
   - For SD: composite black burst (NTSC/PAL) containing horizontal and vertical sync pulses plus color burst at black level.[11][14]
   - For HD/UHD: tri‑level sync, a bipolar analog pulse sequence used solely for timing.[11][14]
   - Carries line and frame timing but no content payload.[11][14]

3. **Genlock consumer device.**
   - Camera, production switcher, graphics engine, SDI/HDMI encoder/decoder, router, or other device with a genlock input.[5][11][13][15]
   - Contains internal oscillators and phase-locked loops (PLLs) that lock to the reference.[5][11][15]

4. **Video program signal.**
   - Baseband analog or digital video (e.g., SDI) whose timing (sampling, blanking intervals) is derived from a locked internal clock.[9][11][15]

5. **Timecode and metadata.**
   - Ancillary mechanisms that label frames with identifiers but do not impose timing; typically LTC, VITC, or embedded timecode.[11]

### 5.2 Timing and Data Flow

The genlock model can be described as a flow from timing source to devices:

```mermaid
flowchart TD
    SPG[House Sync Pulse Generator] --> RefAnalog[Analog Genlock Reference (black burst / tri-level)]
    SPG --> PTP[PTP Grandmaster (SMPTE ST 2059 domain)]
    RefAnalog --> DeviceA[Camera A PLL]
    RefAnalog --> DeviceB[Switchers, Graphics Engines]
    PTP --> IPDevices[IP Encoders / Decoders]
    DeviceA --> ProgramA[Program Video A (SDI)]
    DeviceB --> ProgramB[Program Video B (SDI)]
```

1. The SPG generates analog reference signals and, in hybrid plants, participates in or is disciplined by a PTP grandmaster.[13][14]
2. Each genlock-capable baseband device receives the analog reference and uses a PLL to align its internal line and frame timing to the reference’s pulses.[5][11][15]
3. Program video signals produced by those devices inherit the locked timing (horizontal, vertical, subcarrier relations).[9][12][15]
4. Switchers and routers assume that all connected synchronous inputs share the same line and frame phase, enabling transparent transitions without disturbing downstream receiver sync.[13][15]

### 5.3 Control-flow and State

Devices typically implement implicit states related to genlock, although detailed state machines are vendor-specific and Unverified:

- **Free-running:** Device uses its internal oscillator without external reference; output timing may drift relative to other devices.[11][13]
- **Lock acquisition:** On presence of a valid reference, PLL attempts to phase-align; depending on design, this may take multiple frames.[5][11][15]
- **Locked:** Internal timing is phase-aligned to reference; frame and line boundaries match house sync.[5][11][13]
- **Loss-of-reference:** On reference drop-out, device may hold last-known timing, revert to free-running, or mute output; behaviors are vendor-specific (Unverified).[11][13]

These behaviors define the boundary between standards-derived timing (e.g., NTSC scan parameters) and implementation policy (PLL design, lock criteria).

### 5.4 Boundary Between Standards and Policy

1. **Standards-derived:**
   - Exact frequencies and relationships for NTSC composite video.[2][8][9][10][12]
   - Line count and interlace structure (525 lines, 2:1 interlace).[12]

2. **Implementation/policy:**
   - How a device measures and validates reference presence (thresholds, noise tolerance) – Unverified.
   - Allowed phase offsets between device output and reference (e.g., preset line/field offsets) – typically configurable and not standardized.[11][14]
   - Behavior on loss-of-reference, including switching to internal timing or muting outputs.[11][13]

---

## 6. Formulas, Calculations, and Worked Examples

All formulas in this section pertain to NTSC 525-line composite video, which underlies many SD genlock references. Where possible, values are corroborated across multiple sources.

### 6.1 NTSC Line, Field, and Frame Rates

#### 6.1.1 Horizontal Line Frequency

NTSC documentation provides the horizontal line frequency \( f_H \) as:[8][12]

\[
f_H = \frac{4.5 \times 10^6}{286} \approx 15{,}734.27 \text{ Hz}
\][8][12]

Inputs:
- Audio subcarrier frequency 4.5 MHz (design parameter).[8]
- Integer constant 286 from system design constraints.[8]

This value matches the SMPTE ST 170 statement of 15,734.265 Hz.[12]

#### 6.1.2 Vertical Frequency

The vertical frequency \( f_V \) for a 525-line, 2:1 interlace system is given by:[2][8][10][12]

\[
f_V = f_H \times \frac{2}{525}
\][2][8][12]

Worked example (normative-derived):

- Given \( f_H = 15{,}734.27 \text{ Hz} \).[8][12]
- Compute \( f_V = 15{,}734.27 \times \frac{2}{525} \approx 59.94 \text{ Hz} \).[2][8][10]

This corresponds to a field rate of approximately 59.94 Hz (two interlaced fields per frame) and a frame rate of approximately 29.97 frames per second.[10]

#### 6.1.3 Frame Rate (Interlaced)

For 2:1 interlace:

\[
f_{\text{frame}} = \frac{f_V}{2}
\][10]

Using the worked example above:

- \( f_{\text{frame}} = \frac{59.94}{2} \approx 29.97 \text{ frames/s} \).[10]

This relationship is widely documented as characteristic of NTSC color systems.[8][9][10]

### 6.2 Color Subcarrier Frequency and Relationship

NTSC color systems choose the subcarrier frequency \( f_{SC} \) to be an odd multiple of half the line frequency to minimize dot pattern interference:[6][8][9]

General design formula:[6]

\[
f_{SC} = \frac{(2n + 1)}{2} f_H
\][6]

For NTSC, \( n = 227 \) (design choice), yielding:[6][8][9][12]

\[
f_{SC} = \frac{455}{2} f_H
\][2][8][9][12]

Numeric example (using SMPTE ST 170 horizontal frequency):[12]

- \( f_H = 15{,}734.265 \text{ Hz} \).[12]
- \( f_{SC} = \frac{455}{2} \times 15{,}734.265 \approx 3{,}579{,}545 \text{ Hz} \).[2][6][8]

This produces the canonical NTSC color subcarrier frequency of 3.579545 MHz, consistent across multiple sources.[2][6][8][9][12]

### 6.3 Subcarrier Cycles per Line

SMPTE ST 170 notes that there are 227.5 subcarrier cycles per video line in NTSC composite video:[12][8]

\[
\text{Cycles per line} = \frac{f_{SC}}{f_H} = \frac{\frac{455}{2} f_H}{f_H} = \frac{455}{2} = 227.5
\][8][12]

This relationship is central to color framing and ensures consistent chroma sampling relative to line timing.[8][12]

### 6.4 Formula and Assumption Register

| Calculation name | Formula or method | Inputs and units | Source citation | Status | Worked example available | Confidence |
|------------------|-------------------|------------------|-----------------|--------|--------------------------|-----------|
| NTSC horizontal frequency | \( f_H = \frac{4.5 \times 10^6}{286} \) | Audio subcarrier (Hz), design constant | NTSC overview and SMPTE ST 170 numeric values.[8][12] | Assumed (derives normative value) | Yes (15,734.27 Hz) | High |
| NTSC vertical frequency | \( f_V = f_H \times \frac{2}{525} \) | Horizontal frequency (Hz), line count | NTSC glossaries and overview formulas.[2][8][12] | Assumed (line count normative) | Yes (~59.94 Hz) | High |
| NTSC frame rate (interlaced) | \( f_{\text{frame}} = \frac{f_V}{2} \) | Vertical frequency (Hz) | Historical NTSC explanations.[10] | Assumed | Yes (~29.97 fps) | High |
| Color subcarrier design formula | \( f_{SC} = \frac{(2n + 1)}{2} f_H \) | Horizontal frequency (Hz), integer n | NTSC colour TV system design explanation.[6] | Assumed (design rationale) | Yes (n=227) | Medium |
| NTSC color subcarrier relationship | \( f_{SC} = \frac{455}{2} f_H \) | Horizontal frequency (Hz) | NTSC glossaries, overviews, and SMPTE ST 170 numeric relationship.[2][8][9][12] | Assumed (matches normative values) | Yes (3.579545 MHz) | High |
| Subcarrier cycles per line | \( \text{Cycles} = \frac{f_{SC}}{f_H} = \frac{455}{2} \) | Subcarrier and horizontal frequency | NTSC overview and SMPTE ST 170.[8][12] | Assumed (derived) | Yes (227.5 cycles/line) | High |

Note: No tri-level sync waveform formulas or amplitudes are included; these require direct access to SMPTE HD standards and are Unverified.

---

## 7. Interoperability Risks and Ambiguity Register

| Risk or ambiguity | Evidence | Normative status | Failure symptom | Mitigation or modeling rule |
|-------------------|----------|------------------|-----------------|-----------------------------|
| Mixed reference formats (black burst vs tri-level sync) with unclear support | Secondary sources note black burst as legacy SD reference and tri-level sync as HD/UHD reference, with devices accepting specific formats.[11][14] | Best practice | Devices may fail to lock or misinterpret the reference, causing unstable output timing or phase errors. | Model each device’s accepted reference types explicitly; ensure house SPG provides compatible formats or use format converters; treat unsupported references as absent. |
| Competing timing domains (analog genlock vs PTP) in hybrid plants | Articles describe need for explicit SMPTE ST 2059/PTP relationship to avoid analog reference competing with packet timing.[13][14] | Best practice (underlying normative ST 2059 Unverified) | Apparent drift or phase jumps between IP and baseband paths; lip-sync variations; switching anomalies. | Define an authoritative timing domain; document offsets between analog reference and PTP time; ensure one domain is derived from the other (e.g., SPG disciplined by PTP). |
| Unsynchronized external sources entering a synchronous studio | Broadcast article explains that signals from satellite receivers, camcorders, etc. must be synchronized to studio signals via genlock circuits.[15] | Best practice | Viewer receiver sync circuits are disturbed on transitions; visible tearing, jumps, or loss of lock. | Treat external feeds as asynchronous; pass through frame synchronizers or genlock-capable interfaces; require synchronization before live switching. |
| Device behavior on loss of genlock reference | Guides mention genlock circuits but do not detail standard behavior when reference is lost.[11][13][15] | Unverified | On reference failure, devices may free-run, hold last phase, or mute; plant behavior becomes unpredictable. | Model loss-of-reference behavior per device documentation; implement monitoring to detect reference absence; design failover policies (e.g., switch to backup SPG, alarm operators). |
| Phase offsets and field dominance mismatches | NTSC interlace and line relationships are normative, but allowable phase offsets between sources are not standardized.[10][12] | Unverified | Misaligned field dominance can cause judder or artifacts in interlaced systems; inaccurate keying or layering. | Establish facility phase conventions (e.g., field dominance) and configure devices accordingly; measure output phase against house reference and enforce limits. |
| Lack of explicit genlock specification | Genlock is described conceptually in secondary sources and Wikipedia without a dedicated primary standard.[3][11][13][15] | Unverified | Implementations differ in lock criteria, jitter tolerance, and reporting; AI-assisted tools may misassume behavior. | Treat genlock as an implementation concept; rely on primary video timing standards plus device documentation; avoid assuming uniform genlock behavior across vendors. |
| Misuse of genlock reference for content or control | Tri-level sync guidance states that reference carries timing only.[14][11] | Best practice | Attempting to embed content or metadata in genlock path could confuse devices and risk lock loss. | Segregate timing references from content/control channels; represent genlock as a pure timing primitive in system models. |

---

## 8. Implementation Guidance

All guidance in this section is derived from secondary sources and treated as Best Practice unless stated otherwise.

### 8.1 Modeling Genlock in Systems and AI Contexts

1. Represent **genlock reference** as a distinct signal type with properties:
   - Format (black burst, tri‑level sync).[11][14]
   - Nominal line, field, and frame rates (from NTSC or HD standards).[2][8][9][12]
   - Accepted by specific device inputs (boolean capabilities per device).[11][14]

2. Represent **devices** with genlock-related attributes:
   - `supports_genlock`: true/false.
   - `supported_reference_formats`: list of formats (e.g., NTSC black, 1080i tri-level) – Unverified; populate from vendor data.
   - `lock_status`: states (Free-running, Lock-acquiring, Locked, Loss-of-reference).[11][13][15]
   - `timing_source`: enum (Internal, Analog reference, PTP).

3. For **IP-based timing**, model PTP as providing time stamps and media clock alignment per SMPTE ST 2059 (normative details Unverified).[13][14]

### 8.2 Recommended Checks and House Practices

1. **House reference integrity checks:**
   - Verify that the SPG outputs the expected format (black burst or tri-level) with nominal rates per NTSC or HD system design.[2][8][9][12]
   - Confirm correct 75‑ohm termination and signal distribution via buffered outputs or controlled loop-through.[14]

2. **Device configuration practices:**
   - Ensure all synchronous plant devices that support genlock are configured to use the same reference format from the same SPG.[11][13][14]
   - For devices that cannot genlock, plan frame synchronizers or asynchronous switching modes.[13][15]

3. **Hybrid plant timing practices:**
   - Maintain a clear mapping between analog sync and PTP time; e.g., SPG disciplined by PTP or PTP derived from SPG.[13][14]
   - Avoid configuring devices to follow conflicting timing sources without defined precedence.

4. **Operational practices:**
   - Treat genlock failures as critical alarms; they affect plant-wide timing.[13][14]
   - When integrating new sources (satellite, remote feeds), verify synchronization before including them in live switching paths.[15]

### 8.3 Modeling Unverified Values

1. **Tri-level sync waveform parameters (amplitude, pulse width, rise/fall times).**
   - Mark as Unverified in models; create placeholders like `tri_level_amp` without specific numeric values.
   - Bind actual values from vendor specs or SMPTE HD standards when accessible.

2. **Device-specific lock threshold and jitter tolerance.**
   - Model as ranges (e.g., `max_jitter_ps`) without filling numeric values unless device documentation is available.

3. **Loss-of-reference behavior.**
   - Represent as device policies (`hold_phase`, `switch_internal`, `mute_output`) and mark them as implementation-dependent until confirmed.[11][13]

---

## 9. Validation Checklist

The following checklist is intended for engineering validation or AI-assisted review of genlock configurations. All items are implementation guidance (Best Practice) derived from secondary sources.

1. Confirm that the facility has a declared **house timing authority** (SPG and/or PTP grandmaster).[13][14]
2. Verify that the SPG output format (black burst, tri-level) matches the system’s baseband video format (e.g., NTSC SD, HD 1080).[11][14]
3. Check that the SPG’s NTSC-derived timing (for SD) matches:
   - \( f_H \approx 15{,}734.27 \text{ Hz} \).[8][12]
   - \( f_V \approx 59.94 \text{ Hz} \).[2][8][10][12]
   - \( f_{SC} \approx 3.579545 \text{ MHz} \).[2][6][8][12]

4. Verify that genlock reference distribution uses 75‑ohm coaxial cabling with correct termination and buffered outputs or controlled loop-through.[14]
5. For each device:
   - Confirm it supports genlock and the specific reference format offered.[5][11][13][14]
   - Confirm it is configured to use the house reference rather than free-running or an alternate source.[11][13]

6. Confirm that all synchronous inputs to production switchers share the same timing domain and are locked (or frame-synchronized if they cannot lock).[13][15]
7. In hybrid plants, verify that analog reference and PTP share a defined relationship, with one acting as authoritative and phase offsets documented.[13][14]
8. Implement monitoring for:
   - Reference presence and amplitude (numeric thresholds Unverified; compare to vendor specs).
   - Device lock status (e.g., LEDs, SNMP, control reports).[13][14]

9. Prior to live events, perform test switching between sources to confirm there are no visible sync disruptions or artifacts.[13][15]

---

## 10. Open Questions / Unverified Items

The following items could not be verified against primary standards in the accessed material and must be treated as Unverified until supported by additional sources:

1. **Tri-level sync waveform parameters and tolerances:**
   - Exact amplitude, pulse shape, timing tolerances, and polarity are defined in SMPTE HD standards (e.g., SMPTE ST 274/296) but those documents were not accessed.

2. **SMPTE ST 2059 clause-level requirements:**
   - Specific mandates for mapping PTP time to video frame boundaries and acceptable phase alignment ranges are Unverified; only the existence of ST 2059 and its role in PTP for media was noted.[13][14]

3. **Standardized device behavior on genlock loss:**
   - No primary standard describing required behavior (e.g., holdover, muting) was found; behavior appears vendor-specific and policy-driven.[11][13][15]

4. **Formal genlock standard:**
   - No dedicated SMPTE or ITU standard titled specifically for “genlock” was identified; genlock appears as a technique implemented relative to video timing standards.[3][11][13][15]

5. **Exact mapping of genlock reference formats to specific HD/UHD video formats:**
   - While secondary sources state that tri-level sync is used for HD/UHD, they do not detail the full matrix of which sync format applies to which resolution/frame rate.[11][14]

6. **Numeric jitter and phase tolerance requirements for lock:**
   - No primary numeric limits for allowable jitter or phase error in genlock locking behavior were found; these likely exist in SMPTE or device specifications not accessed.

---

## 11. Sources

Numbers here correspond to citation indices used throughout the report.

1. Community explanation of genlock in a broadcast engineering context (Reddit r/broadcastengineering, “Can someone explain GENLOCK to me?”, 2022-07-11).
2. Analog Devices, “NTSC” glossary entry, providing subcarrier frequency and horizontal/vertical rate relationships (updated 2026-07-27).
3. Encyclopedia article “Genlock” describing generator locking and its use in synchronizing multiple video sources (Wikipedia, last updated 2026-04-26).
4. Historical description of an RCA genlock device in RCA Broadcast News, discussing automatic lock between television pickup systems (mid‑20th century).
5. JEM Productions, “Genlock vs Timecode: Interactive Sync Visualizer & Guide,” explaining genlock pulses, camera shutter alignment, and differences from timecode (updated 2026-03-26).
6. “NTSC Colour TV System” (Brainkart, 2017-04-02), explaining NTSC subcarrier design, including \( f_{SC} = (2n+1) f_H / 2 \) and numeric example.
7. Rediffusion London, “How it works… The Problem of Genlock,” describing house SPG locked to remote generator and facility-wide genlock behavior (2023-07-12).
8. “NTSC Overview” PDF (GoElectronics), giving explicit formulas and numeric values for \( f_H \), \( f_V \), and \( f_{SC} \), including \( f_H = 4.5\times10^6/286 \) and \( f_{SC} = (455/2) f_H \).
9. Analog Devices, “Video Basics,” describing analog video fundamentals including NTSC color subcarrier being 455/2 times the horizontal line rate (2002-05-08, updated 2026-08-23).
10. TV Technology article “Will the End of NTSC Be the End of 59.94?”, explaining historical reasons for 15,734 Hz line rate, 59.94 Hz field rate, and 29.97 fps frame rate (2008-01-08).
11. JEM Productions, “Understanding Genlock vs Timecode: Broadcast Sync Guide,” detailing the distinction between genlock (phasing) and timecode (metadata), and use of tri-level sync and black burst (updated 2026-06-03).
12. SMPTE ST 170:2004 (stable 2010), “Composite Analog Video Signal – NTSC,” providing normative NTSC timing including line frequency, subcarrier relationship, 525 lines, and 2:1 interlace (§11.2 and related clauses).
13. Meinberg, “Genlock in a networked world,” describing genlock as locking devices to various timing signals and its relationship to PTP in networked environments (2022-08-12).
14. AV500, “Tri-Level Sync — Network Ports & Requirements,” describing tri-level sync as a bipolar analog reference, required infrastructure (75‑ohm coax, documented offsets), and its relationship to SMPTE ST 2059 in hybrid plants (2026-05-01).
15. Embedded.com, “Genlock gets broadcast video signal timing in sync,” explaining genlock use in studios, impact on receiver synchronization, and block diagram of a genlock circuit taking SDI input and analog reference (2006-06-16).
