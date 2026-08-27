```yaml
---
report_id: sdi-video-signal-technical-reference
title: SDI Video Signal Technical Reference for Broadcast Engineering
topic: SDI Video Signal
report_version: 0.1.0
generated_date: 2026-08-27
source_access: mixed
source_cutoff: 2026-08-27
prompt_template: template-new-topic.md
prompt_template_version: 0.1.0
status: draft
---
```

## Executive Summary

Serial Digital Interface (SDI) is a family of SMPTE-standardized uncompressed digital video interfaces that carry 10‑bit component video and ancillary data over a single serial link, typically coaxial cable.[2][13] Within this family, SD‑SDI (SMPTE 259M), HD‑SDI (SMPTE 292M), 3G‑SDI (SMPTE 424M), and 12G‑SDI (SMPTE ST 2082) define nominal bit rates from 270 Mbit/s up to 12 Gbit/s for common broadcast formats such as 480i, 720p, 1080i/p, and 2160p.[2][6][9][13]

Normative detail on the packetization and placement of ancillary (ANC) data in SDI is specified by SMPTE ST 291 and by ITU‑R BT.1614 and BT.1619, while full clause‑level text of the core transport standards (SMPTE 259M, 292M, 424M, 2082) was not available in the accessed sources and is therefore treated as unverified in this report.[1][3][5][8]

---

## 1. Scope and Boundaries

### 1.1 What SDI Standardizes

1. SDI defines families of serial digital video interfaces for broadcast‑grade uncompressed video, with specific nominal data rates and supported video formats for each standard.[2][6][9][13]  
2. SMPTE 259M defines SD‑SDI for 10‑bit 4:2:2 component and composite digital signals at bit rates such as 270 Mbit/s, 360 Mbit/s, 143 Mbit/s, and 177 Mbit/s, covering formats like 480i and 576i.[2][6][13]  
3. SMPTE 292M defines HD‑SDI at 1.485 Gbit/s and 1.485/1.001 Gbit/s for formats including 720p and 1080i HDTV.[2][6][13]  
4. SMPTE 424M defines 3G‑SDI at 2.970 Gbit/s and 2.970/1.001 Gbit/s, sufficient for 1080p50/60 and certain dual‑link 4K mappings.[2][9][13]  
5. SMPTE ST 2082 defines 12G‑SDI around 12 Gbit/s for 2160p60 single‑link carriage.[6]  
6. SMPTE ST 291 defines ancillary data packet and space formatting for digital video interfaces including SMPTE 259M and 292M, specifying that ancillary data is carried as 10‑bit words and defining packet structures.[1][3]  
7. ITU‑R BT.1614 defines a 4‑byte video payload identifier and specifies how it is embedded in ancillary data packets formatted according to SMPTE 291M, including sample positions, line numbers, and repetition rates for different SDI interfaces.[8]  
8. ITU‑R BT.1619 defines the mapping of vertical ancillary data services into the ANC spaces of SMPTE 259M and SMPTE 292M signals and describes type‑2 ANC packet formats consistent with SMPTE 291M.[5]  

### 1.2 What SDI Does Not Standardize

1. SDI does not standardize video compression codecs; common compressed formats (e.g., MPEG‑2, H.264) are transported over other interfaces or encapsulations.[2]  
2. SDI does not define IP‑based transport mechanisms; SMPTE ST 2110 and related standards define uncompressed video over IP and reference SMPTE 291 for ancillary data structure rather than SDI electrical transport.[12]  
3. SDI standards do not specify application semantics of ancillary payloads (e.g., timecode meaning, closed caption syntax); these are defined in separate standards that specify DID/SDID allocations and payload formats.[3][12]  

### 1.3 Adjacent Standards and Profiles

1. ITU‑R BT.656 is an earlier parallel digital video interface standard that, together with SMPTE 259M, defines broadcast‑grade digital video interfaces and is conceptually adjacent to SDI but not itself serial SDI.[2]  
2. SMPTE ST 291 ancillary data packets are reused by SMPTE ST 2110‑40 for ANC data over IP, with each ancillary sub‑stream identified by DID/SDID pairs, illustrating continuity of ANC formats across SDI and IP domains.[1][12]  
3. SMPTE RP 214 and SMPTE ST 375 describe practices and mappings for carrying SMPTE KLV metadata and vertical ANC data in SMPTE 259M and 292M video signals, relying on SMPTE 291 ANC formatting.[3][4]  

### 1.4 Source Access Limitations

