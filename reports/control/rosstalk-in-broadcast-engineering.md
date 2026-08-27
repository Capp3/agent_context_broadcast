```yaml
---
report_id: rosstalk-broadcast-control-protocol
title: Rosstalk Broadcast Control Protocol Technical Reference
topic: Rosstalk
report_version: 0.1.0
generated_date: 2026-08-27
source_access: public
source_cutoff: 2026-08-11
prompt_template: template-new-topic.md
prompt_template_version: 0.1.0
status: draft
---
```

## 1. Executive Summary

Rosstalk is a proprietary, plain-text control protocol defined and used by Ross Video across multiple products, including production switchers (e.g., Carbonite, Graphite, Acuity), XPression graphics, automation (OverDrive/Caprica), and ancillary devices such as UltriGPI.[1][3][5][7][8][12][14] Rosstalk is typically carried over TCP/IP and uses port 7788 as the default listening port on many Ross devices and third‑party integrations, with ASCII text commands terminated by a line delimiter per device-specific requirements.[5][8][9][14][15]  

There is no publicly positioned, cross‑product “Rosstalk standard” separate from Ross product manuals and protocol PDFs; instead, each device’s implementation is documented in its own protocol guides and help pages.[1][3][5][7][8][11][14] Implementers must therefore treat the Ross‑supplied product documentation as the normative source for command syntax, transport details, and limits, and must not assume that behavior on one product applies to another.[1][3][8][11][14]  

This report aggregates those sources into a structured reference, with explicit separation between normative requirements drawn from Ross documentation, implementation guidance inferred from those requirements, and unverified or ambiguous aspects of the protocol.

---

## 2. Scope and Boundaries

### 2.1 What Rosstalk Standardizes (Per Available Documentation)

Within the limits of publicly available documentation, Rosstalk standardizes the following for each supported Ross product:

- A plain-text command language that can be used to control Ross products (e.g., switcher operations, graphics actions, GPI triggers).[1][7][11][14]  
- Transport bindings for the command language:
  - TCP over IP, typically on port 7788 for many devices, designated for receiving Rosstalk commands.[5][8][9][14][15]  
  - Serial interfaces for some devices (e.g., protocol selection “Rosstalk” on a serial port).[4][11]  
- Command framing and termination rules (e.g., requirement for carriage-return/line-feed on Carbonite and XPression).[8][14]  
- Device‑specific behaviors such as framebuffer numbering conventions for XPression when controlled via Rosstalk (zero‑based index in Rosstalk versus one‑based in XPression user interface).[14]  
- Roles that devices can play in a Rosstalk ecosystem:
  - Listener/server: devices accepting incoming Rosstalk commands on a configured TCP port (often 7788).[5][8][9][14][15]  
  - Client: devices or controllers that initiate TCP connections and send Rosstalk commands (e.g., Caprica/OverDrive, UltriGPI).[2][5][12]  
  - Bridge: devices such as UltriGPI that relay Rosstalk commands or translate RossTalk “virtual GPIs” to physical GPIs.[12]  

### 2.2 What Rosstalk Does Not Explicitly Standardize

The available sources do **not** provide a single, cross-product Rosstalk specification that defines:

- A unified, versioned Rosstalk command set across all Ross products.[1][7][11][14]  
- Global encoding guarantees beyond references to “plain text” and ASCII usage in specific examples (e.g., telnet and ASCII‑capable applications).[7][8]  
- Maximum command lengths other than product‑specific limits (e.g., Caprica allows custom commands up to 32 characters for certain RossTalk XPression devices).[5]  
- Error responses, acknowledgments, or standardized reply formats across products; most public documentation focuses on outbound commands rather than device replies.[8][11][14]  
- Timing constraints (e.g., maximum command rate, required inter-command spacing, connection keepalive policies).[8][11]  

All such topics remain **Unverified** at the cross‑product “protocol” level and should be treated as implementation-specific until confirmed via Ross documentation for a given device.

### 2.3 Adjacent Protocols and Systems

Rosstalk commonly coexists with, or is bridged to, other protocols and systems:

- UltriGPI can operate as a Rosstalk client or server, and also supports TSL v3 and v5 for routing control, effectively bridging Rosstalk virtual GPIs to physical GPIs and other control systems.[12]  
- Some devices support both Rosstalk and other control mechanisms (e.g., generic GPI interfaces, TSL, or proprietary APIs), but those are outside the scope of this report.[12]  
- Third‑party applications such as Bitfocus Companion and Rascular control tools implement Rosstalk listeners/clients on TCP port 7788, but their documentation is **secondary** and not normative for Rosstalk protocol behavior.[9][10][15]  

