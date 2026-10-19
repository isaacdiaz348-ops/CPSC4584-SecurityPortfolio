# Week 7: Network Traffic Anomaly During Night Shift
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** October 12, 2026
**Analyst:** [Isaac Diaz]
**Case ID:** NET-2026-1012-001 (related: FOR-2026-1005-001)

---

## Incident Summary

[An outbound network anomaly captured a 2,457,814-byte transfer from Marcus Webb's laptop to an unauthorized external IP address starting at 11:48 PM on October 3. This closely aligns with Week 6 disk evidence showing that the ~2.4 MB INSURANCE_EXPORT_FINAL.XLSX file was modified at 11:47 PM and deleted at 11:52 PM.]

---

## Network Flow Analysis

| Flow | Source | Destination | Classification | Finding |
|------|--------|-------------|----------------|---------|
| 1 | Webb laptop | 185.220.101.47 | [Unauthorized Outbound Traffic] | [Flow 1 supports the hypothesis of data exfiltration by capturing an unauthorized outbound transfer whose size and timing closely match the lifecycle of the deleted INSURANCE_EXPORT_FINAL.XLSX file.] |
| 2 | Webb laptop | 185.220.101.47 | [Beacon-Like Traffic Pattern] | [Flow 2 supports the beaconing hypothesis due to the strict four-minute cadence and uniform 312-byte payload size, but it cannot be definitively proven as command-and-control without application payloads or EDR process execution logs.] |
| 3 | EHR prod server | AWS | [Authorized Enterprise Backup] | [Flow 3 represents expected, baseline operational traffic because it captures the organization's scheduled nightly EHR database backup safely transmitting to the authorized AWS vendor destination.] |
| 4 | Radiology WS | 8.8.8.8 | [Anomalous DNS Volume] | [Flow 4 is considered unusual because the radiology workstation generated an unexpected spike of 23 DNS queries totaling 847 bytes in a brief eight-minute window, which deviates from standard clinical device network behavior and warrants review of internal DNS logs for potential tunneling or unauthorized lookups.] |
| 5 | Billing WS | 203.0.113.99 | [Unapproved Periodic HTTP Connection] | [Flow 5's nine repeated, identically sized HTTP connections occurring at exact four-minute intervals mimic the regular cadence and automated nature of beaconing behavior, but they do not prove command-and-control activity because network flow metadata lacks application payloads and process execution context that would confirm malicious intent rather than a legitimate scheduled application utility.] |

---

## Disk and Network Evidence Correlation

**Disk (Week 6):** [The insurance_export_final.xlsx was last modified at 11:47pm on October 3rd and deleted from the device at 11:52pm.]
**Network (Week 7):** [An outbound network flow transferred 2,457,814 bytes from Marcus Webb's laptop to unauthorized IP address 185.220.101.47 beginning at 11:48 PM on October 3.]
**Correlation:** [The tight operational window—where the file transfer begins one minute after modification and finishes just before file deletion, alongside matching byte sizes—strongly suggests data staging, exfiltration, and anti-forensic cleanup.]
**Limitation:** [Flow records only summarize connection metadata and lack full application payloads or packet captures, meaning they cannot definitively prove that the contents of the deleted spreadsheet itself were transmitted.]

---

## Network Artifact Inspection Commands Used

| Command | Purpose |
|---------|---------|
| curl -I https://example.com | [Observed HTTP/2 response headers including a 200 OK status, server: cloudflare, content-type: text/html, and cache control metadata, which allow investigators to efficiently inspect server software, caching states, and resource availability without downloading the response body.] |
| curl -v --max-time 5 | [Observed verbose connection telemetry including TCP/IP connection establishment to the target IP on port 443, ALPN protocol negotiation for HTTP/2 (h2) and HTTP/1.1, certificate validation against system trust stores, a modern TLS 1.3 cryptographic handshake, the negotiated cipher suite (TLS_AES_256_GCM_SHA384), and a 5-second timeout constraint limiting maximum connection wait time.] |
| curl -s -o + xxd | [Observed the raw initial bytes of the application-layer payload via hex translation, which revealed standard, human-readable HTML markup beginning with <!doctype html><html lang=en>, demonstrating that text-based formats start with readable tags rather than fixed binary magic numbers.] |
| curl -s -o /dev/null -w | [Observed session metrics such as the destination IP address, port number, and total transfer size in bytes that directly overlap with core network flow record fields (destination IP, port, and byte-count summaries), allowing analysts to bridge active command-line telemetry with passive infrastructure flow data.] |

---

## Escalation Summary

[Disk Lifecycles: The file INSURANCE_EXPORT_FINAL.XLSX around 2.4 megabytes was modified at 11:47 PM and deleted at 11:52 PM on October 3, 2026, on Marcus Webb laptop at IP address 10.4.2.47.

Network Flow Metadata: Precise capture of connection timestamps, source and destination IP addresses, ports, and byte counts across all five flows, including the authorized EHR backup from server 10.4.1.10 to AWS.

Anomalous Device Traffic: Documented external connections from Webb laptop to 185.220.101.47, a DNS spike from radiology workstation 10.4.3.22, and repeated HTTP requests from billing workstation 10.4.3.88.

Supported Correlations

Data Staging and Exfiltration Window: Flow 1 recorded an outbound transfer of 2,457,814 bytes to 185.220.101.47 from 11:48 PM to 11:50 PM, aligning within a tight 5-minute window of the file modification and subsequent deletion.

Beaconing Pattern: Flow 2 captured uniform 312-byte connections occurring at exact four-minute intervals from 11:15 PM through 6:42 AM, beginning 33 minutes prior to the large data transfer.

Threat Intelligence Proximity: Destination IP 185.220.101.47 resides in the same slash-24 subnet as infrastructure used in the previous billing portal credential stuffing incident INC-2026-0928-001, providing a valid investigative lead.

Unresolved Questions

Payload Contents: Flow records lack packet captures and full payload visibility, leaving the exact contents of the 2.45 MB transfer and 312-byte beacon packets unverified.

Process Attribution: The specific application, script, or executable that initiated the outbound flows on Webb laptop, the radiology workstation, and the billing workstation remains unknown without endpoint execution logs.

Exfiltration Verification: While size and timing match, it remains unproven whether INSURANCE_EXPORT_FINAL.XLSX was successfully exfiltrated or merely staged and compressed.

Actor Attribution Context: Whether the shared subnet ownership points to the same threat actor or merely shared proxy and hosting infrastructure has not been confirmed.

Leadership Decisions

[Threat Actor Attribution: Deciding whether to formally attribute current network anomalies to the actor from incident INC-2026-0928-001, exercising caution since shared subnets are correlation points rather than proof.

Legal and Regulatory Notification: Authorizing engagement with legal counsel, compliance officers, and potential breach notification processes regarding the sensitive insurance data.

Disruptive Containment Actions: Approving enterprise-wide isolation of clinical network segments, asset confiscation, or firewall blocking that could impact active patient care workflows.]

---
*CPSC 4584 | Governors State University | Fall 2026*
    