1. Full clause‑level texts of SMPTE 259M, SMPTE 292M, SMPTE 424M, and SMPTE ST 2082 were not available in the accessed material; only secondary descriptions (e.g., Wikipedia and vendor documents) provided bit rates, example formats, and general roles.[2][6][9][10][11][13]  
2. SMPTE ST 291 (ancillary data packet and space formatting) was available as a 2006 edition PDF, providing specific clause‑level definitions for ANC data words and structures.[1]  
3. ITU‑R Recommendations BT.1614 and BT.1619 are publicly accessible and provide normative description of payload identifiers and vertical ANC mappings for SDI interfaces.[5][8]  
4. Some detailed engineering aspects—such as scrambling polynomials, CRC calculation details, exact line ranges for ANC spaces, and electrical tolerances—could not be verified from primary SDI transport standards and are therefore marked unverified in this report.  

---

## 2. Standards and Source Map

### 2.1 Standards and Source Map Table

| Document | Version/date | Role | Source status | Clause-level visibility |
|---------|--------------|------|---------------|-------------------------|
| SMPTE ST 259M (Television — 10‑Bit 4:2:2 Component and 4fsc Composite Digital Signals) | 1997 (cited) | Core SD‑SDI serial digital interface (SD video) defining 10‑bit 4:2:2 and composite signals at multiple bit rates.[2][3][6] | Primary SDI transport standard; full text not seen in accessed sources (license/availability unverified). | Low (only title, scope, and role inferred from secondary citations).[2][3][6] |
| SMPTE ST 292M (Television — 1.5 Gb/s Serial Digital Interface) | 1998 (cited) | Core HD‑SDI interface defining 1.485 Gbit/s and 1.485/1.001 Gbit/s for HD formats.[2][3][6][10][11][13] | Primary SDI transport standard; full text not seen; role described in secondary sources. | Low (bit rates and formats from secondary sources only).[2][6][13] |
| SMPTE ST 424M (3 Gb/s Serial Digital Interface) | 2006 (cited) | 3G‑SDI interface extending 259M/292M/344M to 2.970 Gbit/s and 2.970/1.001 Gbit/s for 1080p50/60 and dual‑stream formats.[2][9][13] | Primary SDI transport standard; full text not seen; summarized in secondary sources.[9] | Low (bit rates and level A/B definitions from secondary sources).[9] |
| SMPTE ST 2082 (12 Gb/s Single‑Link) | 2015 (cited) | 12G‑SDI interface at ~12 Gbit/s, supporting 2160p60 over single link.[6] | Primary SDI transport standard; full text not seen; coverage in secondary engineering article.[6] | Low (bit rate and example formats from secondary material).[6] |
| SMPTE ST 291 (Television — Ancillary Data Packet and Space Formatting) | 2006 edition (accessed) | Defines ANC data packet structure and ANC space formatting for SMPTE digital video interfaces, including SDI.[1][3][5][8][12] | Primary normative standard; full PDF accessed.[1] | Medium (specific clauses visible; only some are quoted directly here, others unverified). |
| SMPTE RP 214 (Recommended Practice for KLV in ANC) | 2002 (stable 2007) | Recommended practice for packing SMPTE KLV metadata into SMPTE 291 ANC packets within SMPTE 259M/292M video signals.[3] | Recommended practice; full text accessed.[3] | Medium (structure and references visible; detailed KLV mapping beyond scope). |
| SMPTE ST 375 | 2003 (stable 2010) | Specifies mapping of vertical ancillary data packets into DV payload area; references SMPTE 291 ANC packet format.[4] | Primary DV/ANC mapping standard; full text accessed.[4] | Low‑medium (used only for cross‑reference of 291 usage). |
| ITU‑R BT.1614 | 2003 | Defines 4‑byte video payload identifier and its placement into SMPTE 291 ANC packets in various digital interfaces (including SDI).[8] | Primary ITU standard; full text accessed.[8] | Medium (payload ID structure and placement clauses visible). |
| ITU‑R BT.1619 | 2003 | Defines vertical ancillary data mapping for serial digital interfaces based on SMPTE 259M and 292M, using SMPTE 291 ANC packet format.[5] | Primary ITU standard; full text accessed.[5] | Medium (VANC mapping and packet format clauses visible). |
| Wikipedia “Serial digital interface” (EN) | Updated 2026‑07‑16 | Secondary overview of SDI family, listing SMPTE standards, bitrates, and example formats.[2] | Secondary descriptive source; not normative.[2] | Medium (good summary, but no clause‑level normative text). |
| Wikipedia “SMPTE 424M” (EN) | Last updated 2025‑12‑11 | Secondary summary of 3G‑SDI including bit rates and Level A/B carriage descriptions.[9] | Secondary descriptive source.[9] | Medium (specific technical details but no original clauses). |
| Wikipedia “Serial Digital Interface” (DE) | Updated 2026‑06‑02 | Secondary German‑language summary of SDI standards and bitrates.[13] | Secondary descriptive source.[13] | Medium (parallel data to EN article). |
| Lattice Semiconductor “Multi‑Rate Serial Digital Interface PHY Layer” | Accessed 2026 | Vendor IP core documentation listing supported SMPTE SDI‑related standards.[10] | Secondary implementation guidance; cites SMPTE standard numbers.[10] | Low (role: confirm standard names and multi‑rate use). |
| National Semiconductor LMH0031 Deserializer Datasheet | Accessed 2026 | Vendor IC datasheet noting support for SMPTE 259M/344M/292M data rates.[11] | Secondary implementation guidance for receiver devices.[11] | Low (role: confirm typical electrical usage ranges). |
| Xilinx XAPP299 “SDI: Ancillary Data and EDH Processors” | 2003 app note | Vendor application note referencing SMPTE 291 ancillary data format.[7] | Secondary guidance on ANC handling.[7] | Low (high‑level description only). |
| EBU “ancillary_data.md” (pi‑list) | 2017 | Secondary documentation for ST 2110‑40 ANC payloads using SMPTE 291 DID/SDID identification.[12] | Secondary mapping from SDI ANC to IP ANC.[12] | Low‑medium (good for DID/SDID usage patterns). |
| CSDN blog “SMPTE 291M Packet” | 2025 | Secondary explanation of ANC packet structure with 10‑bit words, ADF, DID, SDID, DC, UDW, CS and parity.[14] | Secondary technical tutorial referencing SMPTE 291M.[14] | Low (used to cross‑check packet structure). |
| IEEE 802.1 contribution “uncomp video timing requirements status” | 2009 | Secondary slide mentioning wide‑band jitter limits in unit intervals for serial video.[15] | Secondary high‑level jitter requirement example.[15] | Low (no full context; used cautiously). |

