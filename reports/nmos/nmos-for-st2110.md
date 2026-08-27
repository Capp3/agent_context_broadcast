---
report_id: nmos-for-2110-reference
title: NMOS for SMPTE ST 2110 – Control-Plane Technical Reference
topic: NMOS for 2110
report_version: 0.1.0
generated_date: 2026-08-27
source_access: public
source_cutoff: 2026-08-27
prompt_template: template-new-topic.md
prompt_template_version: 0.1.0
status: draft
---

## 1. Executive Summary

Networked Media Open Specifications (NMOS) are a family of AMWA interface specifications that provide discovery, registration, connection management, security, and related control-plane functions for professional IP media networks.[1][3][11] For SMPTE ST 2110 production systems, IS‑04 (Discovery & Registration) and IS‑05 (Device Connection Management) are identified as the critical NMOS components enabling interoperable, automated routing of essence flows between senders and receivers.[3][9][11][14]

AMWA IS‑04 defines APIs and mechanisms that allow media nodes to locate an NMOS registry using DNS‑SD, register their resources via HTTP and JSON, and expose those resources to control and monitoring applications via query and WebSocket subscription interfaces.[7][9][10][14] AMWA IS‑05 defines a transport‑independent connection management API that uses a shared data model with IS‑04 and is designed to manage logical connections between senders and receivers for protocols including RTP as used in SMPTE ST 2110.[2][4][9][14]

The AIMS/AMWA TR‑1001‑1 guidance (referenced in the NMOS FAQ) specifies that compliant systems shall use IS‑04 version 1.2 or higher and IS‑05 version 1.0 or higher, effectively defining a minimum NMOS profile for ST 2110 deployments.[9] Security, capability advertisement, and status monitoring for NMOS systems are covered by Best Current Practice documents BCP‑003, BCP‑004, BCP‑005, and BCP‑008, which AMWA labels as best practice rather than core normative requirements.[3][6]

This report catalogs the normative requirements from these specifications where accessible, distinguishes them from best‑practice and secondary guidance, maps an engineering model for NMOS‑based control of ST 2110 networks, and identifies interoperability risks and unverified areas that require further primary‑source review.

---

## 2. Scope and Boundaries

### 2.1 What “NMOS for 2110” Covers

1. **Discovery and Registration (IS‑04)**  
   IS‑04 defines the mechanisms for discovery and registration of NMOS resources within a media network, including the Registration API, Query API, and Node API.[7][10]  
   Media Nodes locate an IS‑04 registry using DNS‑SD (with a preference for unicast DNS), register their resource information using HTTP and JSON, and expose it to applications that query via HTTP or subscribe via WebSocket.[7][9][10][14]  
   These functions apply to nodes, devices, sources, flows, senders, and receivers in a networked media system.[14]

2. **Connection Management (IS‑05)**  
   IS‑05 defines a device connection management API that is transport‑independent and uses a shared data model with IS‑04.[2][4]  
   IS‑05 is designed to be used alongside IS‑04 and uses a “Transports register” to describe support for transports such as RTP, WebSocket, MQTT and others.[2][4]  
   IS‑05 focuses on creation of higher‑level logical connections between senders and receivers rather than low‑level transport details.[2][4][9]

3. **ST 2110 Control‑Plane Role**  
   NMOS is widely described as forming the control plane for SMPTE ST 2110 systems, where IS‑04 and IS‑05 provide discovery and routing of uncompressed IP media streams.[8][11][14]  
   The NMOS FAQ explicitly states that TR‑1001‑1 (a deployment guideline for SMPTE ST 2110 systems) mandates IS‑04 v1.2 or higher and IS‑05 v1.0 or higher, indicating their central role in 2110 production environments.[9]

4. **Security and Operational Best Practice (BCPs)**  
   AMWA Best Current Practice (BCP) specifications provide best practice for NMOS security, capabilities, and status reporting:  
   – BCP‑003‑01: Secure communications in NMOS systems.[3][6]  
   – BCP‑003‑02: Authorization in NMOS systems.[3][6]  
   – BCP‑003‑03: Certificate provisioning.[3][6]  
   – BCP‑004‑01 and BCP‑004‑02: Receiver and Sender capabilities.[6]  
   – BCP‑005‑01: EDID to receiver capabilities mapping.[3][6]  
   – BCP‑008‑01 and BCP‑008‑02: NMOS receiver and sender status monitoring (some work‑in‑progress).[6]

5. **Control Architecture and ID/Timing Model (MS Series)**  
   Media Spec documents MS‑04 and MS‑05‑01/‑02 define an ID & timing model and a control architecture/framework for NMOS, serving as architectural references rather than per‑API interface definitions.[3]

### 2.2 What is Explicitly Not Standardized

1. **Media Transport and Payload Format**  
   NMOS specifications do not define the media transport payload formats, timing requirements, or clocking models for SMPTE ST 2110 streams; these remain within SMPTE ST 2110 and related standards.[2][9][11]  
   IS‑05 is transport‑independent by design and relies on a transports register for protocol‑specific details rather than embedding ST 2110‑specific rules directly in the core specification.[2]

2. **Registry Backend Storage and Implementation**  
   The NMOS FAQ states that IS‑04 defines the Registration and Query APIs and how to find their endpoints, but does not define how the API backend should be implemented or how registry data should be stored.[9]  
   Database schema, clustering, and persistence strategies for the registry are therefore implementation‑dependent.

