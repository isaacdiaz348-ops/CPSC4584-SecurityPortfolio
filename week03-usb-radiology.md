# Week 3: Unauthorized USB Drive in Radiology
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** September 14, 2026
**Analyst:** [Isaac Diaz]
**Incident ID:** INC-2026-0914-001

---

## Incident Summary

[At 11:47 AM on Saturday, September 13, 2026, a Maplewood facilities technician discovered an unmarked USB drive plugged into workstation MHS-RAD-WS-03 during a routine equipment inspection in the Radiology imaging suite. The workstation is used by radiology technologists to access the PACS imaging system and the Maplewood EHR. Access to the Radiology suite is restricted to credentialed clinical staff and approved vendors only.]

---

## Chain of Custody

[Chain of Custody matters in this investigation to ensure evidence integrity and anuthentication, prevents the USB from being contaminated, and supports legal action. In this investiagtion, it was maintained by having a Chain of Custody log and logging when the USB was discovered, when the device was received, logged, and tagged, when the escalation was recieved by a SOC Tier 1 Analyst, and when the investiagtion started.]

---

## Key Encoding Finding

**String Found:** Y3VybCAtcyAtbyAvZGV2L251bGw=
**Encoding Type:** [Base64]
**Decoded Content:** [curl -s -o /dev/null
**Significance:** ["curl" command which is used to make silent network requests without saving files or printing any output.This is significant because there could be something covert that is extracting or inputing files into the workstation.]

---

## Terminal Commands Used

| Command | Purpose |
|---------|---------|
| echo "..." \| base64 | [Base64 uses upper and lowercase letters, numbers 0-9, and often ends a string with one or two "=" signs.] |
| echo "..." \| base64 -d | [The decoding revealed curl -s -o /dev/null]  |
| xxd .bashrc \| head -6 | [The hex dump showed the starting file position, then it shows the raw decimal byte, and finally shows
the text representation in ASCII representation.] |
| strings .bashrc \| grep -i "..." | [Pattern filtering narrows the output by stripping away binary clutter and system noise.] |

---

## Escalation Recommendation

[Yes, this incident should be escalated and the strongest evidence that supports my decision is that we do not know who plugged the USB
into the workstation and what is exactly on that USB.]

---
*CPSC 4584 | Governors State University | Fall 2026*
    