---

## 3. Normative Requirements Catalog

The table below catalogues key requirements. “Normative” indicates backed by primary standards (SMPTE, ITU); “Best practice” indicates recommended but not mandated by primary sources accessed; “Assumed” indicates engineering inference; “Unverified” indicates no primary textual basis was seen.

### 3.1 Requirements Table

| ID | Requirement or rule | Applies to | Normative citation | Normative / best practice / assumed / unverified | Implementation implication | Confidence |
|----|---------------------|-----------|--------------------|--------------------------------------------------|----------------------------|-----------|
| SDI‑REQ‑001 | Ancillary data in SDI interfaces shall be carried as 10‑bit words.[1] | SDI senders, receivers, processors using SMPTE 291 ANC | SMPTE ST 291:2006, clause 3.1.3 (“Ancillary data is defined as 10‑bit words”).[1] | Normative | All ANC encoding/decoding logic must treat each ANC element (ADF, DID, etc.) as a 10‑bit word, aligned with the SDI 10‑bit data path.[1][14] | High |
| SDI‑REQ‑002 | ANC packets used for vertical ancillary data mapping shall conform to SMPTE 291M type‑2 packet structure consisting of ADF, DID, SDID, DC, user data words, and checksum.[5] | SDI senders and receivers implementing vertical ancillary (VANC) services | ITU‑R BT.1619, clauses describing type‑2 ANC packets referencing SMPTE 291M.[5] | Normative | Hardware/firmware must generate/parse packets with these fields in the specified order; malformed packets risk loss of VANC services.[5][14] | High |
| SDI‑REQ‑003 | In SMPTE 259M/ITU‑R BT.125M interfaces where chrominance and luminance data are carried in a single stream, all data services shall be carried in that stream with a single ANC data flag and CRC.[5] | SD‑SDI senders/receivers in single‑stream interfaces | ITU‑R BT.1619 clauses describing ANC data flag and CRC usage in 259M/125M signals.[5] | Normative | Implementations must not split ANC data across multiple parallel streams in these interfaces; ANC flag and CRC must be unique per stream.[5] | High |
| SDI‑REQ‑004 | Each data packet in the VANC mapping shall follow SMPTE 291M format with ANC data flag, DID, SDID, DC, UDW, and checksum.[5] | VANC packet generators and parsers | ITU‑R BT.1619, type‑2 packet description.[5] | Normative | VANC services (e.g., captions, timecode) must be encapsulated in packets matching this structure for proper decoding across devices.[5][1] | High |
| SDI‑REQ‑005 | Digital television interfaces may add a 4‑byte payload identifier in an ancillary data packet, placed according to SMPTE 291M and with defined sample positions and repetition rate per interface.[8] | SDI senders and receivers implementing payload identification | ITU‑R BT.1614, payload identifier definition and placement rules.[8] | Normative | Implementations that use payload IDs must place them on specified lines/samples and repeat at prescribed rates; misplacement can break automatic format detection.[8] | High |
| SDI‑REQ‑006 | SMPTE RP 214 describes a means for packing SMPTE KLV metadata into SMPTE 291 ANC packets for transport over SMPTE 259M/292M video signals.[3] | Systems transporting KLV metadata over SDI | SMPTE RP 214:2002.[3] | Best practice | Use KLV‑in‑ANC as per RP 214 to ensure interoperability of metadata carriage; alternative methods may not interoperate.[3][7] | Medium |
| SDI‑REQ‑007 | Vertical ancillary data packets may be mapped into DV payload areas as specified in SMPTE ST 375, using SMPTE 291 ANC packet structure.[4] | DV recorders/players interoperating with SDI ANC | SMPTE ST 375:2003.[4] | Normative (for DV domain) | Proper DV/SDI gateways must preserve ANC packets exactly to maintain metadata such as timecode, captions.[4] | Medium |
| SDI‑REQ‑008 | Serial digital interfaces SD‑SDI, HD‑SDI, 3G‑SDI, and 12G‑SDI shall operate at nominal bit rates as defined in their respective SMPTE standards (e.g., 270 Mbit/s, 1.485 Gbit/s, 2.970 Gbit/s, 12 Gbit/s).[2][6][9][13] | SDI transmitters and receivers | SMPTE 259M/292M/424M/2082 (cited via secondary sources).[2][6][9][13] | Unverified (primary text not seen) | PLLs, serializers/deserializers, and equalizers must be designed to these nominal rates and tolerances; mismatched rates will prevent lock.[10][11] | Medium |
| SDI‑REQ‑009 | ST 2110‑40 embeds ancillary payload according to SMPTE 291M where each ancillary sub‑stream is identified by a DID/SDID pair.[12] | IP‑based video senders/receivers referencing SDI ANC formats | EBU ancillary_data.md summarizing ST 2110‑40 behavior.[12] | Best practice (secondary description of ST 2110‑40) | IP/SDI gateways should preserve DID/SDID semantics to allow common ANC parsers across SDI and IP domains.[1][12] | Medium |
| SDI‑REQ‑010 | Ancillary data shall not interfere with active video samples; it is carried in defined ancillary spaces (horizontal and vertical blanking) associated with SMPTE 259M/292M.[3][5] | SDI encoders/decoders managing blanking and ANC insertion | SMPTE ST 291 (ANC space formatting) and ITU‑R BT.1619 VANC mapping references to 259M/292M ANC spaces.[1][3][5] | Normative (for ANC positioning) | Incorrect placement of ANC words in active video corrupts video; receivers may treat such words as picture data.[1][5] | Medium |

