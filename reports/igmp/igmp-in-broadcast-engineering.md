<pre><code class="yaml language-yaml">---
report_id: igmp-broadcast-engineering-reference
title: IGMP Technical Reference for Broadcast Engineering
topic: IGMP
report_version: 0.1.0
generated_date: 2026-08-27
source_access: public
source_cutoff: 2026-08-26
prompt_template: template-new-topic.md
prompt_template_version: 0.1.0
status: draft
---
</code></pre>
<h2 id="1executivesummary">1. Executive Summary</h2>
<p>Internet Group Management Protocol (IGMP) is the IPv4 host-to-router signaling protocol used to report multicast group memberships on directly attached networks and to control delivery of multicast traffic to receivers.[2][4][7][8][12][15] IGMP has three standardized versions—IGMPv1 (RFC 1112), IGMPv2 (RFC 2236), and IGMPv3 (RFC 3376)—with IGMPv3 being the current standards-track version providing source-filtered multicast (Any-Source Multicast and Source-Specific Multicast) control.[2][4][7][12][15]</p>
<p>For broadcast engineering, IGMP defines how receivers (set‑top boxes, contribution encoders, monitoring hosts) signal interest in program multicast streams to edge routers, how routers query membership, and the timing model for join/leave convergence; multicast routing (e.g., PIM) and transport formats (e.g., MPEG-TS, SMPTE) are explicitly out of scope.[2][4][7][12] This report catalogs normative requirements from the IGMP RFCs, associated timing formulas, and implementation guidance for reliable, deterministic multicast behavior in broadcast networks.</p>
<hr />
<h2 id="2scopeandboundaries">2. Scope and Boundaries</h2>
<h3 id="21whatigmpstandardizes">2.1 What IGMP Standardizes</h3>
<ol>
<li>Host-to-router signaling of IPv4 multicast group membership on a directly attached subnet, via IGMP messages encapsulated in IP datagrams with protocol number 2.[2][8]  </li>
<li>Message formats (fields, codes, checksums) and semantics for:</li>
</ol>
<ul>
<li>Membership Query messages.[2][4][7][8][15]</li>
<li>Membership Report messages (v1/v2).[2][4][8]</li>
<li>Membership Report and Group Record messages (v3).[7][15]</li>
<li>Leave Group messages (v2).[4][10]</li>
</ul>
<ol>
<li>Version-specific behavior for hosts and routers, including:</li>
</ol>
<ul>
<li>How hosts respond to queries and suppress redundant reports.[2][4][7][8]</li>
<li>How routers elect the querier and time out group membership.[4][7][15]</li>
<li>How IGMPv3 hosts express source-filtered interest (INCLUDE/EXCLUDE semantics).[7][9][15]</li>
</ul>
<ol>
<li>Timer and interval calculations for group membership, querier election, and leave confirmation (IGMPv2/IGMPv3).[4][7][15]</li>
<li>Interoperability rules between versions (e.g., how routers distinguish IGMPv1/v2/v3 queries by length and Max Resp Code).[7][15]</li>
</ol>
<h3 id="22whatigmpdoesnotstandardize">2.2 What IGMP Does Not Standardize</h3>
<ol>
<li>Multicast routing algorithms or protocols (e.g., PIM, DVMRP, MOSPF); IGMP is explicitly described as a host/router signaling protocol, with routing left to separate specifications.[2][4][7]  </li>
<li>IPv6 multicast membership; this is handled by Multicast Listener Discovery (MLD) defined in separate RFCs, not covered by IGMP.[7][12]  </li>
<li>Application-layer stream formats (MPEG-TS, RTP, audio/video codecs) or channel/service mapping; IGMP is agnostic to payload.[2][7][13]  </li>
<li>QoS treatment, policing, or traffic shaping of multicast streams; these are implementation and network policy issues.[6][13][14]  </li>
<li>Security mechanisms beyond basic behavior (e.g., authentication of IGMP messages, encryption of multicast); IGMP assumes a trusted local subnet.[2][4][7]</li>
</ol>
<h3 id="23adjacentstandardsandcommonmisconceptions">2.3 Adjacent Standards and Common Misconceptions</h3>
<ol>
<li>IGMP is part of the IPv4 multicast architecture (STD 5), alongside host extensions for IP multicasting defined in RFC 1112.[2][8][14]  </li>
<li>IGMP is often mistakenly equated with “multicast routing”; in reality, multicast routing protocols rely on IGMP state at router interfaces but are specified independently.[5][6][14]  </li>
<li>IGMPv3 introduces source filtering for IPv4 multicast; Source-Specific Multicast (SSM) behavior also depends on routing protocol profiles and address conventions, not IGMP alone.[7][9][12][15]  </li>
<li>IGMP operation is constrained to the local subnet (IP TTL = 1 for queries), countering the misconception that IGMP messages propagate across the routed domain.[2][5][8]  </li>
</ol>
<h3 id="24sourceaccesslimitations">2.4 Source Access Limitations</h3>
<p>All primary IGMP specifications (RFC 1112, RFC 2236, RFC 3376) are publicly available IETF standards-track documents.[2][4][7][8][11][12][15] Secondary materials (vendor documentation, books, and blogs) provide clarifying guidance but are not normative.[1][5][6][13][14] No paywalled IGMP normative text was identified in the referenced materials.</p>
<hr />
<h2 id="3standardsandsourcemap">3. Standards and Source Map</h2>
<h3 id="31primarystandards">3.1 Primary Standards</h3>
<ul>
<li>RFC 1112: “Host Extensions for IP Multicasting” (STD 5), August 1989; defines IGMPv1 within IPv4 multicast host extensions.[2][6][8][14]  </li>
<li>RFC 2236: “Internet Group Management Protocol, Version 2”, November 1997; updates RFC 1112 and specifies IGMPv2.[3][4][10][11]  </li>
<li>RFC 3376: “Internet Group Management Protocol, Version 3”, October 2002; obsoletes RFC 2236 and specifies IGMPv3.[7][12][15]  </li>
</ul>
<h3 id="32secondarysourcesnonnormative">3.2 Secondary Sources (Non-normative)</h3>
<ul>
<li>Cisco IGMPv3 Host Stack documentation (implementation guidance).[1]  </li>
<li>Columbia University IGMP summary page (historical and behavioral overview).[5]  </li>
<li>O’Reilly “Internet Core Protocols” book (context for IPv4 multicast).[6]  </li>
<li>NISP and vendor documentation (Nokia, IONOS) summarizing versions and operational guidance.[13][14]  </li>
<li>Japanese and Russian-language expositions of RFC 3376 and RFC 2236.[3][9][10][15]</li>
</ul>
<h3 id="33standardsandsourcemaptable">3.3 Standards and Source Map Table</h3>
<p>| Document                            | Version/date        | Role                                 | Source status      | Clause-level visibility          |
|-------------------------------------|---------------------|--------------------------------------|--------------------|----------------------------------|
| RFC 1112 – Host Extensions for IP Multicasting (STD 5)[2][8][14] | August 1989          | Primary spec for IGMPv1 and IPv4 multicast host behavior | Normative, standards-track | Full text publicly available; IGMP defined from host perspective |
| RFC 2236 – IGMP Version 2[4][10][11] | November 1997       | Primary spec for IGMPv2; updates RFC 1112 | Normative, standards-track | Full text publicly available; includes timer definitions and router behavior |
| RFC 3376 – IGMP Version 3[7][12][15] | October 2002        | Primary spec for IGMPv3; obsoletes RFC 2236 | Normative, standards-track | Full text publicly available; includes source filtering and detailed timing formulas |
| Cisco IGMPv3 Host Stack doc[1]      | ~2000s (exact date secondary) | Implementation guidance for IGMPv3 host behavior | Secondary, non-normative | Partial behavioral description; does not replace RFCs |
| Columbia IGMP overview[5]           | ~1990s (updated later) | Educational summary of IGMP versions | Secondary, non-normative | High-level summary; limited clause-level detail |
| O’Reilly “Internet Core Protocols”[6] | 2000                | Descriptive reference on IPv4 multicast and IGMP | Secondary, non-normative | Book chapter; not structured as clauses |
| NISP IGMPv3 summary[12]             | 2002                | Profile summary; highlights v3 role and obsoleting of v2 | Secondary, non-normative | High-level description only |
| IONOS IGMP article[13]              | 2018                | Introductory explanation of IGMP and multicast | Secondary, non-normative | Conceptual overview |
| Nokia multicast IGMP chapter[14]    | ~2020s              | Vendor implementation guidance       | Secondary, non-normative | Product-focused feature description |
| Russian/other translations of RFC 2236 &amp; RFC 3376[3][9][10][15] | 1997 / 2002 translations | Accessible presentations of primary specs | Secondary, but reflect normative content | Clause-level visibility where translation aligns with original RFCs |</p>
<p>Confidence in primary RFCs is High; secondary sources are used only for clarification and operational context, not for normative rules.</p>
<hr />
<h2 id="4normativerequirementscatalog">4. Normative Requirements Catalog</h2>
<h3 id="41keybehavioralrequirementsnarrative">4.1 Key Behavioral Requirements (Narrative)</h3>
<ol>
<li>IGMP is an integral part of IPv4 and is required for hosts implementing level 2 of the IP multicasting specification.[2][6][8]  </li>
<li>IGMP messages are carried in IP datagrams with protocol number 2 and typically use TTL = 1 to constrain messages to the local subnet.[2][5][8]  </li>
<li>Multicast routers periodically send Membership Query messages to discover which groups have members on the attached networks.[2][4][5][8]  </li>
<li>Hosts must send Membership Reports when they join a multicast group, in response to queries, and may suppress redundant reports upon hearing another host’s report.[2][4][5][8]  </li>
<li>IGMPv2 introduces Leave Group messages and faster leave processing; routers must send Group-Specific Queries upon receiving a Leave to confirm whether any members remain.[4][10]  </li>
<li>IGMPv3 defines source-filtered membership with INCLUDE and EXCLUDE lists; hosts must encode these in Group Records, and routers must interpret and merge them to control traffic.[7][9][15]  </li>
<li>IGMPv3 obsoletes IGMPv2; IGMPv3-capable systems must handle interoperable behavior with IGMPv1/v2 where required (e.g., query format detection).[7][12][15]</li>
</ol>
<h3 id="42normativerequirementstable">4.2 Normative Requirements Table</h3>
<p>| ID              | Requirement or rule                                                                                                  | Applies to              | Normative citation        | Normative / best practice / assumed / unverified | Implementation implication                                                     | Confidence |
|-----------------|----------------------------------------------------------------------------------------------------------------------|-------------------------|---------------------------|-----------------------------------------------|-------------------------------------------------------------------------------|-----------|
| IGMP-REQ-001    | IGMP must be implemented by all hosts conforming to level 2 of the IP multicasting specification.                    | IPv4 hosts              | RFC 1112, Sec. 6[2][8]    | Normative                                      | Multicast-capable hosts must support IGMP; otherwise they cannot participate in groups. | High      |
| IGMP-REQ-002    | IGMP messages must be encapsulated in IP datagrams with protocol number 2.                                          | Hosts, routers          | RFC 1112, IGMP intro[2][8] | Normative                                     | Packet classifiers and firewalls must recognize protocol 2 as IGMP.          | High      |
| IGMP-REQ-003    | Membership Query messages must be addressed to the all-hosts group 224.0.0.1 with TTL = 1.                          | Routers                 | RFC 1112; Columbia summary[2][5][8] | Normative (RFC); secondary corroboration | Queries do not cross routing boundaries; broadcast engineers must ensure local delivery. | High      |
| IGMP-REQ-004    | Hosts must respond to Membership Queries with Membership Reports for each group they belong to, subject to suppression. | Hosts                | RFC 1112; RFC 2236[2][4][8] | Normative                                    | Host stacks need timers and suppression logic to avoid report storms.        | High      |
| IGMP-REQ-005    | IGMPv2 routers must support Leave Group messages and send Group-Specific Queries on receiving a Leave.             | Routers (v2)            | RFC 2236 core sections[4][10] | Normative                                    | Faster leave convergence; routers must implement extra state and timers.     | High      |
| IGMP-REQ-006    | IGMPv3 hosts must support source-filtering and encode INCLUDE/EXCLUDE mode with source lists in Group Records.      | Hosts (v3)              | RFC 3376 core sections[7][9][15] | Normative                                   | Host APIs must expose per-group source lists; router logic must process them. | High      |
| IGMP-REQ-007    | IGMPv3 must be used by IP hosts to report multicast group memberships to routers, and it obsoletes RFC 2236.        | Hosts, routers          | RFC 3376 abstract, status[7][12][15] | Normative                                   | New designs should target IGMPv3; IGMPv2 support is legacy/interoperability only. | High      |
| IGMP-REQ-008    | Routers must periodically send General Queries to maintain group membership state; timing parameters must follow RFC defaults or configured values. | Routers | RFC 2236; RFC 3376[4][7][15] | Normative (existence); default values normative | Query timers must be tuned for convergence vs control-plane load.          | High      |
| IGMP-REQ-009    | IGMP query version can be determined from size and Max Resp Code: v1: 8 octets with Max Resp Code = 0; v2: 8 octets with non-zero Max Resp Code; v3: ≥12 octets. | Hosts, routers | RFC 3376, query version rules[7][15] | Normative | Mixed-version networks must parse according to this rule to avoid misclassification. | High |
| IGMP-REQ-010    | IGMP behavior is specified from host perspective; router-specific usage is not fully detailed in RFC 1112.          | Hosts, routers          | RFC 1112 intro[2][8]      | Normative (scope), best-practice for routers | Router implementations rely on additional multicast routing specs; IGMP alone is insufficient guidance. | High |
| IGMP-REQ-011    | IGMPv1 defines two host message types: Host Membership Query (type 1) and Host Membership Report (type 2).          | Hosts (v1)              | RFC 1112 IGMP section[2][8] | Normative                                   | Packet decoders and analyzers must classify IGMPv1 by type field.           | High      |
| IGMP-REQ-012    | Routers and hosts must treat IGMP as asymmetric (specified from host point of view); router behavior may be symmetric but is not fully standardized in RFC 1112. | Hosts, routers | RFC 1112 IGMP intro[2][8] | Normative (scope), assumed for routers | Router vendors must rely on multicast routing specs and implementation practice. | High |
| IGMP-REQ-013    | IGMPv3 uses Group Records in Membership Reports to convey per-group, per-source membership and filter-mode information. | Hosts, routers (v3)   | RFC 3376 core format[7][9][15] | Normative                                    | Parsing logic must handle variable-length source lists and record types.     | High      |
| IGMP-REQ-014    | IGMPv3 routers must merge overlapping INCLUDE/EXCLUDE states from multiple hosts to compute forwarding state.       | Routers (v3)            | RFC 3376 behavioral sections[7][15] | Normative                                   | Router state machine must support complex merging; failure leads to over/under-delivery. | High      |</p>
<p>Unspecified or router-specific behaviors not clearly defined in the RFCs are treated as implementation-dependent and are captured as assumed or unverified where appropriate.</p>
<hr />
<h2 id="5engineeringmodel">5. Engineering Model</h2>
<h3 id="51coreobjectsandrelationships">5.1 Core Objects and Relationships</h3>
<ol>
<li>Host: IPv4 system that joins multicast groups and sends IGMP reports to adjacent multicast routers.[2][4][7][8][13]  </li>
<li>Router (multicast-enabled): Device that sends IGMP queries, processes reports, maintains per-interface group/source state, and interacts with multicast routing protocols.[2][4][7][12][14]  </li>
<li>Multicast Group (G): IPv4 multicast address (224.0.0.0–239.255.255.255) representing a logical multicast channel.[2][6][13]  </li>
<li>Source (S): IPv4 unicast address of a sender; IGMPv3 supports source-filtering for flows (S,G).[7][9][15]  </li>
<li>IGMP Message Types:</li>
</ol>
<ul>
<li>Membership Query (General, Group-Specific, Group-and-Source-Specific).[2][4][7][15]</li>
<li>Membership Report (v1/v2 single-group, v3 multi-record).[2][4][7][8][15]</li>
<li>Leave Group (IGMPv2 only).[4][10]  </li>
</ul>
<h3 id="52controlflowsemantics">5.2 Control-Flow Semantics</h3>
<p>High-level IGMP behavior on a subnet:</p>
<ol>
<li>Router periodically sends General Queries to 224.0.0.1 with TTL = 1 to discover membership for all groups.[2][4][5][8]  </li>
<li>Hosts with multicast group memberships start timers and respond with Membership Reports; report suppression reduces duplicate reports.[2][4][5][8]  </li>
<li>When a host joins a new group, it sends an unsolicited Membership Report to accelerate convergence.[4][7][15]  </li>
<li>In IGMPv2, when a host leaves a group, it sends a Leave Group message to the all-routers group; routers then send Group-Specific Queries for that group.[4][10]  </li>
<li>In IGMPv3, hosts and routers exchange Membership Reports containing Group Records with INCLUDE/EXCLUDE source lists and filter modes.[7][9][15]  </li>
<li>Routers feed IGMP-derived local membership state into multicast routing protocols (e.g., PIM join/prune) to establish or tear down multicast forwarding trees.[5][6][14]</li>
</ol>
<h3 id="53timingflowsemantics">5.3 Timing-Flow Semantics</h3>
<p>Timers described in RFC 2236 and RFC 3376 govern:</p>
<ol>
<li>Query Interval: Period between successive General Queries.[4][7][15]  </li>
<li>Max Response Time (or Max Resp Code): Upper bound for host responses to a given Query.[4][7][15]  </li>
<li>Group Membership Interval: Time a router assumes at least one host is listening to a group without hearing further reports.[4][7][15]  </li>
<li>Other Querier Present Interval: Time a non-querier router waits before declaring the querier down.[4][7][15]  </li>
<li>Last Member Query Interval and Count: How routers confirm final departure from a group after Leave or source-change reports.[4][7][15]</li>
</ol>
<h3 id="54versionbehaviorboundary">5.4 Version Behavior Boundary</h3>
<ol>
<li>IGMPv1: Simple join/report model; no explicit Leave; router infers leave on timeout.[2][5][8]  </li>
<li>IGMPv2: Adds Leave Group and improved querier election and timers.[4][10]  </li>
<li>IGMPv3: Adds source filtering, Group Record formats, and extensive timer formulas.[7][9][15]</li>
</ol>
<p>Operational policy (e.g., how often to query, whether to accept fast-leave from access devices, how to aggregate per-host state) is implementation-dependent and often guided by vendor documentation rather than RFCs.[1][14]</p>
<h3 id="55mermaidcontrolflowdiagram">5.5 Mermaid Control-Flow Diagram</h3>
<pre><code class="mermaid language-mermaid">flowchart TD
    RouterQuery[Router sends General Query (224.0.0.1, TTL=1)] --&gt; HostTimer[Hosts start response timers]
    HostTimer --&gt; HostReport[Hosts send Membership Reports (suppression applies)]
    HostJoin[Host joins group G] --&gt; HostReport
    HostLeave[Host leaves group G (v2/v3)] --&gt; LeaveOrUpdate[Leave Group (v2) or updated Report (v3)]
    LeaveOrUpdate --&gt; RouterSpecificQuery[Router sends Group-/Source-Specific Query]
    RouterSpecificQuery --&gt; FinalState[Router updates group/source state and routing]
