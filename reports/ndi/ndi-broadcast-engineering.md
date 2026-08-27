```yaml
---
report_id: ndi-video-broadcast-engineering-reference
title: NDI Video Technical Reference for Broadcast Engineering
topic: NDI Video
report_version: 0.1.0
generated_date: 2026-08-27
source_access: mixed
source_cutoff: 2026-08-27
prompt_template: template-new-topic.md
prompt_template_version: 0.1.0
status: draft
---
```

NDI (Network Device Interface) is a royalty‑free IP media transport specification developed by NewTek/NDI for low‑latency video and audio over standard LAN infrastructure, documented primarily via vendor white papers, SDK documentation, certification criteria, and networking guidance rather than through a formal standards body publication.[2][3][6][9][14] Public documentation is sufficient for high‑level architectural and engineering planning, but detailed on‑the‑wire protocol behavior and many codec/transport parameters depend on non‑public SDK and Advanced SDK materials and must be treated as implementation‑dependent.[1][5][7]  

---

## 1. Executive Summary

NDI defines a family of IP media protocols and SDKs enabling interoperable, low‑latency video, audio, and ancillary data transport between compatible devices and applications over standard Ethernet networks, positioned for live production and broadcast workflows.[2][3][6][14] The ecosystem’s normative information is distributed across NDI white papers, protocol summaries, SDK documentation, NDI Certified technical requirements, and networking guidelines, with several critical details (e.g., full protocol and codec specifications) accessible only under commercial or SDK license terms, constraining clause‑level, third‑party conformance analysis.[1][2][5][7][9]

For engineering purposes, this reference treats the following as primary normative anchors:

- NDI Protocols and NDI White Paper documentation describing the protocol families and fundamental design.[1][3][6]  
- NDI Certified Technical Requirements, which define latency, codec, and feature requirements for products seeking certification.[9]  
- Official NDI Network Guidelines and vendor networking appnotes that, while not protocol‑normative, are widely used as de‑facto guidance for network design and configuration.[10][11]

Key high‑trust points:

- NDI is described as a royalty‑free IP transmission standard intended for live production over standard LAN networking.[2][6]  
- NDI Certified guidelines require meeting specific latency targets (e.g., less than 100 ms latency and 150 ms glass‑to‑glass latency for certain device classes), codec requirements (e.g., SpeedHQ and H.264/H.265 in particular roles), and SDK version constraints.[9]  
- Network design guidance strongly favors managed, non‑blocking switches, multicast support (IGMP), VLAN segregation, and QoS when scaling beyond roughly 20 devices.[10][11]  
- Version evolution introduces features such as reliable UDP (rUDP), NDI|HX3, Bridge, Router, HDR support, and 16‑bit color, with stated backward compatibility but version‑capability negotiation implications.[6][11]

Due to the lack of a public, independent normative standard, several aspects—on‑the‑wire framing, detailed transport behavior, exact port assignments, and formal timing models—must be treated as **Unverified** or **implementation‑dependent** unless tied directly to the accessible documents listed in this report.[1][5][7][13]

---

## 2. Scope and Boundaries

### 2.1 What NDI Standardizes (Within Accessible Sources)

1. **Media transport behavior at the logical level**  
   - NDI is described as a standard that enables video‑compatible products to share video across a local area network using IP transmission.[2]  
   - The NDI White Paper covers “fundamental principles, technological features, protocols, and settings,” indicating that the specification defines families of protocols (e.g., for discovery, control, and streaming) and associated configuration parameters.[3]  
   - The NDI Protocols documentation is positioned as a reference for the protocols themselves, implying canonical definitions of discovery, connection, and transport behavior, although detailed clauses are not visible in public excerpts.[1]

2. **Ecosystem codecs and capability profiles**  
   - NDI Certified Technical Requirements call out specific codecs (e.g., SpeedHQ, H.264, H.265) and latency constraints as part of required device behavior for certification, making these effectively normative for certified senders/receivers and gateways.[9]  
   - NDI 6 is documented as adding official HDR support for NDI HX (HX2, HX3) and NDI High Bandwidth, which defines new capability sets for senders and receivers.[9]

3. **High‑level version behavior**  
   - Vendor networking documentation summarizing NDI versions lists features such as rUDP, NDI‑HX3, NDI Bridge, and NDI Router for NDI 5, and HDR plus 16‑bit color formats for NDI 6, and states that NDI versions are backward compatible, with newer devices expected to interoperate with older ones based on the older devices’ capabilities.[11]

### 2.2 What NDI Does *Not* Explicitly Standardize (or Is Not Publicly Visible)

1. **Formal third‑party standardization artifacts**  
   - None of the accessible documents mention any SMPTE, IETF, or ISO/IEC standard being the published home of NDI; instead, NDI is described as a NewTek/NDI technology and specification.[2][3][14]  
   - The official documentation site indexes NDI materials as “Docs and Guides” and white papers, not as a ratified external standard with numbered clauses.[3][7]  
   - Consequently, there is no public evidence in these sources of a formal, external standard (e.g., an RFC or SMPTE standard) for NDI; this remains **Unverified** and must not be assumed.[1][3][7][14]