---

## 4. Engineering Model

### 4.1 Core Objects and Layers

1. **Physical layer**: SDI is carried over a single serial link, typically 75 Ω coaxial cable, at specified nominal bit rates per standard (e.g., 270 Mbit/s for SD‑SDI, 1.485 Gbit/s for HD‑SDI, 2.970 Gbit/s for 3G‑SDI, 12 Gbit/s for 12G‑SDI).[2][6][9][13] (Unverified for electrical parameters; only bit rates and usage described.)  
2. **Logical data stream**: The SDI stream contains a continuous sequence of 10‑bit words representing active video samples, timing reference words, and ancillary data words.[1][2][13] (Video word semantics are inferred from secondary sources; 10‑bit ANC words are normative per ST 291.)[1]  
3. **Video payload**: For component interfaces, video is carried in YCbCr 4:2:2 format with specified mapping of luma and chroma samples to the 10‑bit word stream; this is described generically for SDI in secondary sources but not clause‑verified here.[2][13]  
4. **Ancillary spaces**: SMPTE 291 and ITU‑R BT.1619 define horizontal ancillary (HANC) and vertical ancillary (VANC) spaces within the line/field structure of SMPTE 259M and 292M signals where ancillary packets can be inserted without affecting visible video.[1][3][5]  
5. **Ancillary packets**: Each ANC packet is composed of multiple 10‑bit words: an Ancillary Data Flag (ADF), followed by Data ID (DID), Secondary Data ID (SDID), Data Count (DC), one or more User Data Words (UDW), and a checksum (CS).[5][14] (UDW payload semantics are delegated to other standards.)  

### 4.2 Data‑Flow Relationships

1. In SD‑SDI and HD‑SDI, ANC packets are interleaved into the serial word stream within designated HANC and VANC regions; packets must begin on specific word boundaries and lines to conform to SMPTE 291 space formatting.[1][3][5]  
2. ITU‑R BT.1619 describes how multiple data services (e.g., teletext, closed captions) are mapped into the vertical ancillary space by assigning specific lines and using SMPTE 291 packets.[5]  
3. ITU‑R BT.1614 defines a video payload identifier that is carried as an ANC packet to enable automatic identification of the video format across diverse digital interfaces, including SDI.[8]  

