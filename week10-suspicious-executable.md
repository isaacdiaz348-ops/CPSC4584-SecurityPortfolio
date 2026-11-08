# Week 10: Suspicious Executable Running on a Server
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** November 2, 2026
**Analyst:** [Your Name]
**Case ID:** EXEC-2026-1030-001

---

## Incident Summary

[During routine maintenance on October 30, 2026, an unauthorized binary named mhs_monitor was discovered running continuously under init (PID 1) on the Maplewood EHR application server. Static analysis revealed hardcoded indicators—including a connection to the known Week 7 external destination (185.220.101.47) and system execution primitives—confirming an active, covert backdoor operating on infrastructure that handles sensitive patient data.]

---

## Static Analysis Findings

| # | Tool | Output Line | Finding | Severity |
|---|------|-------------|---------|----------|
| 1 | strings | 185.220.101.47 | [Hardcoded external C2 destination IP matching Week 7 network anomaly indicators] | [Critical] |
| 2 | strings | /tmp/.mhs_session | [Hidden temporary file path utilized for data staging or session state persistence.] | [High] |
| 3 | strings | Mozilla/5.0 (compatible; MHSMonitor/1.0) | [Custom spoofed User-Agent header designed to disguise beacon traffic as legitimate browser activity] | [High] |
| 4 | readelf | [NEEDED] libcurl.so.4 | [Dynamic library dependency enabling programmatic HTTP/HTTPS network requests and data exfiltration] | [High] |

---

## Process-Level IOCs

| IOC | Observed Value | Significance |
|-----|----------------|--------------|
| Not in inventory | No package manager entry | [Indicates the binary is unmanaged and unauthorized by system packaging tools, bypassing standard integrity tracking and update channels.] |
| Parent process | init (PID 1) | [Establishes top-level system startup execution and persistence across reboots, but does not prove whether the process was spawned legitimately during boot or injected via post-compromise manipulation.] |
| Repeated outbound connection | 185.220.101.47 every 4 min | [Demonstrates a consistent beacon-like communication pattern typical of command-and-control (C2) check-ins, though network timing alone cannot confirm whether payload data was successfully exfiltrated without packet payload capture.] |
| Dot-prefixed file | /tmp/.mhs_session | [Highlights hidden file storage designed to evade casual directory listings, requiring review to determine if it houses active session state, exfiltration staging data, or logs.] |

---

## Dynamic Analysis Assessment

[Connecting an unknown malware sample to a live network risks uncontrolled command-and-control communication, data exfiltration, and system compromise. To safely observe its behavior without endangering production infrastructure, dynamic analysis must be conducted in an isolated, air-gapped sandbox using simulated network services and strict behavioral monitoring.]

---

## Escalation Summary

Strongest Evidence

Static and process-level analysis of mhs_monitor revealed multiple high-confidence indicators of malicious utility:

Hardcoded C2 IP Address: 185.220.101.47 embedded directly within the binary payload.

Covert Data Staging Path: Hidden file storage designated at /tmp/.mhs_session.

Spoofed User-Agent String: Mozilla/5.0 (compatible; MHSMonitor/1.0) configured to disguise network requests as legitimate HTTP traffic.

Network & Execution Primitives: Dynamic reliance on libcurl.so.4 alongside process and file I/O primitives (system, fwrite).

Unauthorized Persistence: Running continuously under init (PID 1) for 72+ hours without a package manager inventory record.

Correlation with Week 7 Anomaly

The destination IP 185.220.101.47 correlates directly with the network telemetry anomalies identified in Week 7. This correlation indicates that mhs_monitor is the persistent host component responsible for initiating regular 4-minute outbound beaconing cycles to established external command-and-control infrastructure.

EHR Server Significance

The target system is an active Electronic Health Record (EHR) application server hosting sensitive Patient Health Information (PHI). The execution of an unauthorized backdoor with root-level process parentage on this node poses an immediate risk of regulatory breach, unauthorized access, and mass exfiltration of protected healthcare data.

Critical Evidence to Preserve

Prior to initiating remediation, the following forensically volatile artifacts must be secured:

Volatile RAM Image: Full memory dump of the active system to preserve unencrypted payload structures, active sockets, and encryption keys.

Binary & Staging File Samples: Cryptographically hashed (SHA-256) copies of /path/to/mhs_monitor and /tmp/.mhs_session.

Network & System Logs: Raw packet captures, firewall logs, web server logs, and system audit logs (/var/log/audit/audit.log, /var/log/secure, kernel execution logs).

Process & Socket States: Complete output tables from ps -eo pid,ppid,user,args and active socket connections via ss -antup.

Incident Response Containment Authority

The following operational decisions strictly belong to the formal Incident Response (IR) Team and leadership:

Host Isolation Strategy: Deciding whether to sever network connectivity immediately (via interface shutdown or VLAN isolation) versus maintaining live monitoring to preserve dynamic network artifacts.

EHR Service Availability: Determining whether to bring the EHR application offline or transition to failover nodes, balancing patient care continuity against containment risks.

Remediation & Eradication Strategy: Authorizing host re-imaging, full system wipes, or credential revocation across connected directory services.

Regulatory & Legal Reporting: Escalating findings to legal compliance teams to assess mandatory HIPAA / PHI breach notification requirements.
---
*CPSC 4584 | Governors State University | Fall 2026*
    