2. **Complete, public, on‑the‑wire protocol definition**  
   - The NDI Protocols page and related documentation are high‑level entry points, and the Advanced SDK documentation is explicitly associated with a commercial license, indicating that not all protocol details are freely accessible.[1][5]  
   - There is no clause‑level, public specification in the snippets for packet formats, precise transport layering, error correction mechanisms, or timing model; these must be treated as **implementation‑dependent** and likely embedded in SDK headers and internal documentation.[1][5][15]

3. **Comprehensive network port table**  
   - A document titled “NDI Related Network Ports” exists and appears to be the authoritative reference for port usage, but the accessible excerpt does not list actual ports.[13]  
   - Exact port numbers, transport types per port, and any optional/alternative port usages remain **Unverified** in this report and should be obtained directly from the port reference documentation.[13]

4. **Formal mapping to other broadcast standards**  
   - Sienna’s Inferred Routing white paper describes mechanisms to integrate NDI routing with MPEG‑TS workflows, reflecting practical mapping to TS‑based broadcast systems rather than a standardized inter‑standard mapping defined by NDI itself.[4]  
   - NDI documentation snippets do not define normative mappings to SDI, SMPTE 2110, or MPEG‑TS; such interworking is thus **implementation‑dependent**.[3][4][6]

### 2.3 Adjacent Technologies and Common Misconceptions

- NDI is described as a software specification and IP media transport standard, often used alongside existing broadcast transport mechanisms such as MPEG‑TS, as evidenced by technical material on NDI‑based routing over TS workflows.[4][14]  
- Network documentation for NDI emphasizes standard LAN infrastructure and managed switches, reinforcing that NDI is designed for conventional Ethernet/IP environments rather than requiring specialized deterministic fabric.[2][10][11]  
- Because technical briefs describe NDI as “royalty free” and focused on LAN IP transmission, it can be mistaken for an open, fully documented standard; however, the presence of commercial SDK and Advanced SDK licensing and absence of a published external standard indicate that much of the specification remains proprietary.[2][5][7][14]

### 2.4 Source Access Limitations

- The **NDI Advanced SDK** documentation is associated with a commercial license, so key protocol and encoding details are not publicly visible and cannot be independently quoted or normatively analyzed here.[5]  
- The **NDI SDK Documentation** referenced in an external repository appears to be a copy of SDK documentation, but its licensing and currency are unclear, and it should not be treated as an authoritative public standard without confirmation from NDI.[15]  
- Some white papers and network guidelines are freely downloadable PDFs, but may not include clause‑level normative language, making it difficult to extract exact “shall” requirements beyond high‑level statements and tables.[2][6][10][11]  
- The NDI Docs & Guides index hints at a dynamic documentation system, but the degree of clause‑level access and whether all internal protocol details are exposed via this mechanism remain **Unverified**.[1][7]

---

## 3. Standards and Source Map

### 3.1 Document Overview Table

| Document                                                | Version/date (as observed)         | Role                                                   | Source status                  | Clause‑level visibility |
|---------------------------------------------------------|------------------------------------|--------------------------------------------------------|--------------------------------|-------------------------|
| NDI Protocols (Docs & Guides)                           | Last updated 2026‑06‑26[1]         | High‑level protocol reference for NDI families         | Public web documentation[1]    | Partial; detailed clauses not visible in excerpt (**Unverified**) |
| NDI White Paper (Docs & Guides)                         | Dated 2026‑01‑23[3]                | Compressed overview of principles, features, protocols | Public web documentation[3]    | High‑level narrative; minimal explicit “shall” language (**Assumed**) |
| NDI 5.6 White Paper                                     | Published 2023‑09 (PDF)[6]         | Detailed description of NDI 5.6 feature set and behavior | Public PDF from NDI[6]      | Moderate; feature descriptions but not wire‑level detail (**Assumed**) |
| NewTek NDI Technical Brief                              | 2016 era; undated in snippet[2]    | Introductory technical brief on NDI as IP standard     | Public PDF from NewTek[2]      | High‑level, mostly descriptive |
| NDI Docs & Guides index                                 | Last updated 2026‑08‑22[7]         | Documentation index and LLM‑oriented access            | Public web documentation[7]    | Meta‑level only        |
| NDI Advanced SDK documentation                          | Last updated 2026‑07‑03[5]         | Commercial SDK docs; detailed protocol/API behavior    | Licensed; restricted access[5] | Full for licensees; not public (**Unverified** here) |
| NDI Certified Technical Requirements                    | Last updated 2026‑06‑28[9]         | Certification criteria (latency, codecs, SDK versions) | Public web documentation[9]    | Good clause‑level items, mostly checklist style |
| NDI Related Network Ports                               | Dated 2025‑03‑03[13]               | Authoritative NDI port and transport assignments       | Public web documentation[13]   | Detailed but not visible in excerpt (**Unverified**) |
| NDI Network Guidelines (2018)                           | Dated 21 May 2018[10]              | Network design and bandwidth planning guidance         | Public PDF (secondary)[10]     | Tables and recommendations; best practice, not normative |
| Ross Ultrix NDI Networking Application Note             | Version date not explicit; features table includes 2021–24[11] | Implementation‑oriented NDI networking and version guide | Vendor appnote (secondary)[11] | Good best‑practice text; not protocol‑normative |
| Sienna NDI Inferred Routing White Paper v1.10           | 2 Dec 2023[4]                      | Description of NDI routing over MPEG‑TS                | Vendor white paper (secondary)[4] | Implementation approach, not NDI‑normative |
| NDI.Cloud White Paper                                   | Date not explicit in snippet[8]    | Cloud transport and routing concepts for NDI           | Vendor white paper (secondary)[8] | High‑level conceptual |
| Wikipedia: Network Device Interface (NDI)               | Last updated 2026‑05‑13[14]        | General overview and historical context                | Tertiary reference[14]         | Descriptive; not normative |
| External NDI SDK Documentation PDF (keylase repository) | Dated 2018‑01‑08[15]               | Copy of NDI SDK docs (version unspecified)            | Unofficial mirror (secondary)[15] | Potentially detailed but licensing and freshness unclear (**Unverified**) |