### 4.3 Control‑Flow and Identification

1. DID/SDID pairs uniquely identify the type of ANC payload (e.g., timecode, captions, payload ID) within the SDI stream; these IDs are interpreted by receivers according to application‑specific standards.[5][8][12]  
2. The payload identifier defined in ITU‑R BT.1614 is a specific DID/SDID‑based ANC packet repeated at defined intervals; receivers inspect this packet to auto‑detect video format and payload characteristics.[8]  

### 4.4 Boundary Between Standards and Implementation Policy

1. SMPTE standards and ITU‑R recommendations define what constitutes a valid SDI/ANC signal: bit rates, word sizes, packet fields, and permitted ANC spaces.[1][2][5][8]  
2. Implementation policies—such as buffer sizes, internal word representations (e.g., 10‑bit packed vs. 16‑bit containers), and error‑handling strategies for malformed ANC packets—are not standardized and must be chosen by implementers.[7][10][11]  
3. Recommended practices like SMPTE RP 214 guide implementers in structuring KLV metadata carriage but do not forbid alternative, non‑interoperable solutions.[3]  

### 4.5 Conceptual Flow Diagram

```mermaid
flowchart TD
    sdiFamily[SDI Transport Standards<br/>(259M, 292M, 424M, 2082)] --> ancStandard[ANC Formatting<br/>SMPTE ST 291]
    ancStandard --> ituAnc[ITU-R BT.1619<br/>VANC Mapping]
    ancStandard --> ituPid[ITU-R BT.1614<br/>Payload Identifier]
    ancStandard --> rp214[SMPTE RP 214<br/>KLV in ANC]
    sdiFamily --> interfaceImpl[SDI Tx/Rx Implementation]
    ancStandard --> interfaceImpl
    ituAnc --> interfaceImpl
    ituPid --> interfaceImpl
    rp214 --> interfaceImpl
```

The diagram is conceptual and does not introduce new technical facts beyond the cited relationships.[1][3][5][8]

---

## 5. Formulas, Calculations, and Worked Examples

This section focuses on simple, implementation‑useful calculations derived from known SDI bit rates and general digital signaling mathematics. These formulas are **assumed engineering relationships**, not explicitly standardized, and are marked as non‑normative.

### 5.1 Formula and Assumption Register

| Calculation name | Formula or method | Inputs and units | Source citation | Normative or assumed | Worked example available | Confidence |
|------------------|-------------------|------------------|-----------------|----------------------|--------------------------|-----------|
| SDI bit period from nominal bit rate | \( T_{\text{bit}} = \frac{1}{R_{\text{bit}}} \) | \(R_{\text{bit}}\): nominal SDI serial bit rate (bit/s); output \(T_{\text{bit}}\): bit period (s). | Bit rates from SDI descriptions (e.g., 270 Mbit/s, 1.485 Gbit/s, 2.970 Gbit/s).[2][6][9][13] | Assumed (basic digital communications math; not explicitly stated in SDI standards). | Yes | Medium |
| Approximate ANC bandwidth overhead | \( \text{ANC fraction} = \frac{N_{\text{ANC\_words}}}{N_{\text{total\_words}}} \) per frame | \(N_{\text{ANC\_words}}\): ANC words per frame; \(N_{\text{total\_words}}\): total SDI words per frame; both dimensionless counts. | ANC words defined as 10‑bit words in SMPTE ST 291.[1] | Assumed (ratio; standards define words but not this formula). | Yes (symbolic) | Medium |
| Jitter in seconds from unit intervals | \( J_{\text{time}} = J_{\text{UIpp}} \times T_{\text{bit}} \) | \(J_{\text{UIpp}}\): jitter in unit intervals peak‑to‑peak (dimensionless); \(T_{\text{bit}}\): bit period (s); output \(J_{\text{time}}\): jitter (s). | Use of UIpp jitter units in IEEE serial video timing discussion.[15] | Assumed (conversion; original slide mentions UIpp but not this explicit formula).[15] | Yes | Low |

### 5.2 Worked Examples (Assumed, Non‑Normative)

#### 5.2.1 Bit Period for Common SDI Rates

Using \( T_{\text{bit}} = \frac{1}{R_{\text{bit}}} \)[2][6][9][13]:

1. SD‑SDI at 270 Mbit/s (SMPTE 259M, typical NTSC/PAL SD):  
   - \( R_{\text{bit}} = 270 \times 10^{6} \text{ bit/s} \).  
   - \( T_{\text{bit}} = \frac{1}{270 \times 10^{6}} \approx 3.7037 \times 10^{-9} \text{ s} \).  
   - Bit period ≈ 3.70 ns (assumed).  