3. **User Interface and Workflow Semantics**  
   NMOS specifications do not prescribe user interface design or specific operational workflows, such as how routers present sources/destinations to operators; these aspects are left to implementations and facility policies.[9][10]

4. **End‑to‑End ST 2110 System Design**  
   TR‑1001‑1 offers a deployment profile and requirements for end‑to‑end ST 2110 systems including NMOS, but this guidance is not fully reproduced in the NMOS specifications themselves and is only partially visible via references.[9]  
   Detailed ST 2110 system architecture, redundancy schemes, and PTP design remain outside the NMOS documents and are governed by SMPTE and other guidance, which are not fully visible in the sources cited here.

### 2.3 Adjacent Standards and Common Misconceptions

1. **Adjacency: SMPTE ST 2110 and TR‑1001‑1**  
   NMOS provides control‑plane APIs used by SMPTE ST 2110 devices but is not itself a SMPTE standard.[11][14]  
   TR‑1001‑1 ties NMOS into ST 2110 deployments by specifying minimum versions of IS‑04 and IS‑05 for compliant systems.[9]

2. **Adjacency: NMOS Extensions (IS‑07, IS‑08, etc.)**  
   Additional NMOS interface specifications such as IS‑07 (Events & Tally) and IS‑08 (Audio Channel Mapping) add higher‑level control capabilities that can be used with ST 2110 systems but are not strictly required by TR‑1001‑1.[3][6]

3. **Common Misconception: NMOS as a Transport Standard**  
   Some secondary materials emphasize NMOS as part of “ST 2110” systems, which can be misinterpreted to mean NMOS defines transport behavior; the NMOS FAQ and IS‑05 description clarify that NMOS is transport‑independent and focused on control/connection management.[2][9][11][14]

### 2.4 Source Access and Limitations

All NMOS specifications referenced here are publicly accessible AMWA documents, including IS‑04, IS‑05, NMOS FAQ, controller guides, and BCP/MS documents listed in the NMOS specification index.[1][3][7][9][10]  
SMPTE ST 2110 primary standards and the full text of TR‑1001‑1 are not present in the retrieved sources and may be subject to licensing or paywalls, limiting clause‑level citation from those documents in this report.[9][11]  

Clause‑level references within IS‑04, IS‑05, and related documents are not visible in the snippets available here; therefore, this report references whole documents rather than specific clause numbers and marks such limitations explicitly as Unverified at clause level where appropriate.[1][2][7][9]

---

## 3. Standards and Source Map

### 3.1 Primary NMOS Specifications and Related Documents

| Document                          | Version/date (as seen)                                           | Role                                                  | Source status                          | Clause-level visibility |
|-----------------------------------|-------------------------------------------------------------------|-------------------------------------------------------|----------------------------------------|-------------------------|
| AMWA IS‑04 NMOS Discovery & Registration Specification | Stable; latest listed release v1.3.3 (Dec 2024)[1][7][11]       | Primary spec for NMOS resource discovery and registration | Public; AMWA specification (Stable)[1][7] | Clauses not visible in snippets (Unverified at clause level) |
| AMWA IS‑05 NMOS Device Connection Management Specification | Stable; releases listed as v1.2.0, v1.1.2, v1.0.2[1][2]; Wikipedia indicates latest v1.2.2 (Oct 2022)[11] | Primary spec for NMOS device connection management    | Public; AMWA specification (Stable)[1][2] | Clauses not visible in snippets (Unverified at clause level) |
| NMOS FAQ                           | Undated FAQ, last updated 2026‑03‑10[9]                           | Clarifies NMOS scope, key requirements, TR‑1001 references | Public; informational[9]               | No formal clauses; narrative sections only |
| AMWA “NMOS Specifications by Type” index | Dated 2025‑08‑14; updated 2025‑08‑25[3]                          | Catalog of IS, MS, BCP documents and status           | Public; informational index[3]          | N/A (index)             |
| AMWA IS‑04 main specification page | Updated 2026‑08‑25[7]                                            | High‑level overview and entry point for IS‑04         | Public[7]                               | Only bullet summaries visible |
| AMWA IS‑05 main specification page | Updated 2026‑08‑21[2]                                            | High‑level overview and entry point for IS‑05         | Public[2]                               | Only bullet summaries visible |
| IS‑05 Interoperability: NMOS IS‑04 | Branch v1.0.x interoperability note dated 2025‑04‑02[4]          | Explains relationship and shared data model with IS‑04 | Public[4]                               | Only brief statement visible |
| AMWA Info‑005 NMOS Controller Implementation Guide | Dated 2024‑08‑29; updated 2025‑09‑25[10]                         | Implementation guidance for controllers using IS‑04 and IS‑05 | Public; informational[10]              | Clauses not visible; narrative |
| NMOS Testing Tool (nmos‑testing)   | Master branch snapshot updated 2026‑08‑14[6]                      | Non‑normative test suite for NMOS specs and BCPs      | Public; tooling[6]                      | N/A (tooling)           |
| AMWA NMOS specification roadmap    | Roadmap PDF dated 2023‑09‑19[15]                                 | Overview of NMOS functional areas and priority        | Public; informational[15]              | N/A (roadmap)           |

### 3.2 Best Current Practice (BCP) and Media Spec (MS) Documents