---

## 4. Normative Requirements Catalog

This catalog distinguishes between:

- **Normative (Certification/Official)**: Explicit requirements from NDI Certified Technical Requirements or NDI‑authored documentation.  
- **Best practice**: Strong guidance from NDI network guidelines or widely adopted vendor appnotes.  
- **Assumed**: Reasonable interpretation of high‑level statements, called out as assumptions.  
- **Unverified**: Items suspected but not verifiable from visible text.

### 4.1 Requirements Table

| ID      | Requirement or rule                                                                                                  | Applies to                             | Normative citation   | Status (normative / best practice / assumed / unverified) | Implementation implication                                                                                           | Confidence |
|---------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------|----------------------|------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|-----------|
| NDI‑REQ‑001 | Devices seeking NDI Certification shall use I‑frame‑only encoding for keyframes where specified by the program. | NDI Certified encoders                 | NDI Certified Tech Requirements[9] | Normative (certification)                                    | Encoder must be configured for I‑frame‑only keyframes in relevant modes; non‑compliance may prevent certification.[9] | High      |
| NDI‑REQ‑002 | NDI Certified devices shall achieve less than 100 ms latency (class‑specific context).                           | NDI Certified senders/receivers        | NDI Certified Tech Requirements[9] | Normative (certification)                                    | System design must ensure end‑to‑end latency under 100 ms as measured per certification methodology.[9]              | High      |
| NDI‑REQ‑003 | NDI Certified devices shall achieve glass‑to‑glass latency of 150 ms or less for applicable classes.            | NDI Certified end‑to‑end solutions     | NDI Certified Tech Requirements[9] | Normative (certification)                                    | Combined camera‑to‑display path must not exceed 150 ms; exceeding this fails certification tests.[9]                  | High      |
| NDI‑REQ‑004 | Keyframe response time for NDI Certified devices shall be under 80 ms.                                          | NDI Certified encoders/decoders       | NDI Certified Tech Requirements[9] | Normative (certification)                                    | Encoder/decoder must support rapid keyframe generation/lock; slow response may break trick‑mode/fast recovery.[9]    | High      |
| NDI‑REQ‑005 | NDI Certified products shall use the current SDK or immediately previous SDK (e.g., 6.0 current, 5.6 previous). | NDI Certified software/hardware        | NDI Certified Tech Requirements[9] | Normative (certification)                                    | Products must track SDK releases; certification may be tied to SDK major/minor and require re‑testing on updates.[9] | High      |
| NDI‑REQ‑006 | NDI Certified devices shall support specified codecs (including SpeedHQ and, where required, H.264/H.265).      | NDI Certified encoders/decoders       | NDI Certified Tech Requirements[9] | Normative (certification)                                    | Implementers must integrate these codecs with NDI SDK; missing codecs may limit device class or block certification.[9] | High   |
| NDI‑REQ‑007 | With NDI 6, HDR shall be supported on send and receive for NDI HX (HX2, HX3) and NDI High Bandwidth where advertised. | NDI 6 senders/receivers supporting HDR | NDI Certified Tech Requirements[9] | Normative (feature behavior within NDI 6 ecosystem)          | Devices advertising HDR capability must correctly implement HDR exchange over NDI HX and High Bandwidth.[9]          | Medium    |
| NDI‑REQ‑008 | NDI implementations shall comply with the protocols as documented in the NDI Protocols documentation.           | All NDI senders/receivers              | NDI Protocols documentation[1]     | Assumed normative                                           | Implementers should treat NDI Protocols as canonical; deviations may break interoperability.[1]                       | Medium    |
| NDI‑REQ‑009 | NDI implementations shall use network ports and behaviors as defined in the NDI Related Network Ports document. | All NDI networked components           | NDI Related Network Ports[13]      | Assumed normative                                           | Port assignments must follow the official mapping; arbitrary reassignment risks connection failure or conflicts.[13]  | Medium    |
| NDI‑REQ‑010 | NDI devices are expected to interoperate across versions, with newer devices interacting based on older device capabilities. | Multi‑version NDI devices          | Ross Ultrix NDI Networking Appnote[11] | Assumed normative; stated behavior but vendor‑authored      | Feature negotiation should be conservative; rely on lowest common denominator when connecting differing NDI versions.[11] | Medium |
| NDI‑REQ‑011 | For networks with more than about 20 NDI devices, a managed, non‑blocking network with appropriate features should be used. | NDI network designers               | NDI Network Guidelines[10]; Ross Ultrix Appnote[11] | Best practice                                                | Use managed switches, adequate uplinks, and traffic engineering when scaling device counts beyond this threshold.[10][11] | High |
| NDI‑REQ‑012 | Large NDI deployments should support multicast (including IGMP querier and snooping) for efficient routing.     | NDI network designers                  | Ross Ultrix NDI Networking Appnote[11] | Best practice                                                | Enable multicast features to control group traffic and avoid flooding; lack of IGMP may cause congestion.[11]        | High      |
| NDI‑REQ‑013 | NDI traffic should be logically separated (e.g., via VLANs) from non‑NDI traffic.                               | NDI network designers                  | Ross Ultrix NDI Networking Appnote[11] | Best practice                                                | Configure VLANs to segregate media and control; reduces contention and improves QoS.[11]                               | High      |
| NDI‑REQ‑014 | QoS should be configured to prioritize NDI media traffic on converged networks.                                | NDI network designers                  | Ross Ultrix NDI Networking Appnote[11] | Best practice                                                | Use DSCP or similar for NDI flows; otherwise media may suffer under load compared to best‑effort traffic.[11]         | High      |
| NDI‑REQ‑015 | Network design must account for per‑format bandwidth approximations as documented in NDI Network Guidelines tables. | NDI network designers              | NDI Network Guidelines[10]         | Best practice                                                | Capacity planning should use documented per‑format Mbps/MB/s figures; under‑provisioning leads to drops/latency.[10] | Medium    |