2. HD‑SDI at 1.485 Gbit/s (SMPTE 292M, 1080i/720p):  
   - \( R_{\text{bit}} = 1.485 \times 10^{9} \text{ bit/s} \).[2][6][13]  
   - \( T_{\text{bit}} = \frac{1}{1.485 \times 10^{9}} \approx 6.737 \times 10^{-10} \text{ s} \).  
   - Bit period ≈ 0.674 ns (assumed).  

3. 3G‑SDI at 2.970 Gbit/s (SMPTE 424M, 1080p50/60):  
   - \( R_{\text{bit}} = 2.970 \times 10^{9} \text{ bit/s} \).[2][9][13]  
   - \( T_{\text{bit}} = \frac{1}{2.970 \times 10^{9}} \approx 3.367 \times 10^{-10} \text{ s} \).  
   - Bit period ≈ 0.337 ns (assumed).  

4. 12G‑SDI at 12 Gbit/s (SMPTE ST 2082, 2160p60):  
   - \( R_{\text{bit}} = 12 \times 10^{9} \text{ bit/s} \).[6]  
   - \( T_{\text{bit}} = \frac{1}{12 \times 10^{9}} \approx 8.333 \times 10^{-11} \text{ s} \).  
   - Bit period ≈ 0.083 ns (assumed).  

These calculations rely on listed bit rates from secondary sources and generic math; they are not normative constraints.[2][6][9][13]

#### 5.2.2 Example Jitter Conversion (Assumed)

If a design specification uses a wide‑band jitter limit of 0.2 UIpp (unit intervals peak‑to‑peak), as mentioned in serial video timing discussions,[15] then for HD‑SDI at 1.485 Gbit/s:

- Bit period \( T_{\text{bit}} \approx 0.674 \text{ ns} \) (from above, assumed).[2][6][13]  
- Jitter time \( J_{\text{time}} = 0.2 \times 0.674 \text{ ns} \approx 0.135 \text{ ns} \) peak‑to‑peak (assumed).  

This demonstrates how UIpp specifications can be converted into time, but does not represent an official SDI jitter limit from a primary standard.[15]

---

## 6. Interoperability Risks and Ambiguity Register

### 6.1 Risk and Ambiguity Table

| Risk or ambiguity | Evidence | Normative status | Failure symptom | Mitigation or modeling rule |
|-------------------|----------|------------------|-----------------|-----------------------------|
| Unverified core transport details (scrambling, CRC polynomials, exact ANC line ranges) | Primary texts of SMPTE 259M/292M/424M/2082 not accessed; only secondary summaries of bit rates and formats available.[2][6][9][10][11][13] | Unverified | Devices might implement incompatible scrambling or CRC schemes, leading to non‑interoperable SDI streams despite matching bit rates. | Treat transport‑layer details as unknown; rely on proven reference implementations or licensed copies of SMPTE standards for design, and validate against industry equipment. |
| Misplacement of VANC packets | ITU‑R BT.1619 defines VANC mapping in terms of specific line ranges and ANC spaces, but full line tables are outside the accessed snippets.[5] | Normative mapping partially visible; some details unverified | Receivers fail to detect certain services (e.g., captions) if packets appear on unexpected lines, or video artifacts occur if ANC appears in active video. | Follow ITU‑R BT.1619 and SMPTE 291 for ANC line selection; treat any deviation as non‑standard and perform interoperability tests with target equipment.[1][5] |
| DID/SDID mis‑assignment | EBU ancillary_data.md and ITU‑R BT.1614 rely on correct DID/SDID use for payload IDs and other ANC streams, but not all DID/SDID allocations are visible here.[8][12] | Partially normative (ITU), partially application‑specific | Payload type confusion, where receivers misinterpret ANC data or ignore unrecognized DID/SDID pairs. | Maintain up‑to‑date DID/SDID allocation tables from relevant standards; design receivers to ignore unknown DID/SDID gracefully. |
| Misinterpretation of SDI family bit rates | Bit rates for SD‑SDI, HD‑SDI, 3G‑SDI, and 12G‑SDI are known only via secondary sources.[2][6][9][13] | Unverified (normative values from SMPTE not seen) | Designs that assume incorrect nominal bit rates may fail to lock or may violate jitter/eye requirements. | Use bit rates cited here only as initial guidance; confirm against original SMPTE standards or authoritative manufacturer documentation before design. |
| ANC packet structure assumptions | ANC packet fields (ADF, DID, SDID, DC, UDW, CS) are normatively defined in SMPTE 291 and ITU‑R BT.1619, but some implementation details (parity bits, exact word values) are drawn from secondary tutorials.[1][5][14] | Core fields normative; some encoding details unverified | Incorrect checksum or parity handling may cause receivers to discard ANC packets. | Implement ANC packet logic according to SMPTE 291 clause‑level text; treat secondary descriptions as illustrative only and validate against reference waveforms. |
| Use of payload identifier | ITU‑R BT.1614 specifies payload ID usage, but some interfaces may not implement it or may implement proprietary identifiers.[8] | Normative when BT.1614 is applied | Auto‑format detection fails, requiring manual configuration; multi‑vendor interoperability issues. | Implement BT.1614 payload ID where possible; provide manual override and detection logic that falls back to picture analysis or operator configuration. |