### 2.4 Source Access Limitations

- Ross Video publishes protocol documents (e.g., “Rosstalk Commands” PDF) and online help material which appear to be freely accessible but may be noted as “archived (do not use)” in some cases; this raises uncertainty about their current normative status.[4][11][13]  
- The “Rosstalk Commands” document is an archived production switcher protocol reference and may not reflect current behavior on newer switchers or software versions.[4][11][13]  
- No paywalls or access controls were evident for the documents referenced here, but their archival status and lack of explicit versioning must be taken into account when treating them as normative.[4][11][13]  

---

## 3. Standards and Source Map

### 3.1 Primary and Secondary Documents

| # | Document (short name) | Version/date (as visible) | Role | Source status | Clause-level visibility |
|---|-----------------------|---------------------------|------|---------------|-------------------------|
| [11] | “Rosstalk Commands (4802DR‑403)” (production switcher protocol PDF; archived) | Labeled as archived; internal date not clearly exposed; file index updated 2026‑01‑05 | Primary protocol description for switchers; defines Rosstalk command syntax and use over Ethernet and serial | Primary, public, archived | Full PDF, clause‑level content visible; section numbering not referenced here |
| [1] | “Rosstalk” – Acuity device help | Online help page; last updated metadata 2026‑01‑20 | High‑level description of Rosstalk for Acuity; indicates plain‑text remote control | Primary, public | Full page visible; no formal clause numbers |
| [3] | “Generic Rosstalk Device” – device database help | Last updated 2026‑07‑16 | Describes switcher output of Rosstalk commands to external devices | Primary, public | Full page visible |
| [6] | “Generic Rosstalk Device” – Carbonite device help | Dated 2022; last updated 2025‑08‑31 | Carbonite configuration guide for Rosstalk output | Primary, public | Full page visible |
| [8] | “Carbonite/Graphite Commands” – Rosstalk section | Last updated 2026‑06‑02 | Normative instructions for controlling Carbonite/Graphite over Rosstalk (TCP, port 7788, CR/LF termination) | Primary, public | Full page visible |
| [14] | “XPression Commands” – Rosstalk section | Last updated 2026‑06‑10 | Normative instructions for controlling XPression via Rosstalk (port, termination, framebuffer numbering) | Primary, public | Full page visible |
| [5] | “Rosstalk XPression” – Caprica device setup sheet (PDF) | Last updated 2026‑03‑08 | OverDrive/Caprica configuration for sending Rosstalk to external devices; specifies TCP, default port 7788, and command length limits | Primary, public | Full PDF visible |
| [2] | “Run Rosstalk Commands from a Stream Deck” – OverDrive application note (PDF) | Last updated 2026‑04‑12 | Application note for using a Stream Deck to send Rosstalk via OverDrive | Primary (product‑specific), public | Full PDF visible |
| [7] | “Rosstalk” – Ross support article | Last updated 2025‑04‑03 | General overview: Rosstalk as plain‑text protocol used to control multiple Ross products | Primary, public | Full article visible |
| [12] | “UltriGPI” product description | Dated 2026‑04‑19; updated 2026‑06‑12 | Describes UltriGPI’s role as Rosstalk client/server and bridge | Primary, public | Full page visible |
| [9] | “Controlling Ross devices with Rosstalk” – Rascular support | Dated 2025‑03‑01 | Third‑party overview of Rosstalk as simple IP‑based protocol; reiterates port 7788 | Secondary, public | Full page visible |
| [10] | “Ross Video – Rosstalk” – Bitfocus Companion | Updated 2026‑06‑25 | Describes Companion’s Rosstalk connection module | Secondary, public | Full page visible |
| [15] | “Rosstalk Control” – Companion user guide | Last updated 2026‑08‑11 | Companion’s listener behavior; newline handling and fixed port 7788 | Secondary, public | Full page visible |

---

## 4. Normative Requirements Catalog

The following requirements are extracted from **primary** Ross documentation unless explicitly marked as secondary or derived. Requirement IDs (RT‑REQ‑xxx) are for reference only and have no standing in Ross documents themselves.

### 4.1 Requirements Table