| Document            | Version/date (as seen)           | Role                                                   | Source status                 | Clause-level visibility |
|---------------------|----------------------------------|--------------------------------------------------------|-------------------------------|-------------------------|
| BCP‑003‑01          | Not dated in snippet; listed as AMWA Specification[3][6] | Secure communications in NMOS systems (best practice)  | Public; BCP (Stable)[3][6]    | Not visible             |
| BCP‑003‑02          | Listed as AMWA Specification[3][6] | Authorization in NMOS systems (best practice)          | Public; BCP (Stable)[3][6]    | Not visible             |
| BCP‑003‑03          | Listed as AMWA Specification[3][6] | Certificate provisioning in NMOS systems (best practice) | Public; BCP (Stable)[3][6] | Not visible             |
| BCP‑004‑01          | Listed as AMWA Specification[6]  | Receiver capabilities                                  | Public; BCP (Stable)[6]       | Not visible             |
| BCP‑004‑02          | Listed as AMWA Specification[6]  | Sender capabilities                                    | Public; BCP (Stable)[6]       | Not visible             |
| BCP‑005‑01          | Listed as AMWA Specification[3][6] | EDID to receiver capabilities mapping                  | Public; BCP (Stable)[3][6]    | Not visible             |
| BCP‑008‑01          | Listed as work in progress[3][6] | NMOS receiver status monitoring                        | Public; WIP[3][6]             | Not visible             |
| BCP‑008‑02          | Listed in nmos‑testing[6]        | NMOS sender status monitoring                          | Public; WIP[6]                | Not visible             |
| MS‑04               | Listed as AMWA Specification[3]  | ID & timing model for NMOS                             | Public; MS[3]                 | Not visible             |
| MS‑05‑01            | Listed as AMWA Specification[3]  | NMOS control architecture                              | Public; MS[3]                 | Not visible             |
| MS‑05‑02            | Listed as AMWA Specification[3]  | NMOS control framework                                 | Public; MS[3]                 | Not visible             |
| MS‑05‑03            | Work in progress[3]              | NMOS control block specs                               | Public; WIP[3]                | Not visible             |

### 3.3 Secondary Sources (Context Only)

| Document/Source                   | Role                                                | Notes |
|-----------------------------------|-----------------------------------------------------|-------|
| “Introduction to NMOS” (AIMS PDF) | Introductory overview; highlights IS‑04/05 as core | Secondary; used for context, not normative[8] |
| NMOS blog on SMPTE ST 2110 control plane | Describes NMOS role as control plane for ST 2110 | Secondary; Go implementation example[14] |
| Wikipedia: Networked Media Open Specifications | Summarizes NMOS history and notes IS‑04/05 as critical for ST 2110 | Secondary; cross‑checked with AMWA[11] |
| Vendor datasheets referencing NMOS (e.g., Tieline, QxP) | Evidence of real‑world use of NMOS in ST 2110 devices | Secondary evidence only[12][13] |

Conflicts between primary and secondary sources (notably IS‑05 latest version numbering) are documented in Section 7.[1][2][11]

---

## 4. Normative Requirements Catalog

**Note:** Because clause‑level text is not visible in all primary sources, requirements are derived from visible statements and labeled with conservative confidence. Where wording such as “provides” or “designed to” appears rather than explicit “shall,” requirements are downgraded to Best Practice or Assumed as appropriate.[2][4][7][9]

### 4.1 Requirements Table