---

## 7. Implementation Guidance

This section describes recommended practices derived from the normative sources and secondary engineering material. All guidance is explicitly marked as implementation advice and does not carry normative force unless tied to a standard.

### 7.1 Recommended Fields and Checks

1. For ANC packet processing, implement explicit parsing of ADF, DID, SDID, DC, UDW, and CS fields for each packet.[5][14]  
   - Validate that ADF patterns match SMPTE 291 definitions (exact bit patterns should be retrieved from the standard).[1]  
   - Verify DC matches the number of UDW words; reject packets with inconsistent counts.[5]  
   - Recompute CS and compare with received checksum; flag CRC/checksum errors for monitoring and optional discard.[5]  

2. Track ANC packets by DID/SDID pairs and maintain a registry of known payload types (e.g., timecode, captions, payload ID).[5][8][12]  
   - Unknown DID/SDID pairs should be logged but not cause stream failure.  

3. When using payload identifiers as per ITU‑R BT.1614, implement:  
   - Correct line and sample placement per interface type (SD‑SDI, HD‑SDI, 3G‑SDI).[8]  
   - Repetition rates consistent with BT.1614 to ensure timely detection.[8]  

4. For multi‑rate PHY implementations, use rate‑detection logic akin to that described in vendor IP cores (supporting SMPTE 125M, 259M, 260M, 267M, 274M, 292M, 295M, 296M).[10]  
   - Implement PLL and clock‑recovery logic capable of locking to all supported nominal bit rates and verifying lock status.  

### 7.2 Modeling Unverified or Externally Supplied Values

1. Treat nominal SDI bit rates from secondary sources as parameters in models, annotated with “Unverified” status until confirmed against SMPTE standards or trusted equipment.[2][6][9][13]  
2. When modeling ANC spaces, represent line ranges and sample boundaries symbolically (e.g., “VANC lines” and “HANC words”) rather than numeric values unless they are extracted from primary texts.[1][5]  
3. Jitter budgets expressed in UIpp (unit intervals peak‑to‑peak) should be considered design‑time assumptions based on generic serial communications practice; SDI‑specific jitter limits should be taken from licensed standards or manufacturer specifications.[15][11]  

### 7.3 Validation Rules Suitable for Implementations

1. **ANC packet sanity checks** (derived from SMPTE 291 and ITU BT.1619):  
   - Check that packets begin with valid ADF patterns and occur only within ANC spaces.[1][5]  
   - Validate DID/SDID combinations against configuration to ensure they correspond to expected services.[5][8][12]  

2. **Bit rate and framing checks**:  
   - Confirm that measured serial rate matches one of the known SDI family rates (e.g., 270 Mbit/s, 1.485 Gbit/s, 2.970 Gbit/s, 12 Gbit/s) within tolerance.[2][6][9][13]  
   - Verify that payload identifier (BT.1614) matches the detected video format (e.g., 1080i, 1080p, 2160p).[8]  

3. **Cross‑domain ANC consistency**:  
   - For systems bridging SDI and ST 2110‑40, ensure DID/SDID mapping is consistent and that ANC packets preserved over IP can be re‑embedded into SDI without loss.[1][12]  

---

## 8. Validation Checklist

This checklist summarizes key validation activities for SDI/ANC implementations. Each item is grounded in the sources cited elsewhere.

1. Confirm ANC words are treated as 10‑bit values end‑to‑end, including packing/unpacking in internal buses.[1]  
2. Verify that ANC packets follow the structure ADF, DID, SDID, DC, UDW, CS for all VANC services.[5][14]  
3. Ensure that ANC packets are inserted only in ANC spaces (horizontal/vertical blanking) and not in active video regions.[1][5]  
4. Confirm that in single‑stream SDI interfaces (SMPTE 259M/ITU‑R BT.125M), all data services share a single ANC data flag and CRC per stream.[5]  
5. Validate that payload identifier packets (BT.1614) are present, correctly formatted, and placed on the lines and samples specified for each interface type.[8]  
6. Measure the serial bit rate and confirm it matches known SDI family rates (e.g., 270 Mbit/s, 1.485 Gbit/s, 2.970 Gbit/s, 12 Gbit/s) within device tolerance.[2][6][9][13][11]  
7. Test interoperability by feeding SDI signals into multiple vendor receivers (deserializers, router inputs) and confirming lock and correct ANC decoding.[10][11][7]  
8. For KLV metadata over SDI, verify ANC packet structure and DID/SDID assignments conform to SMPTE RP 214.[3][7]  
9. For SDI–ST 2110 bridges, verify that ANC packet content and DID/SDID are preserved in ST 2110‑40 streams and re‑embedded correctly.[12]  