| ID | Requirement or rule | Applies to | Normative citation | Normative / best practice / assumed / unverified | Implementation implication | Confidence |
|----|---------------------|-----------|---------------------|-----------------------------------------------|----------------------------|-----------|
| RT‑REQ‑001 | Rosstalk uses plain‑text commands to control Ross products such as switchers and graphics systems. | All Rosstalk controllers and devices | [1][5][7][11][14] | Normative (per Ross docs) | Implementations shall send human‑readable text (ASCII per device guidance) instead of binary framing. | High |
| RT‑REQ‑002 | Rosstalk commands can be sent to production switchers over an Ethernet connection. | Switchers (e.g., Carbonite, Graphite, Acuity) | [8][11] | Normative | Implementations shall support TCP/IP connectivity to the switcher for Rosstalk. | High |
| RT‑REQ‑003 | Carbonite accepts Rosstalk commands over Ethernet on TCP port 7788. | Carbonite/Graphite switchers | [8] | Normative (device‑specific) | Controllers targeting Carbonite shall connect to port 7788 unless reconfigured per product guidance. | High |
| RT‑REQ‑004 | XPression accepts Rosstalk commands over Ethernet on TCP port 7788. | XPression systems | [14] | Normative (device‑specific) | Controllers targeting XPression shall connect to port 7788 as configured in XPression’s TCP settings. | High |
| RT‑REQ‑005 | Caprica’s Rosstalk XPression device uses TCP as the transport protocol, with default remote port 7788 and Ethernet role “Client.” | OverDrive/Caprica Rosstalk XPression device | [5] | Normative (device‑specific) | OverDrive/Caprica shall initiate TCP connections from an ephemeral local port to remote port 7788 unless configured otherwise. | High |
| RT‑REQ‑006 | Each Rosstalk command sent to Carbonite/Graphite shall be terminated by a carriage return and a line feed (CR/LF). | Carbonite/Graphite Rosstalk listeners | [8] | Normative (device‑specific) | Implementations controlling Carbonite must append CR (0x0D) and LF (0x0A) to every command. | High |
| RT‑REQ‑007 | Each Rosstalk command sent to XPression shall be terminated by a carriage return and a line feed (CR/LF). | XPression Rosstalk listeners | [14] | Normative (device‑specific) | Implementations controlling XPression must append CR (0x0D) and LF (0x0A) to every command. | High |
| RT‑REQ‑008 | Commands can be sent via telnet or any application capable of sending ASCII commands to the switcher. | Carbonite/Graphite; likely other switchers | [8][11] | Normative (device‑specific) | Controllers may use generic TCP clients (e.g., telnet) so long as they provide correct ASCII command strings and CR/LF termination. | High |
| RT‑REQ‑009 | The default IP address of Carbonite for Rosstalk connections is 192.168.0.123. | Carbonite/Graphite | [8] | Normative (device‑specific) | Implementations may use 192.168.0.123 as initial default; installers must adjust to operational addressing. | High |
| RT‑REQ‑010 | For XPression, framebuffer 1 is selected by sending framebuffer 0 in Rosstalk, framebuffer 2 by sending 1, and so on (Rosstalk framebuffer indices are zero‑based while UI indices are one‑based). | XPression Rosstalk control | [14] | Normative (device‑specific) | Controllers shall apply an index offset when mapping UI framebuffer numbers to Rosstalk commands. | High |
| RT‑REQ‑011 | Caprica custom controls for Rosstalk XPression can send either preconfigured Rosstalk commands or typed commands up to 32 characters in length. | OverDrive/Caprica Rosstalk XPression device | [5] | Normative (Caprica‑specific) | Controllers implemented via Caprica must respect a 32‑character limit for typed custom Rosstalk commands. | High |
| RT‑REQ‑012 | A generic Rosstalk device on a switcher can output Rosstalk commands to any external device that accepts Rosstalk commands. | Switchers configured with generic Rosstalk device | [3][6] | Normative (device‑specific) | Switchers may act as Rosstalk controllers, sending outbound commands to other devices over TCP or serial as configured. | High |
| RT‑REQ‑013 | UltriGPI can operate as a client or server for Rosstalk and translate Rosstalk “virtual GPIs” to physical GPIs. | UltriGPI | [12] | Normative (product‑specific) | UltriGPI deployments may treat Rosstalk messages as virtual GPI triggers and must configure appropriate client/server roles. | High |
| RT‑REQ‑014 | Devices labeled “Rosstalk‑IN” on Caprica use a configurable port, with 7788 as default, to receive Rosstalk commands. | Caprica Rosstalk‑IN devices | [2] | Normative (product‑specific) | Implementers must configure Caprica Rosstalk‑IN devices with correct host and port, defaulting to 7788 if not overridden. | High |
| RT‑REQ‑015 | Companion’s Rosstalk listener uses TCP port 7788, which is fixed and cannot be changed, and accepts commands terminated by newline (\n) or CR/LF (\r\n). | Companion Rosstalk listener | [15] | Normative (Companion‑specific) | Controllers targeting Companion must use port 7788 and ensure lines end with at least LF; CR is optional. | High (for Companion); secondary for Rosstalk at large |
| RT‑REQ‑016 | When using multiple Rosstalk connections to a switcher, it is recommended to increment the port number for each device. | Carbonite/Graphite multi‑connection setups | [8] | Best practice (explicitly indicated as a tip) | Network designers should allocate distinct TCP ports per Rosstalk client when supported to avoid contention. | High |