| ID | Requirement or rule | Applies to | Normative citation | Normative / best practice / assumed / unverified | Implementation implication | Confidence |
|----|---------------------|-----------|--------------------|--------------------------------------------------|----------------------------|-----------|
| NMOS‑IS04‑DISC‑001 | Media Nodes shall use DNS‑SD to locate the IS‑04 registry; unicast DNS is preferred. | Media Nodes | IS‑04 overview; “Media Nodes locate IS‑04 registry using DNS‑SD (unicast preferred)”[7][14] | Normative (DNS‑SD use); preference for unicast is best practice (wording “preferred”) | Nodes must implement DNS‑SD client capable of discovering registry endpoints via DNS. | Medium (clause text not visible) |
| NMOS‑IS04‑REG‑002 | Media Nodes shall register their resource information using HTTP and JSON with the IS‑04 Registration API. | Media Nodes | IS‑04 overview; “Media Nodes register their resource information with HTTP + JSON”[7][9] | Normative | Nodes must implement HTTP client to POST/PUT JSON resource documents (nodes, devices, sources, flows, senders, receivers) to the registry. | Medium |
| NMOS‑IS04‑QUERY‑003 | Control and monitoring applications shall obtain resource information through the IS‑04 Query API via HTTP and/or WebSocket subscription. | Controllers, monitoring apps | IS‑04 overview and Controller Guide; “Applications query with HTTP and/or subscribe with WebSocket”[7][9][10] | Normative for IS‑04‑compliant discovery; use of WebSocket optional where “and/or” appears | Controllers must implement HTTP client for Query API and may implement WebSocket client for update subscriptions. | Medium |
| NMOS‑IS04‑NODE‑004 | Media Nodes shall expose a Node API that the IS‑04 registry and controllers can use to obtain node information. | Media Nodes | Controller Guide; “It comprises three APIs: the Registration API, the Query API and the Node API.”[10] | Normative (composition of IS‑04) | Nodes must expose an HTTP Node API endpoint serving node and resource metadata. | Medium |
| NMOS‑IS05‑CORE‑005 | IS‑05 shall provide a transport‑independent way of connecting media nodes, using a shared data model with IS‑04. | Devices implementing IS‑05 | IS‑05 overview; “Provides a transport‑independent way of connecting Media Nodes”[2]; Interoperability note; “shares a data model with IS‑04 and is designed to be used alongside it”[4] | Normative (transport independence); Best practice (co‑deployment with IS‑04) | IS‑05 implementations must not hard‑code only one transport; they must model connections abstractly and reuse IS‑04 resource identifiers. | Medium |
| NMOS‑IS05‑TRANS‑006 | IS‑05 shall support multiple transports via a Transports register, including RTP, WebSocket, MQTT and others. | IS‑05 Controllers, Devices | IS‑05 overview; “Supports RTP, WebSocket, MQTT and other transports via the Transports register”[2] | Normative (support via register) but specific transports may be optional per profile (Unverified) | Controllers and devices must consult an external transports registry to understand connection parameters for specific protocols such as RTP. | Medium |
| NMOS‑IS05‑CONN‑007 | IS‑05 shall support single and bulk connections and immediate and delayed connections. | IS‑05 Controllers, Devices | IS‑05 overview; “Supports single + bulk connections, immediate + delayed connections”[2] | Normative (features listed) | Implementations must support applying connection changes both per‑receiver and in bulk, with scheduling for delayed activation. | Low–Medium (details not visible) |
| TR1001‑NMOS‑008 | Systems conforming to TR‑1001‑1 shall implement IS‑04 version 1.2 or higher and IS‑05 version 1.0 or higher. | ST 2110 Systems claiming TR‑1001‑1 compliance | NMOS FAQ; “TR‑1001‑1 specifies use of IS‑04 version 1.2 or higher and IS‑05 version 1.0 or higher.”[9] | Normative for TR‑1001‑1 | ST 2110 deployments following TR‑1001‑1 must select NMOS stacks with at least these versions; lower versions violate TR‑1001‑1. | High (text explicit) |
| NMOS‑SEC‑BCP3‑009 | NMOS systems should apply secure communications, authorization, and certificate provisioning as defined in BCP‑003‑01/‑02/‑03. | All NMOS components | NMOS specs‑by‑type; “These specify best practice for use of NMOS APIs: … BCP‑003‑01 … BCP‑003‑02 … BCP‑003‑03.”[3][6] | Best practice (BCP) | Implementations should deploy TLS, authentication, and certificate workflows per BCP‑003 rather than bespoke mechanisms. | High |
| NMOS‑CAP‑BCP4‑010 | NMOS senders and receivers should expose their capabilities using BCP‑004‑01 and BCP‑004‑02. | Senders, Receivers | NMOS testing tool listing of BCP‑004‑01/‑02.[6] | Best practice | Capability models should follow BCP‑004 to improve interoperability and allow controllers to apply consistent capability negotiation. | Medium |
| NMOS‑EDID‑BCP5‑011 | Where HDMI/EDID is in use, EDID to NMOS receiver capabilities mapping should follow BCP‑005‑01. | Receivers, Gateways | NMOS specs‑by‑type; BCP‑005‑01.[3][6] | Best practice | Device vendors should map EDID information into standardized NMOS receiver capability models to simplify configuration. | Medium |
| NMOS‑STAT‑BCP8‑012 | Implementations may use BCP‑008‑01/‑02 for receiver and sender status monitoring; these documents are work in progress. | Monitoring Systems | NMOS testing tool listing and specs‑by‑type.[3][6] | Unverified/Optional (WIP) | Adoption is optional; where used, behavior may change as specs evolve. | Medium |
| NMOS‑ARCH‑MS‑013 | Implementations should align IDs and timing with MS‑04 and follow control architecture and framework guidance in MS‑05‑01/‑02. | System architects | NMOS specs‑by‑type listing MS‑04, MS‑05‑01/‑02.[3] | Best practice (architectural) | ID and timing behavior, and control frameworks, should follow these media specs, particularly in larger multi‑controller ST 2110 deployments. | Low (details not visible) |

---

## 5. Engineering Model

### 5.1 Core Objects and Relationships

Secondary and primary sources describe the IS‑04 data model in terms of the following resource types: nodes, devices, sources, flows, senders, and receivers.[9][10][14]  
IS‑04 defines how these resources are registered, discovered, and updated via its Node, Registration, and Query APIs.[7][10]

At a high level:

- **Node**: Represents a logical or physical entity on the network that hosts one or more devices and exposes a Node API.[10][14]  
- **Device**: A unit within a node that groups senders and receivers and represents a controllable device.[14]  
- **Source**: An origin of media content (e.g., video or audio source) that can feed one or more flows.[14]  
- **Flow**: A specific media stream (e.g., elementary audio or video) associated with a source; flows are associated with senders and receivers but are independent of physical transport endpoints.[14]  
- **Sender**: Represents an output of a device that can transmit one flow via a specific transport (e.g., RTP) to one or more receivers.[14]  
- **Receiver**: Represents an input of a device that can receive flows from senders via a specific transport.[14]

The NMOS FAQ and Controller Guide describe that controllers use the IS‑04 Query API to discover these resources and obtain updates about them.[9][10] The IS‑04 specification comprises three APIs:

- **Registration API** – where nodes register resource documents.[7][10]  
- **Query API** – where controllers discover resources via HTTP queries and WebSocket subscriptions.[7][9][10]  
- **Node API** – where the node exposes its own resources and state.[10]

IS‑05 shares this data model, using the same senders and receivers defined by IS‑04 to create logical connections between them.[4][9] IS‑05 extends the model with connection resources (not fully visible here) that link a given sender and receiver via a specific transport described in the transports register.[2][4]