---

## 9. Open Questions / Unverified Items

These topics could not be verified from accessible primary SDI transport standards and should be treated as open questions for future research:

1. Exact scrambling polynomial(s) and scrambler/descrambler behavior used in SMPTE 259M, 292M, 424M, and 2082, including reset timing and interaction with timing reference words (TRS).  
2. Precise CRC polynomial(s), field locations, and computation algorithms for video data and ANC spaces in the core SDI transport standards.  
3. Detailed numerical definitions of HANC and VANC line ranges and sample counts for each supported format (e.g., 1080i, 1080p, 720p) as specified in SMPTE 259M/292M/424M/2082 and related mapping standards.  
4. Electrical specifications (amplitude, rise/fall times, impedance tolerances, jitter limits) for SDI physical layer in each standard, beyond illustrative examples of jitter units.[11][15]  
5. Complete DID/SDID allocation tables for all standardized ANC payloads (captions, timecode, audio metadata, payload identifiers) and their mandatory vs optional status in different applications.[5][8][12]  
6. Any profile‑specific restrictions or extensions (e.g., Level A/B distinctions in 3G‑SDI beyond secondary descriptions).[9]  

Each of these items should be resolved by consulting the full SMPTE standards, associated payload specifications, and official errata.

---

## 10. Sources

This section summarizes the main documents and materials used, with their role in the report. Citations in the body correspond to these sources.

- **SMPTE ST 291:2006 – Television — Ancillary Data Packet and Space Formatting**: Primary standard defining ANC packet structure (10‑bit words, ADF, DID, SDID, DC, UDW, CS) and ANC space formatting applicable to SDI interfaces.[1]  
- **SMPTE RP 214:2002 (stable 2007)**: Recommended practice for packing SMPTE KLV metadata into SMPTE 291 ANC packets for transport over SMPTE 259M and 292M video signals.[3]  
- **SMPTE ST 375:2003 (stable 2010)**: Standard specifying mapping of vertical ancillary data packets into DV payload areas, referencing SMPTE 291 ANC packet formats.[4]  
- **ITU‑R BT.1614 (2003)**: Recommendation defining a 4‑byte video payload identifier and its embedding in SMPTE 291 ANC packets for various digital interfaces, including SDI, with defined positions and repetition rates.[8]  
- **ITU‑R BT.1619 (2003)**: Recommendation describing vertical ancillary data mapping for serial digital interfaces aligned with SMPTE 259M/125M and SMPTE 292M, using SMPTE 291 type‑2 ANC packets.[5]  
- **Wikipedia “Serial digital interface” and “SMPTE 424M” (EN, DE)**: Secondary sources summarizing SDI family standards, bit rates, example formats, and naming conventions.[2][9][13]  
- **Samim Group “Understanding Serial Digital Interface (SDI) Video”**: Secondary engineering article outlining SDI generations (SD‑SDI, HD‑SDI, 3G‑SDI, 12G‑SDI) and associated bit rates and example formats.[6]  
- **Lattice Semiconductor “Multi‑Rate Serial Digital Interface PHY Layer”**: Vendor IP‑core documentation listing supported SMPTE standards and describing multi‑rate SDI PHY and rate detection.[10]  
- **National Semiconductor LMH0031 Datasheet**: Vendor datasheet noting support for SMPTE 259M/344M/292M data rates and typical receiver behavior.[11]  
- **Xilinx XAPP299 “SDI: Ancillary Data and EDH Processors”**: Vendor application note referencing SMPTE 291 ANC format and describing ANC/EDH processing in FPGA designs.[7]  
- **EBU “ancillary_data.md” for ST 2110‑40**: Secondary documentation explaining how ANC payloads per SMPTE 291 are carried in IP streams and identified by DID/SDID pairs.[12]  
- **CSDN blog “SMPTE 291M Packet”**: Secondary technical tutorial on SMPTE 291 ANC packet structure, reinforcing the 10‑bit word model and field order.[14]  
- **IEEE 802.1 contribution on uncomp video timing requirements**: Slide deck mentioning wide‑band jitter in unit intervals for serial video; used as context for jitter units rather than normative SDI limits.[15]  

All normative claims in this report are tied to SMPTE and ITU documents where their clauses were visible, or clearly marked as unverified where primary text was not accessed. Secondary materials are used strictly for implementation context and to inform best‑practice guidance.