---

## 5. Engineering Model

### 5.1 Core Entities and Roles

The Rosstalk ecosystem, as observable from Ross documentation, consists of the following key entities:

- **Rosstalk Controller**: Any system that originates Rosstalk commands, such as OverDrive/Caprica, Stream Deck integrations, UltriGPI when acting as client, or third‑party applications.[2][5][8][12][15]  
- **Rosstalk Device**: A Ross or third‑party device that accepts Rosstalk commands—e.g., Carbonite, Graphite, Acuity, XPression, Caprica Rosstalk‑IN, Companion listeners.[1][5][7][8][14][15]  
- **Rosstalk Bridge**: Devices such as UltriGPI or a generic Rosstalk device on a switcher that receive Rosstalk commands and convert them into other actions (e.g., GPIs, downstream commands).[3][6][12]  

A simplified flow is illustrated below (diagram is conceptual; all behaviors must be validated against device‑specific documents):

```mermaid
flowchart LR
    C1((Controller\n(e.g., Caprica))) -->|TCP port 7788| D1[(Switcher\n(Carbonite))]
    C2((Controller\n(e.g., Companion))) -->|TCP port 7788| D2[(XPression)]
    D1 -->|Generic Rosstalk\nOutput| B1[(External Device\n(e.g., Router))]
    B2[(UltriGPI)] -->|Rosstalk Client/Server| D3[(Other Rosstalk Devices)]
```

Textual assertions here are supported as follows: switchers accept Rosstalk commands over TCP port 7788 and can also output Rosstalk commands via generic devices; UltriGPI can function as client or server for Rosstalk.[3][5][8][12][14]  

### 5.2 Transport Model

**TCP/IP Transport**  

- Many Ross devices accept Rosstalk commands over TCP on port 7788 by default (Carbonite/Graphite, XPression, Caprica Rosstalk‑IN, UltriGPI configurations, third‑party listeners).[5][8][9][14][15]  
- Some devices (e.g., Caprica Rosstalk‑IN, Caprica Rosstalk XPression) allow their listening or remote ports to be configured, with recommended defaults of 7788.[2][5]  
- Companion’s listener uses port 7788 and does not allow port changes.[15]  
- Commands are sent as plain text over a TCP stream; no explicit framing beyond line termination is described.[5][8][14][15]  

**Serial Transport**

- The Rosstalk Commands PDF indicates that a serial port can be configured with the “Rosstalk” protocol, implying a serial transport variant.[4][11]  
- Specific serial port parameters (baud rate, data bits, parity, stop bits, flow control) are **not** exposed in the accessible snippets and remain **Unverified**.[4][11]  

### 5.3 Command Framing and Encoding

- Carbonite and XPression documentation explicitly state that each command should be terminated by a carriage return and line feed (CR/LF).[8][14]  
- Companion’s Rosstalk listener requires a newline character (LF) and accepts either LF alone or CR/LF.[15]  
- Carbonite documentation states that commands can be sent via telnet or any application that can send ASCII commands, implying ASCII encoding.[8]  
- No primary Ross document explicitly mandates UTF‑8 or any other extended character set for Rosstalk; absence of such a statement leaves multi‑byte character behavior **Unverified**.[7][8][11]  

From this, the minimal interoperable model is:

- A Rosstalk message is an ASCII text line containing a command string, terminated by a line delimiter acceptable to the target device:
  - For Carbonite/Graphite and XPression: CR+LF is required.[8][14]  
  - For Companion: LF alone is sufficient; CR may be optional.[15]  

### 5.4 Behavioral Semantics and Device‑Specific Variants