### 5.2 Control and Data Flow Semantics

The interaction of IS‑04 and IS‑05 in a typical ST 2110 deployment can be modeled as follows:

```mermaid
flowchart LR
    node[Media Node] -->|Register resources (HTTP+JSON)| reg[IS-04 Registration API]
    reg -->|Index resources| query[IS-04 Query API]
    controller[Controller] -->|Discover resources (HTTP)| query
    query -->|Updates (WebSocket)| controller
    controller -->|Connection requests| is05[IS-05 Connection API]
    is05 -->|Configure| sender[Sender]
    is05 -->|Configure| receiver[Receiver]
    sender -->|ST 2110 RTP stream| receiver
```

– Media Nodes register their resources with the IS‑04 Registration API using HTTP and JSON, after discovering the registry via DNS‑SD.[7][9]  
– The registry indexes these resources and exposes them via the IS‑04 Query API for controllers.[7][9][10]  
– Controllers issue HTTP queries and may subscribe via WebSocket to receive updates about resource changes.[7][9][10]  
– Controllers then use IS‑05 to request logical connections between senders and receivers, selecting transports (particularly RTP for ST 2110) via the transports register.[2][4][9][14]  
– Senders and receivers configure their transport endpoints accordingly, establishing the actual ST 2110 RTP streams.[2][11][14]

IS‑04 does not define how the registry stores data or how applications react to updates; these behaviors are explicitly left to implementations.[9] IS‑05 does not define the detailed ST 2110 packet or SDP structure, relying on a transports register instead, which again shifts implementation specifics to the transport definitions.[2]

### 5.3 Boundaries Between Standardized Behavior and Policy

**Standardized behavior (per NMOS):**

- Use of DNS‑SD to locate IS‑04 registry endpoints.[7][14]  
- Use of HTTP and JSON for registration and query operations.[7][9]  
- Availability of Node, Registration, and Query APIs in IS‑04.[7][10]  
- Use of a shared data model between IS‑04 and IS‑05, including resource types such as senders and receivers.[4][9][14]  
- Use of IS‑05 Connection API to define logical connections between senders and receivers and support for multiple transports via a transports register.[2][4]

**Implementation or policy‑dependent behavior:**

- How registry backends are implemented and scaled.[9]  
- How controllers prioritize or select among multiple possible senders/receivers or transports (e.g., which ST 2110 flow to use).[9][10]  
- Security policy choices (e.g., whether TLS and authorization from BCP‑003 are mandatory within a particular facility).[3][6]  
- Detailed ST 2110 transport configurations such as SDP, multicast addressing, and redundancy, which are defined or influenced by non‑NMOS standards and facility policies.[2][11]

---

## 6. Formulas, Calculations, and Worked Examples

The visible NMOS specifications and summaries do not define explicit numeric formulas (e.g., bandwidth calculations, packet rate formulas) within IS‑04 or IS‑05; instead, they focus on structural models and API interactions.[2][7][9] Numerical aspects relevant to ST 2110 (such as bitrates, packetization intervals, and PTP timing) are governed by SMPTE standards which are not fully accessible in the cited material.[11]

Given the non‑negotiable requirement not to invent formulas, this section limits itself to a formula/assumption register indicating the absence of explicit NMOS formulas and marking several conceptual mappings as Unverified.

### 6.1 Formula and Assumption Register

| Calculation name | Formula or method | Inputs and units | Source citation | Normative or assumed | Worked example available | Confidence |
|------------------|-------------------|------------------|-----------------|----------------------|--------------------------|-----------|
| NMOS resource registration frequency | Unverified – IS‑04 does not expose explicit formula for registration intervals in visible text. | N/A | IS‑04 overview and FAQ indicate registration via HTTP + JSON but do not state timing rules.[7][9] | Unverified | No | Low |
| Connection activation timing (immediate vs delayed) | IS‑05 states support for immediate and delayed connections but does not expose the timing model or formula in visible text. | N/A | IS‑05 overview: “Supports single + bulk connections, immediate + delayed connections.”[2] | Unverified (likely normative details in full spec) | No | Low |
| Mapping ST 2110 flows to NMOS Flow resources | Conceptual mapping only; a single ST 2110 RTP stream is generally represented as an NMOS Flow, but no explicit formula or rule is visible. | N/A | Secondary blog describes resources (sources, flows, senders, receivers) but not explicit ST 2110 mapping rules.[14] | Assumed (secondary) | No | Low |
| ID and timing mapping (MS‑04) | MS‑04 is labeled “ID & Timing Model” but no formulas are visible; details are Unverified. | N/A | NMOS specs‑by‑type listing MS‑04.[3] | Unverified | No | Low |

**Implementation guidance:** any numeric calculations (e.g., bandwidth estimates, latency budgets) used in ST 2110 systems must be derived from SMPTE documents or internal engineering rules and not assumed to be standardized by NMOS.[2][11]  

---

## 7. Interoperability Risks and Ambiguity Register

### 7.1 Risk and Ambiguity Table