</code></pre>
<p>This diagram reflects normative flows from the RFCs, abstracting vendor-specific optimizations.[2][4][7][10][15]</p>
<hr />
<h2 id="6formulascalculationsandworkedexamples">6. Formulas, Calculations, and Worked Examples</h2>
<p>Normative timer formulas are primarily defined in RFC 2236 (IGMPv2) and RFC 3376 (IGMPv3).[4][7][15] Only formulas with strong evidence from those RFCs are marked normative.</p>
<h3 id="61keytimingformulasigmpv2igmpv3">6.1 Key Timing Formulas (IGMPv2 / IGMPv3)</h3>
<ol>
<li><p><strong>Group Membership Interval</strong><br />
The Group Membership Interval is the time a router assumes there is at least one member of a group after not hearing any reports.[4][7][15]<br />
Normative formula (IGMPv2/IGMPv3):</p>
<p>[
\text{GroupMembershipInterval} = (\text{RobustnessVariable} \times \text{QueryInterval}) + \text{MaxResponseTime}
][7][15]</p></li>
</ol>
<ul>
<li>RobustnessVariable: Small integer reflecting expected packet loss; default typically 2 (normative default in IGMPv3).[7][15]  </li>
<li>QueryInterval: Period between General Queries (seconds).[4][7][15]  </li>
<li>MaxResponseTime: Maximum time allowed for responses to a Query (seconds).[4][7][15]  </li>
</ul>
<ol>
<li><p><strong>Other Querier Present Interval</strong><br />
The Other Querier Present Interval is the time a router waits after hearing a Query from another router before transitioning to querier role.[4][7][15]<br />
Normative formula (IGMPv2/IGMPv3):</p>
<p>[
\text{OtherQuerierPresentInterval} = (\text{RobustnessVariable} \times \text{QueryInterval}) + \frac{\text{MaxResponseTime}}{2}
][7][15]</p></li>
<li><p><strong>Last Member Query Interval and Count</strong><br />
IGMPv2/IGMPv3 define Last Member Query Interval and Last Member Query Count, used to confirm a group is empty after a Leave or source-change.[4][7][15]<br />
Common relationship:</p>
<p>[
\text{LastMemberQueryTime} = \text{LastMemberQueryInterval} \times \text{LastMemberQueryCount}
][7][15]</p>
<p>This expression is considered normative in IGMPv3, which defines these parameters and their use in confirming membership.[7][15]</p></li>
</ol>
<h3 id="62defaultparametervaluesnormativeassumed">6.2 Default Parameter Values (Normative / Assumed)</h3>
<p>Exact default values are specified in RFC 3376; the following are widely cited default settings.[7][12][15]</p>
<ul>
<li>RobustnessVariable (IGMPv3): 2 (normative default).[7][15]  </li>
<li>QueryInterval: commonly 125 seconds (normative default in IGMPv3).[7][15]  </li>
<li>MaxResponseTime: commonly 10 seconds (normative default in IGMPv3).[7][15]  </li>
</ul>
<p>For IGMPv2, defaults are similar but exact values may differ slightly depending on implementation; these are treated as assumed when not explicitly stated in the RFC.[4][10][14]</p>
<h3 id="63workedexamplegroupmembershipintervaligmpv3">6.3 Worked Example – Group Membership Interval (IGMPv3)</h3>
<p>Assume normative defaults from IGMPv3:[7][15]</p>
<ul>
<li>RobustnessVariable = 2  </li>
<li>QueryInterval = 125 s  </li>
<li>MaxResponseTime = 10 s  </li>
</ul>
<p>Compute Group Membership Interval:</p>
<p>[
\text{GroupMembershipInterval} = (2 \times 125) + 10 = 250 + 10 = 260 \text{ s}
][7][15]</p>
<p>Interpretation for broadcast engineering:</p>
<ul>
<li>If no host reports are heard for 260 seconds, the router may remove forwarding state for the group on that interface.[7][15]  </li>
<li>This affects how quickly a channel is considered inactive when STBs are turned off without explicit leave (especially in IGMPv1).[2][4][7][10][15]</li>
</ul>
<h3 id="64workedexampleotherquerierpresentinterval">6.4 Worked Example – Other Querier Present Interval</h3>
<p>Using the same defaults:</p>
<p>[
\text{OtherQuerierPresentInterval} = (2 \times 125) + \frac{10}{2} = 250 + 5 = 255 \text{ s}
][7][15]</p>
<p>Meaning:</p>
<ul>
<li>A non-querier router waits ~255 seconds after receiving a Query from the current querier before assuming the querier is lost and becoming the querier itself.[4][7][15]  </li>
<li>This impacts redundancy and failover behavior in access and distribution networks.</li>
</ul>
<h3 id="65formulaandassumptionregister">6.5 Formula and Assumption Register</h3>
<p>| Calculation name               | Formula or method                                                                                                      | Inputs and units                                  | Source citation       | Normative or assumed        | Worked example available | Confidence |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|-----------------------|-----------------------------|--------------------------|-----------|
| Group Membership Interval      | ( \text{GMI} = (\text{RobustnessVariable} \times \text{QueryInterval}) + \text{MaxResponseTime} )                  | RobustnessVariable (unitless), QueryInterval (s), MaxResponseTime (s) | RFC 3376 core timers[7][15] | Normative (IGMPv2/IGMPv3) | Yes                      | High      |
| Other Querier Present Interval | ( \text{OQPI} = (\text{RobustnessVariable} \times \text{QueryInterval}) + \frac{\text{MaxResponseTime}}{2} )       | Same as above                                      | RFC 3376 timers[7][15] | Normative                    | Yes                      | High      |
| Last Member Query Time         | ( \text{LMQT} = \text{LastMemberQueryInterval} \times \text{LastMemberQueryCount} )                                | Interval (s), Count (unitless)                    | RFC 3376 timers[7][15] | Normative                    | Yes                      | High      |
| IGMPv3 default QueryInterval   | Implementation chooses value, default commonly 125 s; used directly in formulas above                                 | QueryInterval (s)                                  | RFC 3376 default values[7][15] | Normative default         | Yes (examples)           | High      |
| IGMPv3 default RobustnessVariable | Implementation chooses value, default commonly 2                                 | RobustnessVariable (unitless)                     | RFC 3376 defaults[7][15] | Normative default           | Yes                      | High      |
| IGMPv3 default MaxResponseTime | Implementation chooses value, default commonly 10 s                               | MaxResponseTime (s)                                | RFC 3376 defaults[7][15] | Normative default           | Yes                      | High      |</p>
<p>Any additional formulas not present in RFCs or strongly evidenced by secondary material are considered Unverified and excluded from this catalog.</p>
<hr />
<h2 id="7interoperabilityrisksandambiguityregister">7. Interoperability Risks and Ambiguity Register</h2>
<h3 id="71narrativerisks">7.1 Narrative Risks</h3>
<ol>
<li>Version Interoperability: Mixed IGMPv1/v2/v3 networks may misinterpret queries if version detection based on message length and Max Resp Code is not correctly implemented.[7][15]  </li>
<li>Timer Misconfiguration: Non-standard QueryInterval, RobustnessVariable, and MaxResponseTime values can cause slow convergence or excessive control traffic.[4][7][15]  </li>
<li>Source-Filtering Semantics: IGMPv3 INCLUDE/EXCLUDE behavior is complex; mis-implementation can lead to unwanted streams being delivered or desired streams being blocked.[7][9][15]  </li>
<li>Lack of Explicit Leave (IGMPv1): IGMPv1 networks rely solely on timeouts, which may cause delayed channel teardown or unnecessary traffic.[2][5][8]  </li>
<li>Router Behavior Beyond RFC 1112: Since RFC 1112 is host-centric, router behaviors may diverge across implementations in the absence of explicit normative text.[2][8][14]</li>
</ol>
<h3 id="72riskandambiguityregistertable">7.2 Risk and Ambiguity Register Table</h3>
<p>| Risk or ambiguity                          | Evidence                                                                                         | Normative status         | Failure symptom                                                   | Mitigation or modeling rule                                        |
|--------------------------------------------|--------------------------------------------------------------------------------------------------|--------------------------|-------------------------------------------------------------------|--------------------------------------------------------------------|
| Mixed-version IGMP query interpretation     | RFC 3376 defines version detection based on length and Max Resp Code; mis-parsing risks exist.[7][15] | Normative (rule exists); risk from non-compliance | Hosts ignore queries or respond incorrectly; groups appear inactive. | Implement strict version detection per RFC 3376; validate via capture tests. |
| Timer misconfiguration                      | RFC 2236 and RFC 3376 define formulas and defaults, but implementations can change parameters.[4][7][15] | Normative formulas; implementation policy | Slow leave convergence, query storms, or premature group timeout.   | Use RFC defaults as baseline; adjust via controlled testing; document deviations. |
| Source-filtering misinterpretation          | IGMPv3 introduces INCLUDE/EXCLUDE modes and complex merging rules.[7][9][15]                     | Normative semantics; ambiguous implementations | Incorrect delivery (wrong sources); user-visible wrong channel or missing program. | Rigorously test INCLUDE/EXCLUDE cases; ensure router and host alignment; consider SSM profiles. |
| IGMPv1 lack of Leave                        | RFC 1112 does not define Leave messages; relies on query/timeout only.[2][5][8]                 | Normative (by omission)  | Slow teardown; stale multicast flows continue to be forwarded.     | Prefer IGMPv2/v3; or reduce Group Membership Interval while monitoring for packet loss. |
| Router behavior not fully specified in RFC 1112 | RFC 1112 states IGMP is specified from the host perspective; router specifics not detailed.[2][8] | Normative scope; ambiguous router detail | Vendor differences in querier election, state aging, group limits. | Depend on multicast routing specs and vendor guides; treat as implementation-dependent. |</p>
<hr />
<h2 id="8implementationguidance">8. Implementation Guidance</h2>
<p>This section provides best-practice guidance for broadcast engineering, explicitly labeled as non-normative unless directly tied to RFC rules.</p>
<h3 id="81hoststackguidancebestpractice">8.1 Host Stack Guidance (Best Practice)</h3>
<ol>
<li>Implement full IGMPv3 functionality, including INCLUDE/EXCLUDE modes and source lists, to support both ASM and SSM use-cases in modern broadcast networks.[7][9][12][15]  </li>
<li>Expose host APIs that allow applications (STBs, encoders, monitoring tools) to specify (S,G) joins and filter modes rather than group-only joins.[7][9][15]  </li>
<li>Ensure IGMP behavior is robust against packet loss by using the default RobustnessVariable = 2 unless reliability constraints justify changes.[7][15]  </li>
</ol>
<h3 id="82routernetworkguidancebestpractice">8.2 Router/Network Guidance (Best Practice)</h3>
<ol>
<li>Use IGMPv3 on access interfaces and support IGMPv2/IGMPv1 fallback for legacy devices; follow RFC 3376 rules for query version detection.[7][12][15]  </li>
<li>Apply RFC-default timer values as a safe baseline, adjusting carefully for broadcast requirements:</li>
</ol>
<ul>
<li>QueryInterval ≈ 125 s.[7][15]</li>
<li>RobustnessVariable = 2.[7][15]</li>
<li>MaxResponseTime ≈ 10 s.[7][15]</li>
</ul>
<ol>
<li>Enable fast-leave behavior only when downstream devices cannot host multiple receivers per port, and ensure Last Member Query parameters are tuned to avoid false negatives.[4][7][14][15]  </li>
<li>Separate IGMP control-plane multicast from content multicast in monitoring and capacity planning (IGMP traffic is low-bandwidth but latency sensitive).[5][6][13][14]</li>
</ol>
<h3 id="83loggingandmonitoringguidancebestpractice">8.3 Logging and Monitoring Guidance (Best Practice)</h3>
<p>Capture at least the following fields for IGMP monitoring:</p>
<ul>
<li>IGMP version (derived via length and Max Resp Code).[7][15]  </li>
<li>Message type (Query, Report, Leave Group, Group Record types).[2][4][7][10][15]  </li>
<li>Group address (G) and, for v3, source list (S).[7][9][15]  </li>
<li>Filter mode (INCLUDE/EXCLUDE for v3).[7][15]  </li>
<li>Interface identifier and router role (querier/non-querier).[4][7][14]  </li>
<li>Calculated timer values (Group Membership Interval, Other Querier Present Interval).[4][7][15]</li>
</ul>
<p>These are implementation recommendations, not normative requirements.</p>
<hr />
<h2 id="9validationchecklist">9. Validation Checklist</h2>
<p>The following checklist can be used to validate IGMP behavior in a broadcast network implementation. Items referencing RFCs are derived from normative requirements; others are best-practice checks.</p>
<ol>
<li>Confirm IGMP messages use IP protocol number 2 and TTL = 1 on local subnet.[2][5][8]  </li>
<li>Verify routers send General Queries to 224.0.0.1 at the configured QueryInterval.[2][4][5][8]  </li>
<li>Check that hosts respond to Queries with Membership Reports, with report suppression when other reports are observed.[2][4][5][8]  </li>
<li>In IGMPv2, verify Leave Group messages are sent when hosts leave groups and routers send Group-Specific Queries in response.[4][10]  </li>
<li>In IGMPv3, confirm Membership Reports contain correctly formed Group Records with INCLUDE/EXCLUDE modes and accurate source lists.[7][9][15]  </li>
<li>Validate that routers correctly detect IGMP query version based on message length and Max Resp Code.[7][15]  </li>
<li>Compute Group Membership Interval, Other Querier Present Interval, and Last Member Query Time from configured parameters and verify router behavior matches these values.[4][7][15]  </li>
<li>Test mixed-version environments (v1, v2, v3) to ensure hosts and routers interoperate without misinterpreting queries or reports.[4][7][12][15]  </li>
<li>Ensure multicast routing protocol state (e.g., PIM) reflects IGMP-derived membership and that changes in IGMP state propagate promptly to routing.[5][6][14]  </li>
<li>Monitor for stale multicast flows (traffic with no receivers) and correlate with IGMP timers and configuration to identify mis-tuning.[4][7][14]</li>
</ol>
<hr />
<h2 id="10openquestionsunverifieditems">10. Open Questions / Unverified Items</h2>
<p>Items in this section are explicitly Unverified due to lack of direct clause-level evidence in the referenced materials or uncertainty in remembered details.</p>
<ol>
<li>Exact numeric default timer values for IGMPv2 (QueryInterval, MaxResponseTime, RobustnessVariable) beyond those implied as similar to IGMPv3 are Unverified; only IGMPv3 defaults are treated as normative.[4][10]  </li>
<li>Specific section numbers for IGMPv2 and IGMPv3 timer formulas are not cited here; while formulas are widely documented, clause references are Unverified in this report.[4][7][15]  </li>
<li>Detailed router querier election algorithm (beyond basic rules implied by timers) may be further specified in routing-protocol RFCs; their exact interactions are Unverified within this IGMP-only scope.[4][7]  </li>
<li>Any vendor-specific extensions (e.g., proprietary IGMP snooping enhancements, custom fast-leave mechanisms) are out of scope and Unverified.[1][14]  </li>
<li>A complete list of IGMPv3 Group Record types and their precise encoding rules is not reproduced here and remains Unverified at clause-level detail, though the existence of Group Records and source lists is normative.[7][9][15]</li>
</ol>
<hr />
<h2 id="11sources">11. Sources</h2>
<p>Primary normative sources (high confidence):</p>
<ol>
<li>RFC 1112 – “Host Extensions for IP Multicasting” (STD 5), August 1989. Defines IPv4 multicast host behavior and IGMPv1 from the host perspective.[2][6][8][14]  </li>
<li>RFC 2236 – “Internet Group Management Protocol, Version 2”, November 1997. Updates RFC 1112; specifies IGMPv2, including Leave Group and improved timers.[3][4][10][11]  </li>
<li>RFC 3376 – “Internet Group Management Protocol, Version 3”, October 2002. Obsoletes RFC 2236; defines IGMPv3, including source-filtered multicast and detailed timer formulas.[7][12][15]</li>
</ol>
<p>Secondary descriptive and implementation-guidance sources (non-normative):</p>
<ol start="4">
<li>Cisco “IGMPv3 Host Stack” documentation – describes IGMPv3 host operation and references RFC 3376.[1]  </li>
<li>Columbia University IGMP overview – educational summary of IGMP versions and basic router behavior.[5]  </li>
<li>O’Reilly “Internet Core Protocols: The Definitive Guide”, Chapter on IP multicasting – contextualizes RFC 1112 and IGMP evolution.[6]  </li>
<li>NISP IGMPv3 profile summary – describes IGMPv3 role and states that it obsoletes RFC 2236.[12]  </li>
<li>IONOS IGMP article – accessible explanation of IGMP, multicast groups, and historical context.[13]  </li>
<li>Nokia multicast documentation (IGMP chapter) – vendor-specific guidance on IGMP version usage and implementation details.[14]  </li>
<li>Russian and Japanese expositions and translations of RFC 2236 and RFC 3376 – provide accessible summaries of IGMPv2 and IGMPv3 behaviors, including query version rules.[3][9][10][15]</li>
</ol>
<p>All normative technical requirements and formulas in this report are drawn from the primary RFCs, with secondary sources used only to corroborate or explain behavior.</p>