- Switchers (e.g., Carbonite) can be both Rosstalk devices (accepting commands) and controllers (outputting commands via generic Rosstalk devices).[3][6][8][11]  
- XPression maps Rosstalk commands to internal operations such as Take, Next, sequencer navigation, and GPI triggers.[14]  
- UltriGPI uses Rosstalk messages as virtual GPIs and can bridge them to physical GPI outputs or other software control paths.[12]  
- Caprica and OverDrive use Rosstalk to control external applications/devices (e.g., XPression) and can also expose Rosstalk‑IN endpoints for remote control of OverDrive sequences, as described in Stream Deck integration notes.[2][5]  

Where behavior (e.g., specific command strings, response messages) is not explicitly documented in the referenced material, it remains **Unverified** and must be determined from the full product protocol manuals or direct observation.

---

## 6. Formulas, Calculations, and Worked Examples

Rosstalk is primarily a textual protocol; only one explicit numeric mapping can be safely derived from the accessible documentation.

### 6.1 Formula and Assumption Register

| Calculation name | Formula or method | Inputs and units | Source citation | Normative or assumed | Worked example available | Confidence |
|------------------|-------------------|------------------|-----------------|----------------------|--------------------------|-----------|
| XPression framebuffer index mapping | \( \text{XPression\_fb} = \text{Rosstalk\_fb} + 1 \) | Rosstalk framebuffer index (integer, zero‑based); XPression framebuffer index (integer, one‑based) | [14] | Normative (derived directly from Ross documentation wording) | Yes | High |

**Justification**: XPression documentation states that to select framebuffer 1, Rosstalk must specify framebuffer 0; for framebuffer 2, Rosstalk uses 1, “and so on.”[14] This implies a general rule that Rosstalk indices are zero‑based while XPression UI indices are one‑based.[14]  

### 6.2 Worked Example: XPression Framebuffer Mapping

**Problem**: A controller must select XPression framebuffer 3 using a Rosstalk command.  

**Given**:  
- XPression framebuffer indices in the user interface are one‑based.[14]  
- Rosstalk framebuffer indices are zero‑based and map as described above.[14]  

**Calculation**:  

\[
\text{Rosstalk\_fb} = \text{XPression\_fb} - 1
\] [14]

For framebuffer 3:

\[
\text{Rosstalk\_fb} = 3 - 1 = 2
\] [14]

**Result**: The controller must send a Rosstalk command specifying framebuffer index 2 to affect XPression framebuffer 3.[14]  

The exact command string syntax (e.g., command keyword, parameter delimiters) is **Unverified** from the accessible snippets and must be obtained from the full XPression Rosstalk command reference.

---

## 7. Interoperability Risks and Ambiguity Register

### 7.1 Risk and Ambiguity Table