| Risk or ambiguity | Evidence | Normative status | Failure symptom | Mitigation or modeling rule |
|-------------------|----------|------------------|-----------------|-----------------------------|
| IS‑05 latest version mismatch (1.2.0 vs 1.2.2) | NMOS index lists IS‑05 releases as v1.2.0, v1.1.2, v1.0.2,[1][2] while Wikipedia states latest IS‑05 is v1.2.2 (Oct 2022).[11] | Unverified; conflict between primary (index) and secondary (Wikipedia) sources | Confusion about required bugfixes/features; inconsistent profile statements | Use the AMWA spec index as the primary authority; confirm actual version from the IS‑05 document itself; document the specific minor version used in deployments. |
| Clause‑level unknowns in IS‑04/IS‑05 | Snippets show overview bullets but not full clause text.[2][7][9] | Not a spec issue but a documentation limitation | Misinterpretation of bullets as full normative text; missing edge‑case rules | Treat overview text as indicative only; consult full IS‑04/IS‑05 texts for clause‑level requirements before writing conformance tests or profiles. |
| Optional vs mandatory transports in IS‑05 | IS‑05 “supports RTP, WebSocket, MQTT and other transports via the Transports register” but does not state which transports are mandatory in visible text.[2] | Unverified | Device claims IS‑05 compliance but supports only non‑RTP transports, causing mismatch in ST 2110 environments | For ST 2110 deployments, profile IS‑05 to require at least one RTP transport entry; treat other transports as optional. Mark this as deployment policy, not NMOS core. |
| Security as “best practice” rather than mandatory | BCP‑003 documents are labeled as specifying best practice.[3][6] | Best practice (not core normative) | Mixed deployments where some devices run unsecured HTTP while others enforce TLS; failed connections or downgraded security | Define a facility‑level policy that mandates BCP‑003 adoption; treat TLS and authorization as required in the local profile even if NMOS core does not mandate them. |
| Capability modeling divergence | BCP‑004 and BCP‑005 define best practice for capabilities and EDID mapping; adoption is optional.[3][6] | Best practice | Controllers cannot reliably interpret capabilities; manual configuration required; misrouted flows | Require BCP‑004/‑005 compliance in ST 2110 profiles; use NMOS testing tools to verify capability representation.[6] |
| Status monitoring spec instability | BCP‑008‑01/‑02 are marked work in progress.[3][6] | Unverified/optional | Changes in status schemas or semantics across versions; incompatible monitoring tools | Treat BCP‑008 as experimental; isolate status monitoring logic and plan for change; version‑gate any reliance on BCP‑008 features. |
| Over‑reliance on NMOS for ST 2110 behavior | Secondary sources emphasize NMOS as “control plane for ST 2110,” which may imply NMOS dictates transport behavior.[8][11][14] | Clarified by NMOS FAQ and IS‑05 as incorrect; NMOS is transport‑independent.[2][9] | Attempt to derive ST 2110 timing or bandwidth rules from NMOS; incomplete system design | Clearly separate NMOS (control APIs) from SMPTE ST 2110 (transport behavior) in design documents; reference appropriate SMPTE specs for transport details. |
| TR‑1001‑1 version dependencies under‑enforced | NMOS FAQ states TR‑1001‑1 requires IS‑04 ≥1.2 and IS‑05 ≥1.0.[9] | Normative for TR‑1001‑1 | Deployments claim TR‑1001‑1 compliance but use outdated NMOS versions; controllers or devices misbehave due to incompatible schemas | Validate NMOS versions as part of acceptance testing; encode minimum version checks in conformance tools and procurement requirements. |

---

## 8. Implementation Guidance

This section provides implementation guidance derived from primary and secondary sources and explicitly labels it as Best Practice. None of the rules in this section are new normative requirements unless they directly restate a primary source requirement.

### 8.1 Minimal NMOS Profile for ST 2110 Facilities (Best Practice)

Based on the NMOS FAQ and TR‑1001‑1 reference, a practical minimal NMOS profile for ST 2110 deployments is:[9][11][14]

1. **Core NMOS**  
   – IS‑04: at least version 1.2, preferably latest stable (e.g., v1.3.x).[1][9][11]  
   – IS‑05: at least version 1.0, preferably latest stable.[1][2][9][11]  

2. **Security and Capabilities**  
   – BCP‑003‑01/‑02/‑03 for secure communications, authorization, and certificate provisioning.[3][6]  
   – BCP‑004‑01/‑02 for receiver and sender capabilities.[6]  
   – Optional: BCP‑005‑01 for EDID mapping; BCP‑008‑01/‑02 for status monitoring (with WIP caveats).[3][6]

3. **Architecture**  
   – Use MS‑04 and MS‑05‑01/‑02 as architectural references for ID and timing models and control architecture in multi‑controller environments.[3]

**Implementation‑dependent policy:** which of these BCP/MS documents are mandatory is a facility decision; it is recommended that security (BCP‑003) be treated as mandatory in production ST 2110 installations.[3][6]

### 8.2 Node Implementation Guidance

1. **Discovery and Registration**  
   – Implement DNS‑SD client logic to locate the IS‑04 registry, preferring unicast DNS while falling back to mDNS where appropriate.[7][14]  
   – Register node, device, source, flow, sender, and receiver resources with the Registration API using HTTP and JSON.[7][9][14]  
   – Provide a Node API endpoint that reflects the node’s resource graph consistently with what is registered.[10][14]

2. **Transport Configuration (IS‑05 Integration)**  
   – Expose senders and receivers with sufficient metadata (e.g., flow association, capabilities) for IS‑05 controllers to configure ST 2110 transports.[2][4][14]  
   – Implement IS‑05 connection handlers capable of applying immediate and delayed connection requests and of handling bulk connection changes.[2]

3. **Security (Best Practice)**  
   – Implement TLS and mutual authentication per BCP‑003 (insofar as facilities require it) instead of ad‑hoc mechanisms.[3][6]