Notes:

- IDs NDI‑REQ‑008 and NDI‑REQ‑009 refer to entire documents as normative anchors because detailed clause‑level requirements are not visible; they are therefore marked as **Assumed** rather than directly observed.  
- IDs NDI‑REQ‑011 through NDI‑REQ‑015 are not protocol‑normative but are operationally critical and widely adopted.

---

## 5. Engineering Model

### 5.1 Core Objects and Roles

Based on accessible documentation, the following conceptual entities are central to NDI engineering:

1. **NDI Sender (Source)**  
   - A device or application that produces one or more NDI streams (video, audio, and ancillary data) for consumption by receivers over IP.[2][3][6]  
   - Examples include cameras, software vision mixers, and playout systems that integrate the NDI SDK or Advanced SDK.[2][5][6]

2. **NDI Receiver (Sink)**  
   - A device or application that subscribes to and decodes NDI streams, rendering them or converting them to other formats such as SDI or MPEG‑TS.[2][4][6]  
   - Receivers rely on discovery mechanisms and NDI protocols to select and connect to senders.[1][3]

3. **Discovery Mechanism**  
   - NDI deployments can use automatic discovery mechanisms such as mDNS or centralized discovery servers, as referenced in network application guidance.[11]  
   - The selection of discovery mechanism impacts scalability, multi‑subnet operation, and network security posture.[11]

4. **NDI Streams and Flows**  
   - NDI Network Guidelines describe format, frame rate (FPS), and bandwidth (Mbps, MB/s), implying each stream is characterized by resolution, frame rate, and bitrate properties used for capacity planning.[10]  
   - Certified requirements further bind certain streams to particular codec families (e.g., SpeedHQ, H.264/H.265, HX profiles).[9]

5. **NDI Version Capabilities**  
   - Versioned feature sets (e.g., rUDP, NDI‑HX3, Bridge, Router in NDI 5; HDR and 16‑bit color in NDI 6) define what a given node can send or receive.[6][11]  
   - Cross‑version interoperability is documented as being based on the older device’s capabilities.[11]

6. **Gateways/Routers/Bridges**  
   - Sienna’s NDI Inferred Routing and NDI.Cloud white papers describe devices and systems that perform routing, switching, and conversion between NDI and other transport families such as MPEG‑TS or cloud paths.[4][8]  
   - These systems typically maintain NDI semantics at the edges while encapsulating streams in other transports internally.[4][8]

### 5.2 Data‑Flow Model (Conceptual)

A simplified NDI media path relevant to broadcast engineering:

```mermaid
flowchart LR
    Camera[NDI Sender / Encoder] --> Net[IP Network (LAN/WAN)]
    Net --> Recv1[NDI Receiver / Switcher]
    Net --> Recv2[NDI Receiver / Monitor]
    Net --> Gateway[NDI Router / Bridge / TS Gateway]
    Gateway --> TS[MPEG-TS / Other Transport]<br/>(implementation dependent)
```

- Senders publish NDI streams which are discovered by receivers via mDNS or a configured discovery server.[1][3][11]  
- Receivers subscribe to selected streams and decode video/audio according to the NDI protocol and codec configuration.[1][3][9]  
- Routers/bridges may terminate NDI connections and re‑emit them over alternate transports (e.g., MPEG‑TS), as documented in Inferred Routing and cloud white papers.[4][8]

### 5.3 Timing and Latency Model (Certification‑Oriented View)

From NDI Certified Technical Requirements:

- Devices must meet **less than 100 ms latency** (in specified certification contexts).[9]  
- End‑to‑end **glass‑to‑glass latency must not exceed 150 ms** for certain classes.[9]  
- **Keyframe response time must be under 80 ms**.[9]