| Risk or ambiguity | Evidence | Normative status | Failure symptom | Mitigation or modeling rule |
|-------------------|----------|------------------|-----------------|-----------------------------|
| Line termination differences across devices | Carbonite and XPression require CR/LF; Companion listener accepts LF or CR/LF.[8][14][15] | Normative (per device docs); cross‑product behavior ambiguous | Commands not executed, or partial commands buffered indefinitely, when only LF or only CR is used against devices expecting CR/LF | Controllers shall implement device‑specific termination policies: CR/LF for Ross switchers and XPression; LF or CR/LF for Companion; do not assume LF alone is universally acceptable. |
| Single, unified Rosstalk specification absent | Ross provides product‑specific protocol docs; no cross‑product Rosstalk versioning is exposed.[1][7][8][11][14] | Unverified (no explicit statement) | Implementers reuse commands or behaviors from one product and encounter undefined or conflicting behavior on another | Treat each product’s protocol document as authoritative for that product; do not generalize command sets or behaviors across products without explicit Ross documentation. |
| Archived status of “Rosstalk Commands” PDF | The production switcher protocol PDF appears in an archived manuals section labeled “DO NOT USE.”[4][11][13] | Ambiguous normative status | Implementations depend on outdated or superseded command syntax or behaviors | Treat archived documents as historical; verify against current product firmware documentation or Ross support before relying on them; maintain a version mapping in implementation context. |
| Serial transport parameters unspecified | Rosstalk can be selected as a serial protocol; serial parameters (baud, parity, etc.) are not visible in snippets.[4][11] | Unverified | Serial connections fail to establish or operate correctly | Obtain full serial configuration from device manuals or Ross support; do not assume default serial settings. |
| Character encoding beyond ASCII | Documents refer to ASCII commands and plain text, but do not explicitly define encoding beyond ASCII range.[7][8][11] | Unverified | Non‑ASCII characters (e.g., Unicode) may be misinterpreted or rejected | Limit Rosstalk commands to ASCII characters unless explicitly documented otherwise for a specific device; treat extended characters as unsafe. |
| Framebuffer numbering offset | XPression uses 1‑based UI numbering; Rosstalk uses zero‑based indices; offset described only in XPression documentation.[14] | Normative (device‑specific) | Commands sent to incorrect framebuffer, causing wrong graphics channel to be triggered | Implement a deterministic mapping between UI and Rosstalk indices as per Section 6; validate mappings in tests. |
| Port 7788 default vs fixed | Many Ross devices use port 7788, often configurable; Companion fixes the port at 7788 and does not allow changes.[5][8][14][15] | Normative (device‑specific) | Connection failures due to misconfigured ports; contention if multiple services share port 7788 | Maintain a per‑device port configuration registry; treat 7788 as default only where documented; for Companion, enforce port 7788 in network design. |
| Maximum command length | Caprica Rosstalk XPression devices limit custom typed commands to 32 characters; other devices’ limits are not documented in accessible snippets.[5] | Normative (Caprica‑specific); Unverified elsewhere | Commands truncated or rejected on devices with shorter limits; buffer overflows in poorly implemented listeners | Enforce a conservative maximum command length (≤32 characters) in Caprica custom controls; for other devices, consult full docs and treat limits as device‑specific. |
| Multi‑connection and port incrementing guidance | Carbonite documentation “recommends” incrementing ports when multiple Rosstalk connections are used.[8] | Best practice (explicitly labeled as a tip) | Port conflicts, connection drops, or unexpected sharing of command streams | When supported, allocate unique listening ports for each Rosstalk connection to a switcher; model this explicitly in configuration schemas. |

---

## 8. Implementation Guidance

This section provides **non‑normative**, implementation‑oriented guidance derived from the requirements above. All “must/shall” below are scoped to this guidance and do not introduce new normative protocol obligations beyond the cited documents.

### 8.1 Baseline Rosstalk Sender Implementation

For a generic Rosstalk controller targeting Carbonite and XPression:

1. **Transport selection**  
   - Use TCP as the transport protocol.[5][8][14]  
   - Configure the remote address and port:
     - Carbonite default: IP 192.168.0.123, port 7788.[8]  
     - XPression: configured IP with TCP port 7788 by default.[14]  

2. **Connection handling**  
   - Establish a persistent TCP connection per controlled device.  
   - For Carbonite, consider following the recommendation to increment port numbers for multiple Rosstalk connections if the device is configured to listen on multiple ports.[8]  

3. **Command encoding and framing**  
   - Encode command strings in ASCII.[8]  
   - Append CR (0x0D) and LF (0x0A) to every command for Carbonite and XPression.[8][14]  
   - Ensure that internal buffers treat CR/LF as the only terminators when targeting Ross devices documented as requiring CR/LF.[8][14]  

4. **Per‑device considerations**  
   - Apply framebuffer index offsets for XPression as per Section 6.[14]  
   - If using Caprica as a controller:
     - Respect the 32‑character limit for typed custom commands to Rosstalk XPression devices.[5]  
     - Configure Caprica Rosstalk‑IN and Rosstalk XPression devices with correct IP/port according to network design.[2][5]  

### 8.2 Baseline Rosstalk Listener Implementation (Non‑Ross Devices)

For third‑party applications implementing a Rosstalk listener, the following practices increase interoperability:

- **Port choices**  
  - Default to TCP port 7788 to align with common Ross and third‑party practice.[5][8][9][14][15]  
  - Allow configuration of the listening port unless constrained by product requirements (Companion is a counterexample).[15]  

- **Line termination**  
  - Accept both CR/LF and LF‑only terminations, mirroring Companion behavior to maximize compatibility.[15]  
  - Treat CR without LF as **invalid or incomplete**, as no Ross document explicitly supports CR‑only termination.[8][14][15]  

- **Encoding**  
  - Accept ASCII commands and treat non‑ASCII bytes cautiously; either reject them or handle via explicit, documented extensions.[7][8]  

- **Security and robustness** (derived guidance; not protocol normative)  
  - Implement length checks on inbound command lines (e.g., reject or truncate lines exceeding a configured maximum length such as 256 characters), given that some Ross controllers have known shorter limits (32 characters).[5]  
  - Log malformed commands and connection anomalies for diagnostics and future tuning.  