### 8.3 Controller Implementation Guidance

The Info‑005 Controller Guide describes controllers as consumers of IS‑04 and IS‑05 APIs.[10] Best‑practice guidance includes:

1. **Resource Discovery**  
   – Use the IS‑04 Query API for resource discovery, periodically querying and/or subscribing via WebSocket for updates.[7][9][10]  
   – Treat the Node API as a reference source, not a replacement for the registry, especially in larger systems.[10]

2. **Connection Management**  
   – Use IS‑05 Connection API to manage sender‑receiver connections instead of proprietary routing protocols.[2][9][10]  
   – Consult the transports register to determine how to configure RTP and other transports, particularly for ST 2110 flows.[2][4]  

3. **Version and Capability Awareness**  
   – Inspect IS‑04 and IS‑05 versions exposed by nodes where possible and enforce TR‑1001‑1 minimums.[9]  
   – Use BCP‑004 capability models to match senders and receivers in a vendor‑neutral manner.[6]

4. **Security and Authorization**  
   – Align with BCP‑003‑02 authorization models, including integration with identity providers if necessary; treat them as facility policy rather than optional convenience.[3][6]

### 8.4 Testing and Validation Practice

AMWA provides a testing tool for the NMOS specifications and BCPs.[6] While non‑normative, use of this tool is a strong best practice:

- It includes test suites for IS‑04, IS‑05, IS‑07, IS‑08, and BCP‑003/‑004/‑005/‑008.[6]  
- Test results can be used to verify claimed NMOS compliance and ensure consistent behavior across vendors, especially in ST 2110 deployments.[6]

---

## 9. Validation Checklist

This checklist provides a practical framework for validating “NMOS for 2110” implementations. Each item references the requirement IDs in Section 4.

### 9.1 Node and Registry Validation

1. **DNS‑SD Discovery**  
   – Verify nodes use DNS‑SD to locate the IS‑04 registry (NMOS‑IS04‑DISC‑001).[7]  
   – Confirm unicast DNS is used where facility design requires it.[7][14]

2. **Registration Behavior**  
   – Confirm that nodes register resources (nodes, devices, sources, flows, senders, receivers) using HTTP and JSON to the Registration API (NMOS‑IS04‑REG‑002).[7][9][14]  
   – Confirm resource representations are consistent between Node API and registry.[10][14]

3. **Query API Functionality**  
   – Validate that controllers can query resources via the Query API and receive correct responses (NMOS‑IS04‑QUERY‑003).[7][9][10]  
   – Validate WebSocket subscriptions deliver resource updates when supported.[7][10]

4. **Node API Exposure**  
   – Confirm that each node exposes a Node API as part of its IS‑04 implementation (NMOS‑IS04‑NODE‑004).[10]

### 9.2 Connection Management Validation

1. **IS‑05 Availability and Data Model**  
   – Confirm devices expose IS‑05 Connection API endpoints.[2]  
   – Verify that IS‑05 resources reference senders and receivers defined in IS‑04 (NMOS‑IS05‑CORE‑005).[4][9]

2. **Transport Handling**  
   – Check that IS‑05 can configure connections using RTP via entries in the transports register (NMOS‑IS05‑TRANS‑006).[2]  
   – Verify behavior for multiple transports (e.g., WebSocket, MQTT) if applicable.[2]

3. **Connection Operations**  
   – Validate single and bulk connection operations (NMOS‑IS05‑CONN‑007).[2]  
   – Validate immediate activation and delayed activation of connections, including correct timing behavior (NMOS‑IS05‑CONN‑007).[2]

### 9.3 Profile and Security Validation

1. **TR‑1001‑1 Compliance Checks**  
   – Confirm IS‑04 version ≥1.2 and IS‑05 version ≥1.0 (TR1001‑NMOS‑008).[9]  
   – Document versions in facility profiles and procurement specifications.[9][11]

2. **Security Best Practice**  
   – Verify that BCP‑003‑01/‑02/‑03 have been implemented for secure communications and authorization (NMOS‑SEC‑BCP3‑009).[3][6]  
   – Confirm that TLS certificates and authorization policies are consistent across all NMOS components.[3][6]

3. **Capabilities and Status**  
   – Check that senders/receivers expose capabilities in line with BCP‑004‑01/‑02 (NMOS‑CAP‑BCP4‑010).[6]  
   – Where used, validate EDID mapping (BCP‑005‑01) and status monitoring (BCP‑008‑01/‑02) for consistency (NMOS‑EDID‑BCP5‑011, NMOS‑STAT‑BCP8‑012).[3][6]

4. **Testing Tool Use**  
   – Run AMWA NMOS testing tool suites against devices and controllers to corroborate compliance with IS‑04, IS‑05, and relevant BCPs.[6]

---

## 10. Open Questions / Unverified Items

The following items remain Unverified due to lack of clause‑level text or absence of primary sources in the retrieved material:

1. **Exact Clause Text and “Shall” Wording in IS‑04 and IS‑05**  
   – Specific clauses defining required/optional behaviors for nodes, registries, and controllers are not visible; requirements in this report are inferred from high‑level summaries and should be confirmed by reviewing the full specifications.[2][7][9]

2. **Mandatory Transport Set for IS‑05**  
   – IS‑05 mentions support for RTP, WebSocket, MQTT, and other transports via a transports register, but it is unclear whether any particular transport (e.g., RTP) is mandatory for IS‑05 conformance.[2]  
   – For ST 2110 profiles, this is a critical question that must be resolved by examining the full IS‑05 text or associated transport documentation.