For engineering modeling:

- Treat these values as maximum allowable latencies measured under defined test conditions (e.g., specific format, frame rate, and network conditions) used by the certification program.[9]  
- The documents do not expose how this budget is distributed among encoder, network, decoder, and display; any such budgeting is **Assumed** and not specified here.[9]

### 5.4 Discovery and Control Flow

Network application guidance provides the following insights:

- Design considerations include discovery mechanism choice (mDNS versus discovery server) and transport type (rUDP, TCP, multicast, etc.), indicating that NDI ecosystems can utilize multiple underlying transport and discovery modes.[11]  
- These choices affect not only connectivity but also firewalling, routing between subnets, and monitoring strategies.[11]  
- As the underlying protocol details are not public in full, the exact discovery packet formats, message timing, and fallback behavior remain **Unverified** and must be understood via SDK documentation.[1][5][11]

### 5.5 Boundary Between Specification and Implementation Policy

- **Specification‑driven aspects:**  
  - Codec requirements and latency targets for certification.[9]  
  - Protocol families and message types, as described in NDI Protocols and the white papers.[1][3][6]  
  - Port assignments and network behavior as defined in the ports documentation.[13]

- **Implementation policy (operator‑specific):**  
  - Exact latency budget allocation among components (camera, encoder, network, decoder, display).[9]  
  - Choice of network topology, redundancy, VLAN layout, and QoS strategy based on NDI Network Guidelines and organizational standards.[10][11]  
  - Decision to use mDNS versus a discovery server and design of any discovery‑domain boundaries.[11]  
  - Approach to integrating NDI with TS, SDI, or cloud transports (e.g., choice of NDI router or gateway product).[4][8][11]

---

## 6. Formulas, Calculations, and Worked Examples

The public NDI documents surveyed include quantitative thresholds and bandwidth tables but do not expose explicit mathematical formulas for core protocol behavior; the formulas here are therefore limited to compliance checks directly tied to published numeric thresholds.

### 6.1 Formula and Assumption Register

| Calculation name                         | Formula or method                                                                                       | Inputs and units                                | Source citation | Normative or assumed | Worked example available | Confidence |
|------------------------------------------|----------------------------------------------------------------------------------------------------------|-------------------------------------------------|-----------------|----------------------|---------------------------|-----------|
| NDI Certified latency compliance         | Pass if measured_latency ≤ 100 ms; Fail otherwise                                                       | measured_latency (ms) from test instrumentation | NDI Certified Tech Requirements[9] | Normative (certification check) | Yes (see 6.2.1)          | High      |
| NDI Certified glass‑to‑glass compliance  | Pass if measured_glass_to_glass_latency ≤ 150 ms; Fail otherwise                                        | measured_glass_to_glass_latency (ms)            | NDI Certified Tech Requirements[9] | Normative (certification check) | Yes (see 6.2.2)          | High      |
| NDI keyframe response compliance         | Pass if measured_keyframe_response_time < 80 ms; Fail otherwise                                         | measured_keyframe_response_time (ms)            | NDI Certified Tech Requirements[9] | Normative (certification check) | Yes (see 6.2.3)          | High      |
| NDI per‑format bandwidth planning        | Look up documented Mbps/MB/s for given format and frame rate from NDI Network Guidelines table          | Format (e.g., 1920×1080), FPS                   | NDI Network Guidelines[10] | Best practice           | No (values not visible in excerpt; **Unverified**) | Medium    |

No bitrate or packetization formulas are provided because the visible sources do not publish them; any such formulas would be speculative and are therefore omitted.

### 6.2 Worked Examples

#### 6.2.1 Latency Compliance Check (NDI‑REQ‑002)

Objective: verify that a certified device meets the “less than 100 ms latency” requirement.[9]

- Measured end‑to‑end latency during certification test: 72 ms (example measurement).  
- Compliance rule: pass if measured_latency ≤ 100 ms.[9]

Result: 72 ms ≤ 100 ms → **Pass** according to NDI Certified criteria.[9]

(Measurement methodology—test pattern, measurement device, and sampling conditions—is defined by the certification program and not visible in the public excerpt; it must be treated as implementation‑dependent.[9])

#### 6.2.2 Glass‑to‑Glass Latency Compliance (NDI‑REQ‑003)

Objective: verify that an end‑to‑end system meets the 150 ms glass‑to‑glass latency requirement.[9]

- Measured camera‑lens to monitor‑glass latency: 138 ms (example).  
- Compliance rule: pass if measured_glass_to_glass_latency ≤ 150 ms.[9]

Result: 138 ms ≤ 150 ms → **Pass** per NDI Certified glass‑to‑glass provision.[9]

#### 6.2.3 Keyframe Response Compliance (NDI‑REQ‑004)

Objective: verify keyframe response time is under 80 ms.[9]

- Measured keyframe response time after a trigger (e.g., scene change or forced keyframe): 95 ms (example).  
- Compliance rule: pass if measured_keyframe_response_time < 80 ms.[9]

Result: 95 ms ≥ 80 ms → **Fail**; encoder/decoder or configuration must be adjusted to meet NDI Certified criteria.[9]

---

## 7. Interoperability Risks and Ambiguity Register