### 8.3 Modeling Unverified or External Values

Within AI or schema‑driven implementations:

- Represent device‑specific parameters (e.g., maximum command length, supported commands, serial parameters) as **externally supplied** configuration fields with provenance tags (e.g., “from Carbonite vX.Y protocol manual”).  
- Mark fields whose values cannot be confirmed from the cited sources (e.g., default serial parameters) as **Unverified** and avoid defaulting them in auto‑generated code.  

### 8.4 Configuration Schema Suggestions

A generic Rosstalk device configuration schema can include:

- `role`: {`controller`, `listener`, `bridge`} – must match device capabilities.[3][5][12]  
- `transport`:
  - `type`: {`tcp`, `serial`}.[4][5][8][11]  
  - `tcp.host`: IP or hostname.  
  - `tcp.port`: positive integer; default 7788 for many devices.[5][8][14][15]  
  - `serial.port`, `serial.baud`, `serial.parity`, etc. (values Unverified from the cited snippets).[4][11]  
- `line_termination`: {`CRLF`, `LF`, `CRLF_OR_LF`} with device‑specific defaults:
  - `CRLF` for Carbonite/XPression.[8][14]  
  - `CRLF_OR_LF` for Companion.[15]  
- `encoding`: default `ASCII`; extended encodings only when documented. [7][8]  
- `max_command_length`: optional integer; e.g., 32 for Caprica Rosstalk XPression typed commands.[5]  
- `framebuffer_index_offset`: integer; default 1 for XPression (UI index = Rosstalk index + 1).[14]  

---

## 9. Validation Checklist

The following checklist is intended for implementers and AI systems to validate Rosstalk integrations.

1. **Transport and addressing**
   - [ ] Confirm the target device supports TCP Rosstalk and identify whether serial Rosstalk is also needed.[4][5][8][11]  
   - [ ] Confirm correct IP address and port; default 192.168.0.123:7788 for Carbonite only, if still applicable.[8]  
   - [ ] Confirm whether the listening port is configurable and whether multiple ports are used for multi‑connection setups.[5][8]  

2. **Line termination**
   - [ ] For Carbonite/Graphite, ensure every command ends with CR/LF.[8]  
   - [ ] For XPression, ensure every command ends with CR/LF.[14]  
   - [ ] For Companion or other third‑party listeners, verify whether LF‑only termination is acceptable; Companion accepts LF or CR/LF.[15]  

3. **Encoding**
   - [ ] Confirm commands are encoded as ASCII; verify if any device requires or supports extended encodings.[7][8]  

4. **Device roles**
   - [ ] Determine whether the device is acting as Rosstalk controller, listener, or bridge (e.g., generic Rosstalk device, UltriGPI).[3][6][12]  
   - [ ] Verify client/server configuration for devices like UltriGPI and Caprica.[5][12]  

5. **Command semantics**
   - [ ] Obtain the current, device‑specific Rosstalk command reference (e.g., switcher protocol PDF, XPression Rosstalk commands).[11][14]  
   - [ ] Verify that command strings and parameters match the device’s command reference.  

6. **Index mappings**
   - [ ] For XPression, confirm framebuffer index mapping (Rosstalk zero‑based vs UI one‑based) is correctly implemented.[14]  

7. **Limits**
   - [ ] For Caprica Rosstalk XPression, ensure typed commands do not exceed 32 characters.[5]  
   - [ ] Confirm any device‑specific rate limits or maximum line length from non‑public documentation or testing (Unverified in the accessible materials).  

8. **Archived vs current documentation**
   - [ ] Check whether the Rosstalk protocol document being used is current or archived; the “Rosstalk Commands” PDF is labeled archived.[4][11][13]  
   - [ ] Validate critical behavior against current firmware or Ross support if only archived documents are available.  

---

## 10. Open Questions / Unverified Items

The following items could not be conclusively determined from the accessible sources and should be treated as **Unverified** until corroborated by official documentation or vendor communication:

1. **Global Rosstalk versioning**  
   - No evidence of a protocol version field or a unified version number across products.[1][7][11][14]  

2. **Comprehensive command set**  
   - The full set of commands and parameters for each device (including responses, error codes, and acknowledgments) requires inspection of complete product protocol manuals; only partial descriptions are visible in the snippets.[11][14]  

3. **Serial transport parameters**  
   - Baud rate, data bits, parity, stop bits, and flow control settings for serial Rosstalk are not defined in the accessible excerpts.[4][11]  