3. **Detailed Structure of IS‑05 Connection Resources**  
   – The specific JSON schemas or resource structures for connection requests/responses are not visible in the available snippets; tests and implementations must consult the full specification.[2][4]

4. **MS‑04 ID & Timing Model Details**  
   – MS‑04 is labeled “ID & Timing Model,” but its contents and any normative mapping between NMOS IDs and ST 2110 timing references are not visible.[3]  
   – Use of MS‑04 as a normative reference should be deferred until clause‑level review.

5. **BCP‑008 Stability and Versioning**  
   – BCP‑008‑01/‑02 are listed as work in progress; specific status schemas and semantics may change.[3][6]  
   – No clause‑level guarantees of stability are visible.

6. **TR‑1001‑1 Full Requirements**  
   – Beyond the statement that TR‑1001‑1 requires IS‑04 ≥1.2 and IS‑05 ≥1.0, additional requirements (e.g., specific endpoints, redundancy, or PTP integration) are not visible.[9]  
   – Precise TR‑1001‑1 conformance demands review of the full document.

7. **SMPTE ST 2110 Numerical Constraints and Their Integration with NMOS**  
   – No SMPTE ST 2110 primary documents are visible; bandwidth, timing, and packetization rules must not be inferred from NMOS materials.[11]  
   – Any formulas or numeric checks must come from ST 2110 documents or related SMPTE/EBU guidance, which are outside the scope of this report.

---

## 11. Sources

Numbers below correspond to inline citations in this report.

1. **AMWA NMOS Introduction / Specification Index** – Lists NMOS interface specifications (IS‑04 Discovery & Registration; IS‑05 Device Connection Management) and their stable releases and statuses; identifies IS‑04 v1.3.3 and IS‑05 v1.2.0 among releases.[1]  
2. **AMWA IS‑05 NMOS Device Connection Management Specification (overview)** – Describes IS‑05 as a transport‑independent way of connecting media nodes, supporting transports such as RTP, WebSocket, and MQTT via a transports register, and supporting single/bulk and immediate/delayed connections.[2]  
3. **NMOS Specifications by Document Type** – Catalog of NMOS Interface Specifications (IS), Media Specs (MS), and Best Current Practices (BCP), including IS‑04, IS‑05, IS‑08, MS‑04, MS‑05‑01/‑02/‑03, and BCP‑003/‑004/‑005/‑008.[3]  
4. **IS‑05 Interoperability: NMOS IS‑04** – States that the IS‑05 Connection Management API shares a data model with IS‑04 and is designed to be used alongside it.[4]  
5. *(Not used)* – Product datasheet referencing NMOS and ST 2110.  
6. **NMOS Testing Tool (nmos‑testing)** – Describes AMWA’s test suite for NMOS specifications and BCPs, listing IS‑04, IS‑05, IS‑07, IS‑08, and BCP‑003‑01/‑02/‑03, BCP‑004‑01/‑02, BCP‑005‑01, BCP‑008‑01/‑02 as covered.[6]  
7. **AMWA IS‑04 NMOS Discovery and Registration Specification (overview)** – Summarizes IS‑04, stating that it allows control and monitoring applications to find resources, and that media nodes locate the registry via DNS‑SD, register via HTTP + JSON, and expose resources via HTTP query and WebSocket subscription.[7]  
8. **AIMS “Introduction to NMOS”** – Introductory presentation listing IS‑04 and IS‑05 as stable NMOS specifications and contextualizing NMOS within SMPTE ST 2110 systems.[8]  
9. **NMOS FAQ** – Provides explanatory text about NMOS, clarifies that IS‑04 defines Registration and Query APIs and how to find endpoints while not constraining backend implementations, describes IS‑05 as managing logical connections between senders and receivers, and states that TR‑1001‑1 specifies IS‑04 ≥1.2 and IS‑05 ≥1.0.[9]  
10. **AMWA Info‑005 NMOS Controller Implementation Guide** – Describes how controllers use the IS‑04 Query API to discover resources and obtain updates, summarizes that IS‑04 comprises Registration, Query, and Node APIs, and notes that controllers use IS‑05 Connection API to make connections.[10]  
11. **Wikipedia: Networked Media Open Specifications** – Secondary source summarizing NMOS history and versions, stating that IS‑04 and IS‑05 are deemed critical for the success of ST 2110 and listing latest releases of IS‑04 (v1.3.3) and IS‑05 (v1.2.2).[11]  
12. **Tieline Gateway product information** – Vendor datasheet indicating real‑world deployment of AMWA NMOS IS‑04 and IS‑05 for discovery, registration, and control of ST 2110 AoIP streaming.[12]  
13. **QxP product datasheet** – Vendor datasheet referencing NMOS IS‑04, IS‑05, and IS‑09 for PTP resource detection, indicating NMOS usage in monitoring products.[13]  
14. **Blog: “AMWA NMOS – Building the Control Plane for SMPTE ST 2110 with Go”** – Secondary article describing NMOS as the control plane for ST 2110, explaining IS‑04 (discovery & registration) and IS‑05 (device connection management), and detailing how devices use DNS‑SD, HTTP, JSON, and WebSocket to interact with NMOS APIs.[14]  
15. **AMWA NMOS Roadmap PDF** – Roadmap document identifying NMOS core functionality, including IS‑04, IS‑05, IS‑08, and stream compatibility management, and placing them in the broader NMOS ecosystem.[15]