### 7.1 Risk and Ambiguity Table

| Risk or ambiguity                                                                                  | Evidence                                                                                                   | Normative status                                  | Failure symptom                                                                                   | Mitigation or modeling rule                                                                                     |
|----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|---------------------------------------------------|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| Incomplete public protocol specification                                                           | Protocols documented via NDI Protocols and white papers, but full details appear tied to SDK/Advanced SDK.[1][3][5][7][15] | Specification gap; on‑wire details proprietary    | Third‑party implementations may misinterpret packet formats, timing, or error handling.           | Treat SDK and Advanced SDK as canonical behavior; avoid independent re‑implementations without official support.[1][5] |
| Unspecified port numbers in accessible excerpt                                                     | Dedicated NDI Related Network Ports doc exists, but ports are not visible in the snippet.[13]             | Ports are normative but not visible here          | Misconfigured firewalls; streams fail to connect; intermittent discovery.                         | Obtain and track port assignments from NDI ports document; parameterize them in config models rather than hard‑coding.[13] |
| Multi‑version feature mismatch                                                                     | NDI versions are backward compatible, but capabilities differ (rUDP, HX3, HDR, 16‑bit color, etc.).[6][11] | Feature negotiation behavior described at high level | Newer features silently disabled when connecting to older devices; confusion over HDR/HX behavior. | Model connections in terms of lowest common capability set; expose per‑endpoint version/capability metadata.[6][11] |
| Discovery mechanism divergence (mDNS vs discovery server)                                          | Network appnote lists transport and discovery as design considerations, including mDNS and discovery servers.[11] | Implementation choice; not protocol‑normative     | Devices fail to discover each other across subnets; excessive discovery traffic in large networks. | Establish a discovery policy (mDNS vs server) per deployment; ensure all NDI nodes conform; document discovery domains.[11] |
| Network scaling issues on unmanaged or lightly managed switches                                   | Network guidelines and appnotes recommend managed, non‑blocking switches, especially beyond ~20 devices.[10][11] | Best practice; not protocol requirement           | Packet loss, increased latency, and jitter; intermittent stream failure under load.               | Use managed switches, ensure adequate uplink capacity, and follow bandwidth planning tables.[10][11]                         |
| Lack of formal mapping to TS/SDI/other broadcast transports                                       | Sienna’s Inferred Routing outlines an implementation, not an NDI‑normative mapping to MPEG‑TS.[4]         | Implementation‑defined                             | Different vendors’ gateways may behave inconsistently; unexpected timing or metadata handling.    | Normalize on a limited set of gateway types and document their behavior; avoid assuming standardized mapping.[4]           |
| Ambiguous latency measurement methodology for certification thresholds                             | Certification doc states numeric thresholds but not measurement details in visible excerpt.[9]             | Normative thresholds; methodology **Unverified**   | Disagreement about whether a device meets “<100 ms” or “150 ms” requirements.                     | Align with NDI certification test procedures; record test conditions with each measurement; do not mix methods.[9]         |
| Reliance on tertiary or mirrored documentation (e.g., external SDK PDF)                           | External repository hosts NDI SDK Documentation PDF of unknown license/date.[15]                          | Non‑authoritative                                  | Relying on outdated or modified docs; protocol drift between versions.                           | Use only official NDI documentation and SDKs for normative behavior; treat external mirrors as reference only.[5][7][15]    |

---

## 8. Implementation Guidance

The following guidance is based on the normative and best‑practice material above and is intended for engineering and AI‑assisted tooling. Items are explicitly labeled as **Best practice** or **Assumed** where not strictly normative.

### 8.1 Network Design and Capacity Planning

1. **Switching and topology (Best practice)**  
   - For deployments with more than roughly 20 NDI devices, plan on using managed, non‑blocking Ethernet switches.[10][11]  
   - Ensure sufficient uplink bandwidth between switches to accommodate aggregated NDI traffic, referencing per‑format Mbps/MB/s tables in the NDI Network Guidelines for conservative planning.[10]

2. **Multicast and IGMP (Best practice)**  
   - Enable multicast with IGMP querier and snooping where NDI streams are to be replicated efficiently to many receivers.[11]  
   - Verify that IGMP is correctly configured across all relevant VLANs to avoid flooding or black‑holing multicast traffic.[11]

3. **Segregation and QoS (Best practice)**  
   - Place NDI media traffic on dedicated VLANs or logically separated segments from general data traffic.[11]  
   - Implement QoS policies that prioritize NDI packets over best‑effort traffic, using DSCP or comparable mechanisms supported by network equipment.[11]

4. **Bandwidth headroom (Best practice)**  
   - Use per‑format bandwidth values from the NDI Network Guidelines as a baseline and add additional headroom (e.g., 20–30%) to accommodate bursts and network variability; the exact margin is an **Assumption**, not specified in the documents.[10]

### 8.2 Discovery and Addressing

1. **Discovery mechanism choice (Best practice)**  
   - In single‑subnet, small deployments, mDNS may be sufficient for automatic discovery.[11]  
   - In multi‑subnet or large deployments, prefer a discovery server architecture to reduce broadcast/multicast discovery traffic and provide deterministic endpoint registration.[11]

