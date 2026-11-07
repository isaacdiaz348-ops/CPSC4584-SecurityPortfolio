# Week 10: Suspicious Executable Running on a Server
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** November 2, 2026
**Analyst:** [Your Name]
**Case ID:** EXEC-2026-1030-001

---

## Incident Summary

[One to two sentences describing the mhs_monitor discovery, the EHR server context, and the connection to the Week 7 network anomaly.]

---

## Static Analysis Findings

| # | Tool | Output Line | Finding | Severity |
|---|------|-------------|---------|----------|
| 1 | strings | 185.220.101.47 | [Your finding] | [Your severity assessment] |
| 2 | strings | /tmp/.mhs_session | [Your finding] | [Your severity assessment] |
| 3 | strings | Mozilla/5.0 (compatible; MHSMonitor/1.0) | [Your finding] | [Your severity assessment] |
| 4 | readelf | [NEEDED] libcurl.so.4 | [Your finding] | [Your severity assessment] |

---

## Process-Level IOCs

| IOC | Observed Value | Significance |
|-----|----------------|--------------|
| Not in inventory | No package manager entry | [Explain the significance.] |
| Parent process | init (PID 1) | [Explain what this does and does not establish.] |
| Repeated outbound connection | 185.220.101.47 every 4 min | [Explain the beacon-like pattern and its limitation.] |
| Dot-prefixed file | /tmp/.mhs_session | [Explain why this artifact requires review.] |

---

## Dynamic Analysis Assessment

[Explain why normal outbound connectivity is inappropriate for an unknown sample and describe the controlled, isolated environment required for authorized dynamic analysis.]

---

## Escalation Summary

[Summarize the strongest evidence, the Week 7 correlation, the EHR server significance, the evidence that should be preserved, and the containment decisions that belong to the incident response team.]

---
*CPSC 4584 | Governors State University | Fall 2026*
    