4. **Encoding beyond ASCII**  
   - Whether devices accept multi‑byte encodings such as UTF‑8 or treat non‑ASCII bytes as invalid is not specified.[7][8][11]  

5. **Maximum command length for devices other than Caprica Rosstalk XPression**  
   - The 32‑character limit is documented only for Caprica’s typed custom commands.[5]  

6. **Timing and rate limits**  
   - No published limits on command frequency or recommended inter‑command delays were found in the accessible material.[8][11]  

7. **Formal deprecation or replacement of the archived “Rosstalk Commands” document**  
   - The “archived (do not use)” status suggests superseding documentation exists, but that replacement is not identified in the accessible snippets.[4][11][13]  

8. **Security expectations**  
   - No explicit guidance on authentication, encryption, or access control for Rosstalk over IP is visible in the cited sources.[5][8][11][14]  

Implementations should document how they address each of these open questions (e.g., via local policy, testing, or vendor clarification) and maintain traceability to those decisions.

---

## 11. Sources

Numbers correspond to inline citations.

1. **“Rosstalk” – Acuity Device Help** (Ross Video online help). Describes Rosstalk as a plain‑text protocol enabling external control of the switcher. Primary, public.  
2. **“Run Rosstalk Commands from a Stream Deck – Ross Video”** (OverDrive application note, PDF). Explains configuring OverDrive/Caprica and a Stream Deck plugin to send Rosstalk commands, including host/port and Rosstalk‑IN device configuration. Primary, public.  
3. **“Generic Rosstalk Device” – Device Database Help** (Ross Video). States that the generic device allows the switcher to output Rosstalk commands to any device that accepts Rosstalk. Primary, public.  
4. **“Rosstalk Commands” – Production Switchers Protocol Document (archived manuals section)** (Ross Video). Indicates that a serial port can be set to “Rosstalk” protocol and that commands can be sent over Ethernet. Primary but archived, public.  
5. **“Rosstalk XPression” – Caprica Device Setup Sheet** (Ross Video, PDF). Defines configuration of a Rosstalk XPression device on Caprica, including TCP, default remote port 7788, Ethernet role Client, and 32‑character limit for typed commands. Primary, public.  
6. **“Generic Rosstalk Device – Carbonite Device Help”** (Ross Video). Describes selecting Rosstalk as a device type and configuring the switcher to output Rosstalk commands. Primary, public.  
7. **“Rosstalk” – Ross Video Support Article**. Presents Rosstalk as a plain‑text protocol for controlling different Ross products. Primary, public.  
8. **“Carbonite/Graphite Commands – Rosstalk”** (Ross Video online help). Specifies that Carbonite accepts Rosstalk commands over Ethernet on port 7788, requires CR/LF termination, can be controlled via telnet/ASCII apps, provides default IP 192.168.0.123, and recommends port incrementing for multiple connections. Primary, public.  
9. **“Controlling Ross Devices with Rosstalk” – Rascular Support Article**. Secondary overview describing Rosstalk as a simple IP‑based control protocol and noting that devices listen on TCP port 7788. Secondary, public.  
10. **“Ross Video – Rosstalk” – Bitfocus Companion Connection Module**. Secondary document describing Companion’s Rosstalk connection configuration and module behavior. Secondary, public.  
11. **“Rosstalk Commands (4802DR‑403)” – Ross Video Protocol PDF**. Production switcher protocol document stating that switchers can be controlled from remote editors/computers via Rosstalk over Ethernet. Appears in archived manuals. Primary but archived, public.  
12. **“UltriGPI” – Ross Video Product Page**. Describes UltriGPI as able to operate as a client or server for Rosstalk and TSL protocols, bridging virtual GPIs with physical GPIs and software control systems. Primary, public.  
13. **Duplicate/archived copy of “Rosstalk Commands”** (Ross Video). Another indexed copy of the Rosstalk Commands PDF within an archived manuals collection. Primary but archived, public.  
14. **“XPression Commands – Rosstalk”** (Ross Video online help). Describes controlling XPression via Rosstalk on TCP port 7788, CR/LF termination, operations like Take/Next and GPI triggering, and the framebuffer numbering offset between Rosstalk and XPression UI. Primary, public.  
15. **“Rosstalk Control” – Companion User Guide**. Secondary document explaining that Companion can be triggered by Rosstalk commands over TCP port 7788 (fixed), with commands terminated by newline (LF) or CR/LF, and that the listener must be enabled in settings. Secondary, public.