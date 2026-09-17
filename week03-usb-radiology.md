# Week 3: Unauthorized USB Drive in Radiology
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** September 14, 2026
**Analyst:** [Your Name]
**Incident ID:** INC-2026-0914-001

---

## Incident Summary

[One to two sentences describing what was found and where.]

---

## Chain of Custody

[One to two sentences explaining why chain of custody matters in this investigation and how it was maintained.]

---

## Key Encoding Finding

**String Found:** Y3VybCAtcyAtbyAvZGV2L251bGw=
**Encoding Type:** [Identified encoding type]
**Decoded Content:** [What the string decoded to]
**Significance:** [What this finding means for the investigation]

---

## Terminal Commands Used

| Command | Purpose |
|---------|---------|
| echo "..." \| base64 | [Explain what you learned about Base64 representation.] |
| echo "..." \| base64 -d | [Explain what decoding revealed.] |
| xxd .bashrc \| head -6 | [Explain what the hex dump showed.] |
| strings .bashrc \| grep -i "..." | [Explain how pattern filtering narrowed the output.] |

---

## Escalation Recommendation

[State whether you would escalate, identify the strongest evidence, and note at least one question that still requires deeper investigation.]

---
*CPSC 4584 | Governors State University | Fall 2026*
    