2. **Configuration modeling (Implementation guidance)**  
   - Model each NDI endpoint with attributes: NDI version, discovery mechanism, supported codecs and features (e.g., HX/HDR), and network location (VLAN, IP range).[6][9][11]  
   - Represent discovery domains as explicit objects (e.g., “Site A NDI discovery domain”) to clarify which devices can discover each other without gateway translation.[11]

### 8.3 Latency and Performance Engineering

1. **Latency targets (Normative and best practice)**  
   - Treat <100 ms latency and ≤150 ms glass‑to‑glass latency as hard upper bounds for certified devices.[9]  
   - For non‑certified deployments that require tight interactivity (e.g., vision mixing), adopting these thresholds as internal design targets is a strong **Best practice**.[9]

2. **Monitoring and measurement (Implementation guidance)**  
   - Use standardized latency measurement setups consistent with the certification program where possible; if not available, clearly document methods and conditions.[9]  
   - Track keyframe response performance, as >80 ms measured response violates certification criteria and may indicate encoder configuration problems.[9]

### 8.4 Codec and Feature Configuration

1. **Codec support (Normative for certification)**  
   - Ensure that SpeedHQ encoding/decoding is implemented where required by NDI Certified guidelines.[9]  
   - Support H.264 or H.265 where the device class mandates these codecs (e.g., certain NDI|HX modes), and verify the integration is compatible with the NDI SDK.[9]

2. **HDR and high‑bit‑depth operation (Normative within NDI 6 ecosystem)**  
   - For NDI 6 devices that advertise HDR, implement HDR support for NDI HX (HX2, HX3) and NDI High Bandwidth in both send and receive paths.[9]  
   - Validate HDR operation in workflows where the entire chain (camera, encoder, network, decoder, display) supports HDR; otherwise HDR capability may be unused or require conversion.[9]

### 8.5 Version Management

1. **SDK version constraints (Normative for certification)**  
   - Track the current and previous SDK versions (e.g., 6.0 current, 5.6 previous at time of observation), and ensure that certified devices use one of these in compliance with NDI Certified requirements.[9]  
   - Establish processes to update devices when newer SDKs are released and re‑validate interoperability with existing equipment.[9]

2. **Capability negotiation (Assumed)**  
   - When connecting devices of different NDI versions (e.g., NDI 5 to NDI 3), assume operation will fall back to the lower version’s capabilities, per vendor guidance on backward compatibility.[11]  
   - Do not assume newer features such as HDR, HX3, or Bridge will be available when older endpoints participate in the signal path.[6][11]

---

## 9. Validation Checklist

This checklist can be converted into automated or semi‑automated tests for NDI‑related deployments.

1. **Documentation and Versioning**  
   - [ ] Device documentation identifies NDI version (e.g., 5, 6) and supported feature sets (rUDP, HX3, HDR, etc.).[6][11]  
   - [ ] Device uses current or immediately previous NDI SDK version, as required for NDI Certified products.[9]

2. **Network Infrastructure**  
   - [ ] Managed, non‑blocking switches used where NDI device count exceeds ~20.[10][11]  
   - [ ] Uplink bandwidth between switches is provisioned according to per‑format NDI Network Guidelines plus safety margin.[10]  
   - [ ] Multicast, IGMP querier, and IGMP snooping are enabled if multicast operation is required.[11]  
   - [ ] VLANs or equivalent logical separation are configured to segregate NDI media from general data traffic.[11]  
   - [ ] QoS policies prioritize NDI traffic on converged networks.[11]

3. **Discovery and Ports**  
   - [ ] A consistent discovery strategy (mDNS or discovery server) is chosen and applied across the deployment.[11]  
   - [ ] Firewalls and ACLs permit required NDI ports as defined in the NDI Related Network Ports documentation.[13]  
   - [ ] Cross‑subnet routing and discovery are verified for all required NDI flows.[11][13]

4. **Latency and Performance**  
   - [ ] Measured latency complies with <100 ms target for certified devices.[9]  
   - [ ] Glass‑to‑glass latency does not exceed 150 ms for certified end‑to‑end systems.[9]  
   - [ ] Keyframe response time is <80 ms.[9]  
   - [ ] Monitoring is in place to detect packet loss, jitter, and congestion on NDI flows.[10][11]

5. **Codec and HDR**  
   - [ ] Required codecs (SpeedHQ, H.264/H.265 where applicable) are implemented and validated with the NDI SDK.[9]  
   - [ ] HDR support is implemented and verified for NDI 6 devices that advertise HDR capability for HX and High Bandwidth streams.[9]

6. **Interoperability and Routing**  
   - [ ] Multi‑version NDI endpoints interoperate with behavior consistent with lowest common capability, especially in mixed NDI 3/5/6 environments.[11]  
   - [ ] If NDI is routed via TS or cloud, gateways conform to documented NDI behavior at their NDI interfaces.[4][8]  
   - [ ] Any reliance on external SDK documentation mirrors is explicitly flagged and cross‑checked against official documentation.[5][7][15]

---

## 10. Open Questions / Unverified Items

The following items are explicitly **Unverified** given the limitations of the visible sources and should be treated with caution in any AI‑assisted engineering workflow.

1. **Exact on‑the‑wire protocol details**  
   - Packet formats, transport headers, timing models, and error recovery mechanisms for each NDI mode are not visible in accessible documentation and are presumed to exist only in SDK/Advanced SDK materials.[1][5][15]  
   - Any attempt to implement NDI without official SDKs, or to derive packet behavior purely from public docs, risks divergence.[1][5][15]

2. **Complete port and transport matrix**  
   - The NDI Related Network Ports document is identified as the authoritative reference, but the port table is not visible; exact TCP/UDP ports, multicast groups, and their mapping to discovery, control, and media flows remain unknown here.[13]  
   - Network designs relying on guessed ports or outdated information should be considered high risk.[13]

3. **Formal external standardization status**  
   - There is no reference in the visible documents to SMPTE, IETF, or other formal standards hosting NDI; it appears to remain a vendor‑controlled specification.[2][3][14]  
   - Whether any internal conformance test suites or formal profiles exist beyond NDI Certified guidelines is not documented in the excerpts.[9]

4. **Detailed certification test methodology**  
   - Latency and keyframe thresholds are given numerically, but test conditions, measurement points, and acceptance criteria beyond simple thresholds are not published in the visible text.[9]  
   - Engineering teams should treat NDI’s official certification program as the source of truth for such methodology and avoid assuming equivalence with internal tests.[9]

5. **Precise bandwidth values and compression behavior per format**  
   - NDI Network Guidelines include tables of Mbps and MB/s for various formats and frame rates, but concrete values are not visible in the excerpt, and any compression behavior (e.g., variable bitrate characteristics) is not described.[10]  
   - AI tools should avoid deriving precise bandwidth predictions without direct access to the full guidelines.[10]

6. **End‑to‑end HDR behavior and metadata handling**  
   - While HDR support for NDI 6 HX and High Bandwidth is stated, the exact handling of HDR metadata, transfer characteristics, and color space signaling is not visible.[9]  
   - Interoperability with other HDR ecosystems (HDR10, HLG, etc.) and conversion rules remain unverified from these sources.[9]

7. **Security model and encryption**  
   - The visible documents do not describe authenticated connections, encryption, or key management explicitly.[1][3][6][9][11]  
   - Any claims regarding NDI’s security mechanisms, or lack thereof, would be speculative and are therefore omitted.

---

## 11. Sources

(Ordered by priority; all numbering corresponds to inline citations.)

1. **NDI Protocols | Docs and Guides** – NDI documentation page describing protocol families and providing an API for dynamic documentation queries; last updated 2026‑06‑26.[1]  
2. **NewTek NDI Technical Brief** – PDF describing NDI as a royalty‑free standard for IP transmission and live production over LAN; provides foundational description of NDI’s role in broadcast workflows.[2]  
3. **NDI White Paper | Docs and Guides** – Overview white paper summarizing fundamental principles, technological features, protocols, and settings of NDI; dated 2026‑01‑23.[3]  
4. **Sienna NDI Inferred Routing White Paper v1.10** – Vendor white paper (2 Dec 2023) explaining NDI‑based routing and monitoring integrated with MPEG‑TS workflows; implementation‑focused.[4]  
5. **NDI Advanced SDK | Docs and Guides** – Documentation describing the Advanced SDK as part of a commercial license for integrators, indicating the presence of additional protocol and integration detail not publicly accessible.[5]  
6. **NDI 5.6 White Paper (2023)** – NDI‑authored PDF describing features, workflows, and evolution of NDI 5.6; includes technology overview and upgrade guidance.[6]  
7. **Docs & Guides Index | NDI** – Central index for NDI documentation, including an llms.txt index and Markdown variants of documentation pages; meta‑documentation resource.[7]  
8. **NDI.Cloud White Paper** – Vendor white paper describing cloud‑based transport and routing concepts for NDI, including references to the NDI Protocol.[8]  
9. **Technical Requirements | NDI Certified** – Official NDI Certified program guidelines listing technical requirements such as I‑frame‑only keyframes, latency thresholds, codec requirements, SDK version constraints, and HDR support for NDI 6.[9]  
10. **NDI Network Guidelines (21 May 2018)** – PDF outlining network considerations for NDI, including format/FPS/Mbps/MB/s tables and general network best practices.[10]  
11. **Ross Ultrix NDI Networking Application Note (NDI NETWORKING)** – Vendor application note describing NDI as an IP media transport standard, summarizing NDI versions and features (including rUDP, HX3, Bridge, Router, HDR, 16‑bit color) and recommending managed switches, multicast, VLANs, and QoS.[11]  
12. **Ross Ultrix NDI Networking Application Note (archived copy)** – Archived version of the same application note; content appears equivalent; used only as corroborating context.[12]  
13. **NDI Related Network Ports | Docs and Guides** – Official NDI document listing network ports associated with NDI protocols; referenced as normative for port assignments but not visible in detail in this report.[13]  
14. **Network Device Interface (NDI) – Wikipedia** – Tertiary reference providing general description of NDI as a software specification developed by NewTek and its role in IP video.[14]  
15. **NDI SDK Documentation (external repository PDF)** – External copy of NDI SDK documentation dated 2018‑01‑08; used cautiously as a secondary implementation context due to unclear licensing and potential obsolescence.[15]